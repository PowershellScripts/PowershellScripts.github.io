---
layout: page
title: 'Azure Functions mit API Management absichern'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2024-12-29'
---




<h1>Design</h1>

Welche App ruft welche auf? Eines der ersten Dinge, die Sie bei der Planung Ihres API Managements (kurz APIM) berücksichtigen sollten, ist das [Design](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview#oauth-20-authorization-scenarios), dem Sie folgen möchten. Der gewählte Authentifizierungsablauf entscheidet, was für was autorisiert ist. Betrachten Sie diese zwei Szenarien:

<img src="/articles/images/SecureAzFunc/Github-SecureAzFunc1.PNG" width="600" alt="Diagramm, das die OAuth-Kommunikation zeigt, wobei die Zielgruppe das Backend ist. Diagramm, das die OAuth-Kommunikation zeigt, wobei die Zielgruppe das API Management Gateway ist.">

<sup>
Bildquelle: https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview
</sup>

Das erste Szenario ist das gängigste. Azure API Management ist ein "transparentes" Proxy zwischen dem Aufrufer und der Backend-API. Dies ist das, worauf sich die meisten Tutorials, Anleitungen und Problemlösungen beziehen. Sie können dennoch Richtlinien in APIM konfigurieren, um das Token zu validieren und andere relevante Ansprüche aus dem Token zu prüfen. Die aufrufende Anwendung fordert jedoch direkten Zugriff auf die API an. Der Gültigkeitsbereich des Zugriffstokens liegt zwischen der **aufrufenden Anwendung und der Backend-API**. 

Im zweiten Szenario handelt der API Management-Dienst im Namen der API. Der Gültigkeitsbereich des Zugriffstokens liegt zwischen der **aufrufenden Anwendung und API Management**. Dieses zweite Szenario wird normalerweise verwendet, wenn ein direkter Aufruf der Backend-API nicht möglich ist, z. B. wenn die [Backend-API OAuth nicht unterstützt](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview#audience-is-api-management).

<br/><br/>

<h1>Autorisierungsablauf</h1>

Stellen Sie sicher, dass Sie den [Autorisierungsablauf](https://learn.microsoft.com/en-us/azure/api-management/authorizations-overview#process-flow-for-runtime) verstehen, wenn Sie Ihr Azure API Management einrichten.

<img src="/articles/images/SecureAzFunc/Github-secureayfunc2.svg" width="600" alt="Diagramm, das den Prozessablauf zur Erstellung der Laufzeit zeigt.">

Die Client-App muss API Management aufrufen. Testen Sie dies. Wenn Sie Ihre Azure Function direkt aufrufen können, indem Sie nur den Funktionscode verwenden, ist diese nicht mit OAuth gesichert.


<br/><br/>
<h1>JSON Web Token (JWT)</h1>

Gemäß der OAuth-Spezifikation sind Zugriffstoken undurchsichtige Zeichenfolgen ohne festgelegtes Format.

JSON Web Tokens sind in drei Teile unterteilt:
* Header – Bietet Informationen darüber, wie das Token validiert werden soll, einschließlich Informationen über den Typ des Tokens und wie es signiert wurde.
* Payload – Enthält alle wichtigen Daten über den Benutzer oder die Anwendung, die versucht, den Dienst aufzurufen.
* Signature – Ist das Rohmaterial, das zur Validierung des Tokens verwendet wird. Jeder Abschnitt ist durch einen Punkt (.) getrennt und separat Base64-codiert.

Nützliche Ressourcen:
* [Liste von Token-Ansprüchen und deren Beschreibung](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens#claims-in-access-tokens)
* [Optionen zur Validierung eines Tokens](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens#validate-tokens)

<h3>Token-Formate</h3>

Es gibt zwei verschiedene Versionen von JSON Web Tokens (JWTs), v1.0 und v2.0, die auf der Microsoft-Identitätsplattform verfügbar sind. Entwickler können bei benutzerdefinierten APIs, die in Azure Active Directory registriert sind, zwischen diesen wählen. Von Microsoft entwickelte APIs wie Microsoft Graph oder APIs in Azure verwenden andere, proprietäre Token-Formate.

v1.0 wird standardmäßig für reine Azure AD-Anwendungen ausgewählt.

v2.0 wird standardmäßig für Anwendungen ausgewählt, die Verbraucheraccounts unterstützen.

Der Inhalt des Tokens wird nicht direkt in Anwendungen dekodiert und ist ausschließlich für die API vorgesehen. Zur Fehlerbehebung können Sie jedoch Ihre JSON Web Tokens mit den Seiten [https://jwt.ms/](https://jwt.ms/) oder [https://jwt.io/](https://jwt.io/) dekodieren.

**V1.0**

Beispiel für ein dekodiertes v1.0-Token (der Tokenwert stammt von [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens)):

<img src="/articles/images/SecureAzFunc/Github-SecureAzFunc3.PNG" width="600">

**V2.0**

Beispiel für ein dekodiertes v2.0-Token (der Tokenwert stammt von [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens)):

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc4.png" width="600">

Die Seite [https://jwt.ms/](https://jwt.ms/) hilft auch, die im Token enthaltenen Ansprüche zu interpretieren:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc5.png" width="400">

    ! Beachten Sie den Unterschied zwischen den Issuern in den beiden Versionen.
    Die Token-Version ist ein häufiges Problem bei der Konfiguration der Authentifizierung zwischen Apps!

<h3>Was bedeutet es, wenn die Token-Version falsch ist?</h3>

* Wenn es eine Diskrepanz zwischen den Issuern gibt, z. B. wenn Sie im dekodierten Token **sts.windows.net** sehen und die API Management-Richtlinie **login.microsoftonline.com** erfordert.
* Wenn Sie den Fehler "Invalid token" erhalten.

<h3>Wie kann man die Token-Version "korrigieren"?</h3>

Gehen Sie ins Azure-Portal zu Ihrer App-Registrierung. Öffnen Sie innerhalb der Azure-App-Registrierung das Manifest und setzen Sie `accessTokenAcceptedVersion` auf 2:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc6.png" width="400">

 

Es kann ein paar Minuten dauern, bis die Änderungen übernommen werden. Holen Sie sich einen Kaffee :)

<br/><br/>

<h1>Scope</h1>
Beim Festlegen der Scope für die Berechtigungen Ihrer Anwendung sollten Sie die verschiedenen Zugriffsszenarien beachten:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc7.png" width="400">
<sup>Bildquelle: https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview</sup>

Weitere Details zu delegiertem Zugriff vs. App-only-Zugriff finden Sie unter [Berechtigungen und Zustimmung](https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview).

Um es jedoch zu vereinfachen: Wenn ein Benutzer der Nutzung einer bestimmten Ressource zustimmen muss, gehen Sie hierhin im Azure-Portal:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc8.png" width="500">

Im Szenario der Client-App und der Backend-API, wie oben beschrieben, sollten Sie jedoch eher App-only-Berechtigungen verwenden und mit den Enterprise-Anwendungen beginnen, wo Sie **Zuweisung erforderlich** auf **True** setzen.

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc9.png" width="500">

Wenn diese Option auf "Ja" gesetzt ist, müssen Benutzer und andere Apps oder Dienste dieser Anwendung zuerst zugewiesen werden, bevor sie darauf zugreifen können.

<h3>Warum ist das wichtig?</h3>
Weil, wenn Sie die erforderliche Zuweisung nicht aktivieren, **jede App**-Registrierung den Standardbereich Ihrer App anfordern kann. Dies ist die Standardeinstellung. **Wenn Sie keine weiteren Prüfungen der Token-Bereiche im Code durchführen, bleibt Ihre App offen** für Aufrufe von Clients, die nicht explizit berechtigt wurden. Schauen Sie sich [diese interessanten Tests](https://medium.com/airwalk/azure-app-service-easy-auth-and-the-default-scope-1fb0b65b4d26) an, die zeigen, wie das geschieht.

<h3>Anwendung ist keiner Rolle für die Anwendung zugewiesen</h3>

Sobald Sie die Einstellung für erforderliche Zuweisungen aktiviert haben, aber bevor Sie Rollen zuweisen, sollten Sie einen *invalid_grant*-Fehler erhalten, z. B.:

    AADSTS501051: Application '1df771d6-ef9c-473e-af87-cafa8928e024'(testClient) 
    is not assigned to a role for the application '8d138478-d3a3-47f5-a233-1408cd6baae4'(testBackend2)

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc10.png" width="600">

Wenn Sie den Fehler **Anwendung ist keiner Rolle für die Anwendung zugewiesen** erhalten, gehen Sie zum Azure-Portal >> App-Registrierungen >> Wählen Sie Ihre Client-App >> API-Berechtigungen:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc11.png" width="400">

Fügen Sie die erforderliche Berechtigung hinzu und vergewissern Sie sich, dass die Administrator-Zustimmung erteilt wurde. Wenn Sie auf **Berechtigung hinzufügen** klicken, können Sie einfach zur richtigen API navigieren, indem Sie **Meine APIs** oder **APIs, die meine Organisation verwendet** auswählen:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc12.png" width="400">

Wenn Sie dort nicht die richtige API sehen, gehen Sie zum Azure-Portal >> App-Registrierungen >> Wählen Sie Ihre Backend-App (die die Azure Function repräsentiert) >> App-Rollen (für das App-only-Zugriffsszenario).


<h3>Standardbereich (Default Scope)</h3>

Der Bereich /.default hat mehrere Verwendungszwecke:
* Er dient als Abkürzung zurück zum Azure AD v1-Verhalten (z. B. statische Zustimmung).
* **Er ist erforderlich, wenn die App Dienst-zu-Dienst-Aufrufe ausführt oder ausschließlich Anwendungsberechtigungen verwendet.**
* Er ist erforderlich, wenn der On-Behalf-Of (OBO)-Flow verwendet wird, bei dem Ihre API im Namen des Benutzers Aufrufe an eine andere API tätigt, z. B. Client-App --> Ihre API --> Graph-API.

Lesen Sie den großartigen Artikel von [John Patrick Dandison](https://dev.to/jpda), der im Detail erklärt, [Was ist der /.default-Bereich in der Microsoft-Identitätsplattform & Azure AD?](https://dev.to/425show/just-what-is-the-default-scope-in-the-microsoft-identity-platform-azure-ad-2o4d). Hier wird der Unterschied zwischen statischer und dynamischer Zustimmung sehr gut und mit Beispielen beschrieben.

Für uns ist das zweite Szenario (Dienst-zu-Dienst-Aufrufe oder ausschließlich Anwendungsberechtigungen) am relevantesten. Daher wird unser Bereich so aussehen:
{BackendID}/.default.

Für eine Backend-App (unsere Azure Function) mit den folgenden Daten:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc13.png" width="400">
wird der Bereich folgendermaßen aussehen: *8d138478-d3a3-47f5-a233-1408cd6baae4/.default*

<br/><br/>

<h1>API-Management-Richtlinien</h1>

Azure API-Management-Richtlinien ermöglichen die Verwaltung hybrider, Multi-Cloud-APIs in allen Umgebungen. Als Platform-as-a-Service unterstützt API Management den gesamten API-Lebenszyklus. Die Regeln für die eingehende Verarbeitung ermöglichen Ihnen die Konfiguration einer JWT-Validierungsrichtlinie zur Vorautorisierung von Anfragen:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc14.png" width="400">

Die Validierung von JWT-Token ist eine von vielen Zugriffsbeschränkungsrichtlinien, die in API Management konfiguriert werden können. Schauen Sie sich die [API Management Zugriffsbeschränkungsrichtlinien-Dokumentation](https://learn.microsoft.com/en-us/azure/api-management/api-management-access-restriction-policies) für weitere Optionen an.

<h3>JWT-Validierungsrichtlinie</h3>

Beispiel für eine Eingangsrichtlinie:

```xml

<inbound>
    <base />
    <set-backend-service id="apim-generated-policy" backend-id="provisionierungacco" />
    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Invalid token.">
        <openid-config url="https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0/.well-known/openid-configuration" />
        <issuers>
            <issuer>https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0</issuer>
        </issuers>
        <required-claims>
            <claim name="aud" match="any">
                <value>7e5ff242-8d3a-46a9-8890-45722c2f3d27</value>
            </claim>
        </required-claims>
    </validate-jwt>
</inbound>

```

<h4>Analyse der Werte:</h4> 

* **backend-id="provisionierungacco"** Dies sollte auf Ihre Azure Function verweisen.
* **openid-config url="https://login.microsoftonline.com/0da700fe-a3a7-4aaa-a43f-48a79eefc326/v2.0/.well-known/openid-configuration"**
Jede App-Registrierung in Azure AD erhält einen öffentlich zugänglichen Endpunkt, der ihr OpenID-Konfigurationsdokument bereitstellt.
Um das Konfigurationsdokument für Ihre App zu finden, navigieren Sie im Azure-Portal zu: Azure Active Directory >> App-Registrierungen >> Ihre App >> Endpunkte. Suchen Sie die URI unter OpenID Connect-Metadatendokument. Sie sollte ungefähr so aussehen:
https://login.microsoftonline.com/YOURTENANTID/v2.0/.well-known/openid-configuration

* Pfad: /.well-known/openid-configuration
* Autoritäts-URL: https://login.microsoftonline.com/{tenant}/v2.0

Weitere Informationen finden Sie unter OpenID Connect auf der Microsoft-Identitätsplattform.

<h3>Erforderliche Ansprüche (required-claims)</h3> <h4>aud</h4> Es liegt an Ihnen, zu entscheiden, welche Ansprüche überprüft werden sollen. Einer der häufigsten ist der **aud**, der in unserem Fall mit der GUID in unserem Bereich identisch ist.
Gemäß der RFC-Definition bezieht sich der aud-Anspruch auf den Empfänger des Zugriffstokens:

Der "aud" (audience)-Anspruch identifiziert die Empfänger, für die das JWT bestimmt ist. Jede Entität, die das JWT verarbeiten soll, MUSS sich mit einem Wert im "aud"-Anspruch identifizieren. Wenn die Entität, die den Anspruch verarbeitet, sich nicht mit einem Wert im "aud"-Anspruch identifiziert, wenn dieser Anspruch vorhanden ist, MUSS das JWT abgelehnt werden.

In unserem Fall ist der Empfänger, oder die Zielgruppe, die Azure Function.


<h4>azp</h4>

[IANA](https://www.iana.org/assignments/jwt/jwt.xhtml) definiert **azp** als <i>Authorized party - die Partei, der das ID-Token ausgestellt wurde</i>.

Die [OpenID-Spezifikation](https://openid.net/specs/openid-connect-core-1_0.html) liefert folgende Definition:
<br/>

>OPTIONAL. Authorized party - die Partei, der das ID-Token ausgestellt wurde. Wenn vorhanden, MUSS es die OAuth 2.0-Client-ID dieser Partei enthalten. Dieser Anspruch wird nur benötigt, wenn das ID-Token einen einzigen Audience-Wert hat und diese Audience sich von der autorisierten Partei unterscheidet. Es KANN auch enthalten sein, wenn die autorisierte Partei mit der alleinigen Audience identisch ist. Der azp-Wert ist eine fallunempfindliche Zeichenkette, die einen StringOrURI-Wert enthält.

<br/>

Ich überprüfe diesen Anspruch gerne, da er hilft, das Problem der erforderlichen Zuweisung zu vermeiden. Anstatt (oder zusätzlich zu) den Berechtigungen für Ihren Client zu Ihrer Backend-App können Sie im JWT überprüfen, wer die Anfrage sendet. Falls erforderlich, können mehrere Werte akzeptiert werden, z. B. in Szenarien, in denen 2 oder mehr Apps Ihre Azure Function aufrufen:

```xml

<required-claims>
    <claim name="aud" match="any">
        <value>7e5ff242-8d3a-46a9-8890-45722c2f3d27</value>
    </claim>
    <claim name="azp" match="any">
         <value>a1888df2-84c2-4379-8d53-7091dd630ca7</value>
         <value>f1d55d9b-b116-4f54-bc00-164a51e7e47f</value>
         <value>d5dfkae9-4f54-bc00-8d53-164a5130ca7b</value>
    </claim>
</required-claims>

```

<br/><br/>

<h1>Tooling</h1>
Es gibt mehrere großartige Tools, die Ihnen beim Testen und Debuggen Ihres Szenarios helfen können.

<h3>Postman</h3>
Alle API-Aufrufe können mit Postman getestet werden. Sie können die Postman-Software kostenlos von der offiziellen Postman-Website herunterladen. Falls eine Installation aufgrund von Proxy-Problemen, Unternehmensrichtlinien oder anderen Einschränkungen nicht möglich ist, gibt es eine Online-Version von Postman. Sie können sich anmelden, und es funktioniert hervorragend. Gelegentlich kann es CORS-Probleme geben, insbesondere bei einer komplexen Netzwerkkonfiguration, aber in 99 % der Fälle ist es einfach zu verwenden und sehr portabel - was wichtig ist, wenn Sie häufig zwischen Maschinen und Umgebungen wechseln.

Postman bietet eine Option, Ihre Anmeldeinformationen zu speichern, um Ihre Anfragen zu beschleunigen. Da jedoch die Authentifizierung genau das ist, was Sie testen, um Ihre API-Management-Einrichtung zu überprüfen, sollten Sie diese Option nicht nutzen.

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc15.png" width="400">


Zum Senden der Anfrage für das JWT sind folgende Angaben erforderlich:

* POST
* client_id
* client_secret
* grant_type
* scope


<img src="/articles/images/SecureAzFunc/Github-SecAzFunc16.png" width="400">


Setzen Sie Grant_type auf "client_credentials". Stellen Sie sicher, dass der Scope mit dem Bereich übereinstimmt, den Sie in Ihrer Azure-App-Registrierung definiert haben:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc17.png" width="200"> <br/> <img src="/articles/images/SecureAzFunc/Github-SecAzFunc18.png" width="600"> ```





<h3>Application Insights</h3>

**Application Insights** ist eine Erweiterung von Azure Monitor und bietet Funktionen für die Anwendungsleistungsüberwachung (auch bekannt als „APM“). Es ist nahtlos in Azure Functions integriert und erfordert keinen zusätzlichen Programmieraufwand. 

Um Application Insights zu aktivieren, navigieren Sie zu Ihrer Function App, scrollen Sie nach unten und klicken Sie auf **Turn on Application Insights**:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc19.png" width="400">

Stellen Sie sicher, dass Application Insights auch Protokolle für **API Management** sammelt:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc20.png" width="400">

Überprüfen Sie Ihre Application Insights-Konfiguration, indem Sie z. B. ein abgelaufenes Token senden. In den Protokollen sollten Sie eine Ausnahme wie diese sehen:

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc21.png" width="400">

---

<h4>Ausgewählte Funktionen</h4>

Einige der nützlichen Funktionen von Application Insights sind:

* **Verfügbarkeitstests (OOTB)**, die automatisch prüfen, ob Ihre Anwendung verfügbar ist:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc22.png" width="400">

* **Benachrichtigungen (OOTB)**, die per E-Mail oder SMS gesendet werden:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc23.png" width="400">

* Die Möglichkeit, einen Überblick über die allgemeine Anwendungsleistung zu erhalten und die Eigenschaften jeder Ausnahme detailliert zu untersuchen (diese können Sie auch anpassen):

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc24.png" width="400">

* Die Möglichkeit, Ergebnisse zu gruppieren, um Muster und wiederkehrende Probleme zu identifizieren:

  <img src="/articles/images/SecureAzFunc/Github-SecAzFunc25.png" width="400">



<h3>Trace</h3>

**Trace** ist eine der Testoptionen innerhalb des API Managements. Es ermöglicht Ihnen, Ihre Aufrufe schnell zu testen. Beachten Sie jedoch, dass Trace-Protokolle sensible Informationen wie Schlüssel, Zugriffstoken, Passwörter, interne Hostnamen und IP-Adressen enthalten können. Seien Sie vorsichtig beim Teilen von Trace-Protokollen aus dem API Management.

<h4>Wie aktiviert man Trace?</h4>

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc26.png" width="400">

---

<h3>Ausführungen mit History überwachen</h3>

Jede Azure Function benötigt einen Speicher, in dem Sie zwei Tabellen finden:

* **History**  
* **Instances**

Die Daten enthalten die Eingaben und Ausgaben der Azure Function sowie die Zeitpunkte, zu denen die Funktion aufgerufen wurde.

<img src="/articles/images/SecureAzFunc/Github-SecAzFunc27.png" width="400">

---

<h1>Siehe auch</h1>

* [Authentication and authorization in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/authentication-authorization-overview)
* [Introduction to permissions and consent](https://learn.microsoft.com/en-us/azure/active-directory/develop/permissions-consent-overview)
* [Just what *is* the /.default scope in the Microsoft identity platform & Azure AD?](https://dev.to/425show/just-what-is-the-default-scope-in-the-microsoft-identity-platform-azure-ad-2o4d)

---

<h2>Kommentare? Fragen?</h2>

Hinterlassen Sie Ihre Kommentare [hier](https://github.com/PowershellScripts/PowershellScripts.github.io/issues/new/choose)





<!-- Default Statcounter code for Azure - all
https://powershellscripts.github.io/articles/en/Azure/Securing%20Azure%20Functions/
-->
<script type="text/javascript">
var sc_project=12962351; 
var sc_invisible=1; 
var sc_security="ab89bc3d"; 
var sc_client_storage="disabled"; 
</script>
<script type="text/javascript"
src="https://www.statcounter.com/counter/counter.js"
async></script>
<noscript><div class="statcounter"><a title="Web Analytics
Made Easy - Statcounter" href="https://statcounter.com/"
target="_blank"><img class="statcounter"
src="https://c.statcounter.com/12962351/0/ab89bc3d/1/"
alt="Web Analytics Made Easy - Statcounter"
referrerPolicy="no-referrer-when-downgrade"></a></div></noscript>
<!-- End of Statcounter Code -->
