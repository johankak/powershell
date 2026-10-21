# Zadání
## Vytvořte alias np (notepad.exe) a ct (control.exe). Oba aliasy si zálohujte do formátu JSON a také do formátu XML. Potom oba aliasy smažte a obnovte je z JSON souboru. Znovu je smažte a obnovte z XML souboru.


### Postup řešení

1) Nejdříve jsem si připravila složky pro zálohy příkazem:
   
 ``` New-Item -ItemType Directory -Path C:\Temp -Force ```

•	New-Item -ItemType Directory vytvoří složku

•	-Force zajistí, že příkaz nevyhodí chybu, pokud složka už existuje

3) Vytvořila jsem aliasy příkazem:
   
``` New-Alias -Name np -Value notepad.exe ```

``` New-Alias -Name ct -Value control.exe ```

•	New-Alias vytvoří nový alias, tedy zkratku pro příkaz nebo program

•	-Name je název, -Value je to, co se má spustit

5) Kontrola, že aliasy existují pomocí:
   
``` Get-Alias np, ct ```

7) Aliasy si zálohuju do JSON příkazem:
   
``` Get-Alias np, ct | Select-Object Name, Definition | ConvertTo-Json | Out-File C:\Temp\aliasy.json ```

•	Get-Alias np, ct získá objekty obou aliasů

•	Select-Object Name, Definition ponechá jen název aliasu a to, na co ukazuje

•	ConvertTo-Json převede objekty do textového formátu JSON

•	Out-File uloží výsledek do souboru

9) Kontrola obsahu:
    
``` Get-Content C:\Temp\aliasy.json ```

11) Záloha do XML:
    
``` Get-Alias np, ct | Export-Clixml -Path C:\Temp\aliasy.xml ```

•	Export-Clixml uloží objekty do XML ve speciálním formátu CLIXML, který PowerShell umí později načíst zpět

13) Kontrola:
    
``` Get-Content C:\Temp\aliasy.xml ```

15) Smazání aliasů:
    
``` Remove-Item Alias:np, Alias:ct ```

•	Alias: je PowerShellový “disk”, který obsahuje všechny aliasy, podobně jako C: obsahuje soubory

17) Kontrola, že aliasy zmizely:
    
``` Get-Alias np, ct ```

•	PowerShell vypíše chybu, že alias nebyl nalezen

19) Obnovení z JSON souboru:
    
```
Get-Content C:\Temp\aliasy.json | ConvertFrom-Json | ForEach-Object {
    New-Alias -Name $_.Name -Value $_.Definition
}
```

•	Get-Content načte text ze souboru

•	ConvertFrom-Json převede text zpět na objekty s vlastnostmi Name a Definition

•	ForEach-Object { ... } projde každý objekt (jednou pro np, jednou pro ct). Uvnitř je aktuální objekt v proměnné $_

•	New-Alias pro každý objekt znovu vytvoří alias

21) Kontrola:
    
``` Get-Alias np, ct ```

•	Aliasy jsou zpátky

23) Znovu smazat aliasy:
    
``` Remove-Item Alias:np, Alias:ct ```

``` Get-Alias np, ct ```

25) Obnovení z XML souboru:
    
```
Import-Clixml C:\Temp\aliasy.xml | ForEach-Object {
    New-Alias -Name $_.Name -Value $_.Definition
}
```

•	Import-Clixml načte objekty z XML souboru. Nevrátí ale funkční aliasy, jen jejich “otisky” (deserializované objekty), které nesou stejná data

•	Proto se opět použije ForEach-Object a New-Alias, který z těchto dat vytvoří skutečné aliasy

27) Kontrola:
    
``` Get-Alias np, ct ```

 
<img width="945" height="623" alt="image" src="https://github.com/user-attachments/assets/383ab73b-43ae-47ad-9493-7ab123f83839" />
 
<img width="945" height="471" alt="image" src="https://github.com/user-attachments/assets/b619861f-c321-4896-83c5-eed9e46f74f0" />
 
<img width="945" height="638" alt="image" src="https://github.com/user-attachments/assets/95e5f36c-289c-43c6-8c5c-eda7b1ddb63c" />
