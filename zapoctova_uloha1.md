1. Spustila jsem docker přes Powershell pomocí:
docker pull bkimminich/juice-shop
docker run -d -p 3000:3000 bkimminich/juice-shop
2. Ujistila se, že mi běží v prohlížeči http://localhost:3000
3. V Juice shop jsem si vytvořila účet test@test.cz  s heslem 123456
4. Pak jsem do PS vložila kód s přihlašovacími údaji, abych získala JWT Token – tj. potvrzení, že jsem se úspěšně přihlásila:

$baseUrl = "http://localhost:3000"

$loginBody = @{ email = "test@test.cz"; password = "123456" } | ConvertTo-Json
$login = Invoke-RestMethod -Uri "$baseUrl/rest/user/login" -Method Post -Body $loginBody -ContentType "application/json"
$token = $login.authentication.token
$headers = @{ Authorization = "Bearer $token" }
$token

<img width="945" height="326" alt="image" src="https://github.com/user-attachments/assets/c3daae94-e20a-4dd5-af1d-dd312cf6a173" />


5. Zjištění basketId z tokenu: 6

<img width="945" height="137" alt="image" src="https://github.com/user-attachments/assets/b7e24a71-6000-4cf7-a5a5-b377799f60e1" />

6. Nalezení skrytého vánočního produktu přes SQL injection: Našla jsem Christmas Super-Surprise-Box s id 10

<img width="945" height="413" alt="image" src="https://github.com/user-attachments/assets/c1686184-b89c-4338-80b7-d5ebb0bb77ca" />

7. Přidala jsem produkt do košíku přes API:
 
<img width="940" height="193" alt="image" src="https://github.com/user-attachments/assets/8c3f2a43-e38c-44ca-a472-733cb2467e99" />

8. Vytvoření a uplatnění falešného kupónu 50% na říjen 2026:
pEw8phz3^u je kód, který vznikl zakódováním textu OCT26-50 (říjen 2026, 50% sleva) algoritmem Z85 – stejným, jaký Juice Shop interně používá ke generování kupónů. Protože jde jen o zakódování (ne šifrování), kód lze takhle dopředu „vyrobit” i bez znalosti skutečného aktuálního kupónu.
 
<img width="859" height="171" alt="image" src="https://github.com/user-attachments/assets/55ebde02-d052-4269-b03e-f49cc3e352f0" />

9. Kontrola obsahu košíku

<img width="945" height="700" alt="image" src="https://github.com/user-attachments/assets/6c1eec46-0428-4c9f-8709-4cbb82a115a0" />

10. Přepnula jsem se do prohlížeče, přihlásila jsem se jako daný účet a šla na basket:

<img width="945" height="338" alt="image" src="https://github.com/user-attachments/assets/02c7073b-5f1d-4c77-84a2-58ddcedce983" />

Klikla jsem na checkout, vyplnila adresu, vybrala libovolné doručení, kartu
Výsledek neboli checkout platby:

<img width="686" height="725" alt="image" src="https://github.com/user-attachments/assets/b11f334f-6372-4fcd-8bec-76f0a7c8348f" />

 

