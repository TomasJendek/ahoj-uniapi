# Sprievodca integráciou Ahoj UNI API rozhrania


## Inštalácia a inicializácia Ahoj UNI API rozhrania
Pri používaní volaní na Ahoj API je potrebné:
* V testovacom prostredí nastaviť BASE_URL = https://api.test.psws.xyz
* V produkčnom prostredí nastaviť BASE_URL = https://api.ahojsplatky.sk
* Každé HTTPS volanie Ahoj API musí obsahovať hlavičku (header) a v nej parameter API_KEY. Zodpovednosť pri používaní API_KEY je na strane integrátora. API_KEY bude dodaný zo strany Ahoj TBD.

Pre inicializáciu knižnice Ahoj.js je potrebné :
* Importovanie JavaScript skriptu ahoj.min.js do kódu hlavičky stránky.
* Importovanie CSS súboru ahoj.min.css do kódu hlavičky stránky.
* Realizovať volanie API Ahoj [GET metódy /eshop/{businessPlace}/product/payment-methods](/) ktorej výstupom je zoznam platobných metód `paymentMethods`. Príklad volania: `[GET] https://api.test.psws.xyz/eshop/TEST_ESHOP/product/payment-methods`

* Vytvoriť JavaScript-ový objekt Ahoj pomocou konfigurácie, ktorá obsahuje zoznam platobných metód z predošlého volania API Ahoj . Konfigurácia je bližšie popísaná v Ahoj.js dokumentácii.




```html
<head>
    <!-- Zdrojové súbory knižnice -->
    <script src="./ahoj.min.js"></script>
    <link rel="stylesheet" href="ahoj.min.css">

    <!--- Inicializácia Ahoj rozrania -->
    <script>
        const configuration = {
            paymentMethods: [
                /** sem je potrebné vložiť platobné metódy **/
            ]
        }
        const ahoj = new Ahoj(configuration);
        window.ahoj = ahoj;
    </script>
</head>
```

## Inštalácia a inicializácia Ahoj UNI API rozhrania
asdfasf

### Zobrazenie produktového mini banneru
asdf


## Ahoj API endpointy

### Payment-methods
`[GET] /eshop/{businessPlace}/product/payment-methods`

Metóda sa volá pri inicializácii Ahoj.js JavaScript knižnice. Volanie metódy vráti zoznam platobných metód dostupných pre E-shop. Výstupný objekt obsahuje parametre služby ktoré definujú obchodné podmienky.

#### Vstupné parametre

| Názov | Typ | Popis | Povinný |
|:-------:|:---------:|:------|:------|
| `API_KEY`  | header | Autorizačný kľúč predajného miesta   |  Áno |
| `businessPlace` | path | Identifikátor pre predajné miesto služby KTZo30d | Áno |


#### Návratová hodnota
Array of [`PaymentMethod`]: Objekt reprezentuje platobnú metódu dostupnú pre E-shop v rámci KTZo30d.



Príklad volania:
`[GET] https://api.test.psws.xyz/eshop/TEST_ESHOP/product/payment-methods`

Príklad návratovej hodnoty:

```json
[
   {
     code: 'DEFERRED_PAYMENT',
     name: 'Odlož.to',
     title: 'Ahoj - platba o 30 dní bez navýšenia',
     promotions: [ [Object] ]
   },
   {
     code: 'SPLIT_PAYMENT',
     name: 'Rozlož.to',
     title: 'Ahoj - v 3 platbách bez navýšenia',
     promotions: [ [Object] ]
   }
 ]
```


