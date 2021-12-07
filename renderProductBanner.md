# Metóda: renderProductBanner

____
`Ahoj.renderProductBanner(selector, productBannerPayload)`

Metóda na vygenerovanie produktového banneru.

#### Použitie

```javascript
const selector = '.product-banner';
const calculatedProduct = {
    /** výsledok kalkulácie produktu **/
};
const productBannerPayload = {
    fullPrice: 123.34,
    calculatedProduct: calculatedProduct
}
const productBannerRendered = ahoj.renderProductBanner(selector, productBannerPayload);
```

#### Vstupné argumenty

| Poradie | Dátový typ | Popis |
|---- |:-------|:-------|
| 1. | [`DOMString`](https://developer.mozilla.org/en-US/docs/Web/API/DOMString), [`Element`](https://developer.mozilla.org/en-US/docs/Web/API/Element) | CSS selektor, do ktorého sa má vložiť banner; Alebo element, do ktorého sa má vložiť banner. |
| 2. | [`ProductBannerPayload`](../types/ProductBannerPayload.md) | Objekt obsahujúci údaje, ktoré má banner vygenerovať. |

#### Návratová hodnota

`boolean`: `true` ak bol banner úspešne vygenerovaný, inak `false`