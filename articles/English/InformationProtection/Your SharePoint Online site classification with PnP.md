
## First: Enable the capability
Site classification has to be enabled at the Azure AD level.
After enabling the site classification capability at the Azure AD level, you see an additional field *How sensitive is your data?* while creating new sites. Site classification allows you to define the sensitivity of your data on site level and group your sites based on the sensitivity of the information they contain.

You can enable the capability using ** cmdlet.

Mind you, if you already, e.g. this might not work. 



Verify the existing site classification
Use Get-PnPSiteClassification cmdlet to retrieve the existing site classification settings. As a result, you receive UsageGuidelinesUrl, Classifications, and DefaultClassification values. DefaultClassification value has to be also in the list of values you see under Classifications.

```
Get-PnPSiteClassification
```
 

In the User Interface you can see these values when you create a new site:

 




Update Site Classification options
Use Update-PnPSiteClassification cmdlet to set the available classifications.

Update-PnPSiteClassification -Classifications "HBI", "CRI", "LBI"

 

<the rest coming soon>
 


