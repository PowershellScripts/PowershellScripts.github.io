Introduction


Managing your flows using PnP is fabulously easy. Faster to code than CSOM, more options than classic SharePoint Online Management Shell. Using PnP you can easily retrieve users who are creating flows and extract data statistics.



Get Flow Properties

First, retrieve properties of the flow. Remember to use -AsAdmin switch. Otherwise you will get only your own flows.
Connect-PnpOnline
$environment = Get-PnPPowerPlatformEnvironment
$Flows = Get-PnPFlow -Environment $environment -AsAdmin | select -expandProperty Properties

 

One of those properties is the Creator. If you expand it, you obtain ObjectId, which is Azure Active Directory ObjectId.
 


Using Group-Object and Sort-Object cmdlets, you get the users who created most flows.
$props.Creator | Group-Object -Property ObjectId -NoElement  | Sort-Object -Descending

 



Get Azure Active Directory Users


Use Azure Active Directory ObjectId to obtain user's UPN or email address.
Connect-MSOLService
Get-MsolUser | where {$_.ObjectId -eq "f655dd56-ffea-45ad-aa45-775e4e0eeb9b"}



Full Script

Connect-PnPOnline
$environment = Get-PnPPowerPlatformEnvironment
$flowprops = Get-PnPFlow -AsAdmin -Environment $environment | select -ExpandProperty Properties
$MostProlific = $flowprops.Creator | Group-Object -Property ObjectId | sort
 
$MostProlific | Foreach-Object {
    $FlowCreator = $_ ; 
    $user = Get-MsolUser | where {
        $_.ObjectId -eq $FlowCreator.Name
    };
    $user | Add-Member -MemberType NoteProperty -Name NoOfFlow -Value $FlowCreator.Count;
    Write-Host $user.DisplayName $user.NoOfFlow
    $user | Export-CSV -Path yourcsvpath.csv -Append
}

 


