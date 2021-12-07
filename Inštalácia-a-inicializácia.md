### Inštalácia

### Inicializácia

Pre používanie Ahoj.js knižnice a jej metód je najskôr potrebné inicializovať rozhranie Ahoj spolu s jeho konfiguráciou.

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

### Konfigurácia

| Parameter | Popis |
| --------- | ----- |
| `paymentMethods` | Pole platobných metód. |

### Dátové typy

| Názov | Dokumentácia |
| ----- | ----- |
| CalculatedProduct | [[link]](./types/calculatedproduct.md)
| Instalment | [[link]](./types/Instalment.md)
| ProductBannerPayload | [[link]](./types/ProductBannerPayload.md)
| Promotion | [[link]](./types/Promotion.md)
| PromotionAvailability | [[link]](./types/PromotionAvailability.md)

### Metódy

| Názov | Dokumentácia |
| ----- | ----- |
| `ahoj.getAvailablePromotions()` | [[link]](./methods/getAvailablePromotions.md) |
| `ahoj.getPromotionAvailability()` | [[link]](./methods/getPromotionAvailability.md) |
