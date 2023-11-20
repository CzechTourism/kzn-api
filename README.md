# Kudy z nudy API

Níže je popsáno aplikační rozhraní (API) pro přístup k publikovanému obsahu a dalším službám na portálu [Kudy z nudy](https://www.kudyznudy.cz/). API slouží pro integraci externích subjektů a aplikací a umožňuje jim získávat obsah publikovaný na portále Kudy z nudy - [Export dat z portálu Kudy z nudy](/Export.md). Dále se API používá u obrácené integrace - [Import dat od externích subjektů do Kudy z nudy](/Import.md).

Provozovatelem portálu [Kudy z nudy](https://www.kudyznudy.cz) a poskytovatelem API je agentura [CzechTourism](https://www.czechtourism.cz/).

## Přístup k API

API je přístupné pouze pro smluvní partnery CzechTourism. **Pro přístup k API je nutné mít účet na portálu Kudy z nudy a k tomuto účtu mít přidělena přístupová práva.** Přístupová práva obdrží partner od poskytovatele API. Více informací pro získání přístupu naleznete na [Kudy z nudy - API](https://www.kudyznudy.cz/faq-casto-kladene-otazky/api).

## Základní informace k API

API běží na adrese [https://api.kudyznudy.cz](https://api.kudyznudy.cz/).

Automaticky generovaná dokumentace API ve formátu Swagger je na adrese [https://api.kudyznudy.cz/swagger/ui/index](https://api.kudyznudy.cz/swagger/ui/index).

V dokumentaci jsou uvedeny příklady volání API pomocí cURL.

## Formát dat

Jednotlivé služby popsané níže vrací data ve formátu [JSON](https://www.json.org/). Modely dat pro publikovaný obsah byly navrženy podle vzoru Otevřených dat [https://opendata.gov.cz](https://opendata.gov.cz/), respektive podle Otevřených formálních norem [https://data.gov.cz/ofn/](https://data.gov.cz/ofn/).

## Podporované metody

[Import dat do Kudy z nudy](/Import.md)

[Export dat z Kudy z nudy](/Export.md)
