# Zápočtová úloha 2
Vytvořte skript, který vykreslí paletu 16 odstínů barev přednastavených v terminálu (viz obrázek)

AI Claude mi pomohl vytvořit skript, ve kterém:
[System.ConsoleColor].GetEnumNames() načte seznam 16 názvů barev

foreach projde barvy jednu po druhé

.PadRight(12) doplní mezery, aby sloupce byly zarovnané

Write-Host je cmdlet, který vypisuje text do konzole

-NoNewline aby se tři sloupce vypsaly na jeden řádek

-ForegroundColor je pro barvu písma a -BackgroundColor pro barvu pozadí

Podmínka if ve skriptu řeší, že pokud je aktuální barva černá, pak se vytvoří řetězec 12 mezer a ten se předá do 2. sloupce

<img width="696" height="463" alt="image" src="https://github.com/user-attachments/assets/6d8f81d8-07d3-4c3c-87fc-76d3249fd1e1" />
 


Skript jsem si uložila jako paleta.ps1 na plochu a spustila v PS
 
<img width="903" height="594" alt="image" src="https://github.com/user-attachments/assets/b8a563b9-7938-4915-94f9-8b29d90e88f9" />


