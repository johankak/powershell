# Zápočtová úloha 2
Vytvořte skript, který vykreslí paletu 16 odstínů barev přednastavených v terminálu (viz obrázek)

AI Claude mi pomohl vytvořit skript, ve kterém:

[System.Enum]::GetNames([System.ConsoleColor]) vrátí všech 16 názvů barev ve stejném pořadí, jaké je na obrázku (Black, DarkBlue, DarkGreen, ... White).

-NoNewline drží všechny tři sloupce na jednom řádku. Až poslední Write-Host řádek ukončí.

PadRight(22) zarovná sloupce do stejné šířky, takže jsou pod sebou.

<img width="1090" height="462" alt="image" src="https://github.com/user-attachments/assets/1b1c03a3-b5a7-480b-b5dc-1817fae58090" />


Skript jsem si uložila jako paleta.ps1 na plochu a spustila v PS
 
<img width="962" height="553" alt="image" src="https://github.com/user-attachments/assets/a7949c7a-7144-4730-a19c-0ce637484656" />
