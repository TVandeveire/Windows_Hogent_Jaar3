# --AD-- 
## Uitleg powerscript

Als we beginnen met het maken van een DC is het belangrijk om de computernaam aantepassen dit doe je via
```
Rename-computer -newname DC1
```
We moet daarbij ook een fixed ip toe wijzen, deze staat in de opdracht omschrijving
```
New-NetIPAddress -InterfaceAlias "Ethernet 2" -IPAddress "192.168.1.1" -AddressFamily IPv4 -PrefixLength 24
```
De rollen voor AD te installeren gaat als het volgend.
```
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName name.loc -SafeModeAdministratorPassword "wachtwoord"
```
**Note** merk op dat er hierbij ook een domain wordt aangemaak door  
*-DomainName*  
daarbij word er ook een SafeModeAdministratorPassword meegeven deze is voor het aanmelden van het domain , dit doe je via  
*-SafeModeAdministratorPassword*

## Testen

om te testen of dit werken moeten we in de eerste plaats contoleren of de services van adws,kdc,netlogon,dns werken.  
Dit kunnen we zien door het commando.
```
Get-Service adws,kdc,netlogon,dns
```
Als alle deze services op running staan. is er geen probleem.  
Daarbij is een controle op de sysvol en de netlogon ook van belang, bekijk de output van de het commando
```
Get-smbshare
```
Moest je toch meer informatie wensen over wat er allemaal is gebreurd kan je steeds kijken naar de Review logs via
```
get-eventlog "Directory Service" | select entrytype, source, eventid, message
get-eventlog "Active Directory Web Services" | select entrytype, source, eventid, messag
```

# Bronnnen

https://blogs.technet.microsoft.com/chadcox/2016/10/25/chads-quick-notes-installing-a-domain-controller-with-server-2016-core/  
https://docs.microsoft.com/nl-nl/windows-server/networking/technologies/dhcp/dhcp-deploy-wps  
https://www.reddit.com/r/PowerShell/comments/8fr0l8/how_do_i_convert_to_systemsecuritysecurestring/  
https://www.faqforge.com/windows/configure-dhcp-powershell/  

https://mikefrobbins.com/2013/03/14/use-powershell-to-add-an-additional-domain-controller-to-an-existing-windows-server-2012-active-directory-domain/
