# Sprievodca integráciou Ahoj UNI API rozhrania
Sprievodca integráciou je určený pre registrovaných partnerov spoločnosti Amico
Finance a.s., ktorí prevádzkujú elektronický obchod.

## Definícia pojmov a skratiek ##
| Skratka | Popis |
|:-------:|:---------|
| Služby Ahoj  | Komerčné služby Ahoj, poskytované spoločnosťou Amico Finance, a.s. | 
| Poskytovateľ  | Poskytovateľom Služby Ahoj „Kúp teraz, zaplať o 30 dní“ je Amico Finance, a.s. | 
| KTZo30d  | Služba Ahoj „Kúp teraz, zaplať o 30 dní“. | 
| API Ahoj  | Application Programming Interface – aplikačné programové REST rozhranie back-endových služieb Ahoj
|
| Integrátor  | Osoba na strane obchodníka, ktorá implementuje integráciu Ahoj UNI API rozhrania |
| Ahoj.js  | Ahoj JavaScript-ová knižnica |

Pod integráciou Ahoj UNI API rozhrania rozumieme definovanie technických prostriedkov na výmenu informácií medzi Ahoj API, Ahoj.js knižnicou a online službou obchodníka za účelom realizácie poskytnutia služby KTZo30d. Technicky je integrácia realizovaná kombináciou volaní na API Ahoj a použitím JavaScript knižnice Ahoj.js na client site strane integrátora.



## Inštalácia a inicializácia Ahoj UNI API rozhrania
Pri používaní volaní na Ahoj API je potrebné:
* V testovacom prostredí nastaviť BASE_URL = https://api.test.psws.xyz
* V produkčnom prostredí nastaviť BASE_URL = https://api.ahojsplatky.sk
* Každé HTTPS volanie Ahoj API musí obsahovať hlavičku (header) a v nej parameter API_KEY. Zodpovednosť pri používaní API_KEY je na strane integrátora. API_KEY bude dodaný zo strany Ahoj TBD.

Pre inicializáciu knižnice Ahoj.js je potrebné :
* Importovanie JavaScript skriptu ahoj.min.js do kódu hlavičky stránky.
* Importovanie CSS súboru ahoj.min.css do kódu hlavičky stránky.
* Realizovať volanie API Ahoj [metódy /payment-methods](/api-payment-methods) ktorej výstupom je zoznam dostupných platobných metód `paymentMethods`.
* Vytvoriť JavaScript-ový objekt Ahoj pomocou konfigurácie, ktorá obsahuje zoznam dostupných platobných metód z predošlého volania API Ahoj .

#### Príklad inicializácie Ahoj.js
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

## Zobrazenie produktového banneru
Pre zobrazenie a prekreslenie produktového mini banneru služby KTZo30d na strane integrátora na stránkach detail produktu je nutné:
* Realizovať volanie API Ahoj [metódy /calculation](/api-calculation), ktorá pre konečnú cenu produktu vráti sadu finančných parametrov pôžičky v objekte `CalculatedProduct`.
* Pre vykreslenie produktového mini banneru použijeme metódu [Ahoj.renderProductBanner](https://github.com/TomasJendek/ahoj-uniapi/wiki/renderProductBanner), kde na vstupe vložíme výsledok kalkulácie produktu z predošlého volania - objekt `CalculatedProduct`.

####Sekvenčný diagram zobrazenia produktového banneru:
![](https://github.com/TomasJendek/ahoj-uniapi/blob/main/product-banner-sequence-diagram.png?raw=true)



## Ahoj API endpointy

<a name="api-payment-methods"></a>
### Payment-methods
`[GET] /eshop/{businessPlace}/product/payment-methods`

Metóda sa volá pri inicializácii Ahoj.js JavaScript knižnice. Volanie metódy vráti zoznam platobných metód dostupných pre E-shop. Výstupný objekt obsahuje parametre služby ktoré definujú obchodné podmienky.

#### Vstupné parametre

| Názov | Typ | Popis | Povinný |
|:-------:|:---------:|:------|:------|
| API_KEY  | header | Autorizačný kľúč predajného miesta   |  Áno |
| businessPlace | path | Identifikátor pre predajné miesto služby KTZo30d | Áno |


#### Návratová hodnota
Array of [`PaymentMethod`]: Objekt reprezentuje platobnú metódu dostupnú pre E-shop v rámci KTZo30d.



#### Príklad volania:

`GET https://api.test.psws.xyz/eshop/TEST_ESHOP/product/payment-methods`

#### Príklad návratovej hodnoty:

```json
[
  {
    "code": "DEFERRED_PAYMENT",
    "name": "Odlož.to",
    "title": "Ahoj - platba o 30 dní bez navýšenia",
    "promotions": [
      {
        "code": "DP_DEFER_IT",
        "name": "Zaplať o 30 dní",
        "description": "Nakúp teraz, splať o 30 dní",
        "instalmentIntervalDays": 30,
        "minGoodsPrice": 10.0,
        "minGoodsItemPrice": 2.0,
        "maxGoodsPrice": 300.0,
        "maxGoodsPriceProspect": 300.0,
        "deposit": {
          "from": 0.0,
          "to": 0.0,
          "step": 0.0
        },
        "depositPercent": {
          "from": 0.0,
          "to": 0.0,
          "step": 0.0
        },
        "instalmentCount": {
          "from": 1,
          "to": 1,
          "step": 1
        },
        "loanAmount": {
          "from": 10.0,
          "to": 350.0
        },
        "productType": {
          "code": "GOODS_DEFERRED_PAYMENT",
          "name": "Kúp teraz, zaplať neskôr"
        }
      }
    ]
  },
  {
    "code": "SPLIT_PAYMENT",
    "name": "Rozlož.to",
    "title": "Ahoj - v 3 platbách bez navýšenia",
    "promotions": [
      {
        "code": "SP_SPLIT_IT",
        "name": "rozlož to",
        "description": "3 bezúročné platby",
        "instalmentIntervalDays": 1,
        "minGoodsPrice": 50.0,
        "minGoodsItemPrice": 20.0,
        "maxGoodsPrice": 300.0,
        "maxGoodsPriceProspect": 300.0,
        "deposit": {
          "from": 0.0,
          "to": 0.0,
          "step": 0.0
        },
        "depositPercent": {
          "from": 0.0,
          "to": 0.0,
          "step": 0.0
        },
        "instalmentCount": {
          "from": 3,
          "to": 3,
          "step": 1
        },
        "loanAmount": {
          "from": 20.0,
          "to": 350.0
        },
        "productType": {
          "code": "GOODS_SPLIT_PAYMENT",
          "name": "Rozlož to",
          "instalmentDayOfMonth": 20
        }
      }
    ]
  }
]
```
Detailný opis metódy nájdete v [API dokumentácii](/).


<a name="api-calculation"></a>
### Calculation
`[POST] /eshop/{businessPlace}/calculation`

Metóda slúži na výpočet plnej sady finančných parametrov pôžičky na základe zvolených vstupných parametrov. Používa sa pre zobrazenie splátok metódy RT.
#### Vstupné parametre

| Názov | Typ | Popis | Povinný |
|:-------:|:---------:|:------|:------|
| API_KEY  | header | Autorizačný kľúč predajného miesta   |  Áno |
| businessPlace | path | Identifikátor pre predajné miesto služby KTZo30d | Áno |

#### Telo volania

| Typ | Objekt | Popis |  
|:-------:|:---------:|:------|
| application/json  | `Product` | Vstupné parametre pre výpočet parametrov pôžičky  | 


#### Návratová hodnota
Array of [`CalculatedProduct`]: Objekt reprezentuje finančné parametre pre produkty na vstupe.



#### Príklad volania:

`POST https://api.test.psws.xyz/eshop/{businessPlace}/calculation/`

```json
POST /eshop/{businessPlace}/calculation
Content-Type: application/json
Accept: application/json
API_KEY: ...
        
{
   "amount": 900,
   "depositAmount": 100,
   "depositPercent": 0.1,
   "instalmentCount": 12,
   "promotion": {
      "code": "SP_SPLIT_IT",
      "name": "rozlož to",
      "description": "3 bezúročné platby",
      "instalmentIntervalDays": 1,
      "minGoodsPrice": 50,
      "minGoodsItemPrice": 20,
      "maxGoodsPrice": 300,
      "maxGoodsPriceProspect": 300,
      "productType": {
         "code": "GOODS_SPLIT_PAYMENT",
         "name": "Rozlož to",
         "instalmentDayOfMonth": 20
      }
   },
   "goods": [
      {
         "price": 1000,
         "totalPrice": 1000
      }
   ],
   "instalment": {
      "amount": 75,
      "amountWithInsurance": 75,
      "insuranceAmount": 0
   },
   "averageRpmn": 0,
   "interest": 0,
   "maxReward": 0,
   "reward": 0,
   "rpmn": 0,
   "totalAmount": 900,
   "totalAmountWithInsurance": 900,
   "totalCosts": 0
}
```

#### Príklad návratovej hodnoty:

```json
[
  {
    "amount": 70.89,
    "depositAmount": 0,
    "depositPercent": 0,
    "instalmentCount": 3,
    "promotion": {
      "code": "SP_SPLIT_IT",
      "name": "rozlož to",
      "description": "3 bezúročné platby",
      "instalmentIntervalDays": 1,
      "minGoodsPrice": 50,
      "minGoodsItemPrice": 20,
      "maxGoodsPrice": 300,
      "maxGoodsPriceProspect": 300,
      "productType": {
        "code": "GOODS_SPLIT_PAYMENT",
        "name": "Rozlož to",
        "instalmentDayOfMonth": 20
      }
    },
    "goods": [
      {
        "price": 70.89,
        "totalPrice": 70.89
      }
    ],
    "instalment": {
      "amount": 23.63,
      "amountWithInsurance": 23.63,
      "insuranceAmount": 0
    },
    "averageRpmn": 0,
    "interest": 0,
    "maxReward": 0,
    "reward": 0,
    "rpmn": 0,
    "totalAmount": 70.89,
    "totalAmountWithInsurance": 70.89,
    "totalCosts": 0
  }
]
```
Detailný opis metódy nájdete v [API dokumentácii](/).