# Sprievodca integráciou Ahoj UNI API rozhrania

Pri používaní volaní na Ahoj API je potrebné:
* V testovacom prostredí nastaviť BASE_URL = https://api.test.psws.xyz
* V produkčnom prostredí nastaviť BASE_URL = https://api.ahojsplatky.sk
* Každé HTTPS volanie Ahoj API musí obsahovať hlavičku (header) a v nej parameter API_KEY. Zodpovednosť pri používaní API_KEY je na strane integrátora. API_KEY bude dodaný zo strany Ahoj TBD. 

Pre inicializáciu knižnice Ahoj.js je potrebné :
* Importovanie JavaScript skriptu ahoj.min.js do kódu hlavičky stránky.
* Importovanie CSS súboru ahoj.min.css do kódu hlavičky stránky.
* Realizovať volanie API Ahoj [GET metódy /eshop/{businessPlace}/product/payment-methods](/) ktorej výstupom je zoznam platobných metód paymentMethods. 
* Vytvoriť JavaScript-ový objekt Ahoj pomocou konfigurácie, ktorá obsahuje zoznam platobných metód z predošlého volania API Ahoj . Konfigurácia je bližšie popísaná v [v Ahoj.js dokumentácii](https://gitlab.com/webgate/uni_plugin/-/tree/develop/public/docs). 


## Inštalácia a inicializácia Ahoj UNI API rozhrania
asdfasf

## Inštalácia a inicializácia Ahoj UNI API rozhrania
asdfasf

### Zobrazenie produktového mini banneru
asdf