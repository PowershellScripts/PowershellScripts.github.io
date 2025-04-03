
ExchangeOnlineManagement fails to connect


Connect-ExchangeOnline -UserPrincipalName
InvalidOperation: You cannot call a method on a null-valued expression.


Check how many you have

get-module -ListAvailable | where {$_.Name -match "Exchange"}

Uninstall-Module -Name ExchangeOnlineManagement -force

get-module -ListAvailable | where {$_.Name -match "Exchange"}


Install-Module -Name ExchangeOnlineManagement -RequiredVersion 3.6.0

import-module -Name ExchangeOnlineManagement -MinimumVersion 3.0.0 -force