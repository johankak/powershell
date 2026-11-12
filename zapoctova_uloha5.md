## Úloha 1
Nalezení třídy pro správu tiskáren 
```
Get-CimClass -ClassName *Printer*
```
Pro správu tiskáren se používá třída Win32_Printer

Zjištění vlastností třídy
```
(Get-CimClass -ClassName Win32_Printer).CimClassProperties | Select-Object Name, CimType, Flags
 ```
<img width="884" height="41" alt="image" src="https://github.com/user-attachments/assets/e4d737a8-f3c7-40d8-8916-7daeb545e9dd" />

Location je zapisovatelná, takže ji lze změnit

Výpis nainstalovaných tiskáren a jejich umístění
```
Get-CimInstance -ClassName Win32_Printer | Select-Object Name, Location
```
Fax v systému není, použiju existující tiskárnu

Změna umístění pomocí location
```
Get-CimInstance -ClassName Win32_Printer -Filter "Name='Microsoft Print to PDF'" |
    Set-CimInstance -Property @{ Location = "Brno, budova A, 2. patro" }
```
Ověření změny
```
Get-CimInstance -ClassName Win32_Printer | Select-Object Name, Location
 ```
<img width="755" height="328" alt="image" src="https://github.com/user-attachments/assets/2d4afb90-ffaa-4d10-8e5a-aeea20aa8100" />

## Úloha 2
Přejmenování disku C: na “System”

Zjistit aktuální název disku
```
Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DeviceID='C:'" |
    Select-Object DeviceID, VolumeName, FileSystem, Size, FreeSpace
```
<img width="945" height="186" alt="image" src="https://github.com/user-attachments/assets/d692e80d-63f5-424f-8266-daa5fca53a5b" />

Přejmenování
```
$disk = Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DeviceID='C:'"
Set-CimInstance -InputObject $disk -Property @{ VolumeName = "System" }
```
** Ověření
```
(Get-CimInstance Win32_LogicalDisk -Filter "DeviceID='C:'").VolumeName
 ```
<img width="945" height="86" alt="image" src="https://github.com/user-attachments/assets/7f464c14-2c5e-4c4d-b6b9-16ed179cb54e" />


## Úloha 3
Nepoužité a uzamčené účty
#### A) Přes CIM rozhraní
Uzamčené lokální účty
```
Get-CimInstance -ClassName Win32_UserAccount -Filter "LocalAccount=True AND Lockout=True" |
    Select-Object Name, Domain, Lockout, Disabled
```
žádné
<img width="1054" height="95" alt="image" src="https://github.com/user-attachments/assets/8a1f1104-99b6-4f23-8a22-484098f71b2e" />
 

Nepoužité účty (nikdy se nepřihlásily)
```
Get-CimInstance -ClassName Win32_NetworkLoginProfile |
    Where-Object { $_.LastLogon -eq $null } |
    Select-Object Name, LastLogon, NumberOfLogons
```
<img width="945" height="257" alt="image" src="https://github.com/user-attachments/assets/49b815d8-55b0-42ff-b7d4-22b9418154e5" />
 

#### B) Pomocí příkazů PS
Nepoužité účty
```
Get-LocalUser | Where-Object { $null -eq $_.LastLogon } |
    Select-Object Name, Enabled, LastLogon
``` 
<img width="945" height="236" alt="image" src="https://github.com/user-attachments/assets/c5e9202c-30d3-4613-b6c2-6d152f61e8ef" />

Uzamčené účty: Get-LocalUser vlastnost pro uzamčení nemá…proto se uzamčené účty tímto příkazem zjistit nedají (lze použít net user <jméno>, nebo CIM)

### Porovnání 
Nepoužité účty: Get-LocalUser je jednodušší, protože LastLogon má přímo u účtu. U CIM je potřeba jiná třída než pro základní údaje o účtu.

Uzamčené účty: CIM je jednodušší, protože Win32_UserAccount má vlastnost Lockout. Get-LocalUser ji nemá.


