Vytvořila jsem v notepadu 2 skripty
1) palindrom.ps1 který ověří, jestli je zadaný text palindrom
```
param([Parameter(Mandatory)][string]$Text)

$cisty = $Text.Normalize([Text.NormalizationForm]::FormD) -replace '\p{M}', ''
$cisty = ($cisty -replace '[^\p{L}\p{N}]', '').ToLower()

$znaky = $cisty.ToCharArray()
[array]::Reverse($znaky)
$obracene = -join $znaky

if ($cisty -eq $obracene) { "'$Text' JE palindrom." }
else                      { "'$Text' NENÍ palindrom." }
```

2) analyza.ps1 vypíše počet znaků s mezerami i bez nich, samohlásek a slov
```
param([Parameter(Mandatory)][string]$Text)

$sMezerami  = $Text.Length
$bezMezer   = ($Text -replace '\s', '').Length
$samohlasky = [regex]::Matches($Text, '[aeiouyáéěíóúůý]', 'IgnoreCase').Count
$slova      = ($Text -split '\s+' | Where-Object { $_ }).Count

"Znaky s mezerami: $sMezerami"
"Znaky bez mezer:  $bezMezer"
"Samohlásky:       $samohlasky"
"Slova:            $slova"
```
Výstup:

<img width="945" height="642" alt="image" src="https://github.com/user-attachments/assets/a7a0c8a5-237e-44f1-9a83-96348d9da640" />

 
