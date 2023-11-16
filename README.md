# Kudy z nudy API

Níže je popsáno aplikační rozhraní (API) pro přístup k publikovanému obsahu a dalším službám na portálu [Kudy z nudy](https://www.kudyznudy.cz/). API slouží pro integraci externích subjektů a aplikací a umožňuje jim získávat primárně obsah publikovaný na portále. Stejný formát dat se také používá u obrácené integrace - tedy [importu dat od externích subjektů do Kudy z nudy](/Import.md).

Provozovatelem portálu [Kudy z nudy](https://www.kudyznudy.cz) a poskytovatelem API je agentura [CzechTourism](https://www.czechtourism.cz/).

## Přístup k API

API je přístupné pouze pro smluvní partnery CzechTourism. **Pro přístup k API je nutné mít účet na portálu Kudy z nudy a k tomuto účtu mít přidělena přístupová práva.** Přístupová práva obdrží partner od poskytovatele API. Více informací pro získání přístupu naleznete na [Kudy z nudy - API](https://www.kudyznudy.cz/faq-casto-kladene-otazky/api).

## Základní informace k API

API běží na adrese [https://api.kudyznudy.cz](https://api.kudyznudy.cz/).

Automaticky generovaná dokumentace API ve formátu Swagger je na adrese [https://api.kudyznudy.cz/swagger/ui/index](https://api.kudyznudy.cz/swagger/ui/index).

V dokumentaci jsou uvedeny příklady volání API pomocí cURL.

## Formát dat

Jednotlivé služby popsané níže vrací data ve formátu [JSON](https://www.json.org/). Modely dat pro publikovaný obsah byly navrženy podle vzoru Otevřených dat [https://opendata.gov.cz](https://opendata.gov.cz/), respektive podle Otevřených formálních norem [https://data.gov.cz/ofn/](https://data.gov.cz/ofn/).


## Autentizace k API

Pro volání služeb API je nutné se přihlásit pod uživatelským účtem, který má patřičná přístupová práva [https://api.kudyznudy.cz/login](https://api.kudyznudy.cz/login). Po úspěšném přihlášení je uživateli vrácen Bearer token, kterým je potřeba podepisovat veškerá volání v rámci API.

Uživatel může mít plný nebo omezený přístup (v závislosti na stupni partnerství). Viz  [Kudy z nudy - API](https://www.kudyznudy.cz/faq-casto-kladene-otazky/api). V případě plného přístupu je vrácen vždy kompletní datový objekt. V případně omezeného přístupu je vrácena pouze jeho veřejná část.

## Aktivita (Turistický cíl)

Aktivita (turistický cíl) je typ obsahu publikovaného na portálu Kudy z nudy. Vyznačuje se časově neomezenou dobou trvání.

Aktivity naleznete v sekci portálu [Co chcete dělat](https://www.kudyznudy.cz/co-chcete-delat).

### Seznam aktivit (turistických cílů)

Metoda vrací seznam publikovaných aktivit.

Seznam všech aktivit získáte pomocí volání:

    curl --location --request GET 'https://api.kudyznudy.cz/content/activities' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'

Pro získání aktivit změněných po určitém čase je volání služby následující:

    curl --location --request POST 'https://api.kudyznudy.cz/content/activities' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "date": "2022-10-10T00:00:00.000Z"
    }'

*Metoda se hodí například pro efektivnější synchronizaci dat.*

Odpověď je pole JSON objektů. Například:

    [
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Turistický cíl",
            "id": "630586e5-9541-4099-84ef-f8a35dc8b745",
            "iri": "https://www.kudyznudy.cz/aktivity/komentovane-prohlidky-arealu-zirec-a-domova-sv-jos",
            "název": {
                "cs": "Bylinkové zahrady Žíreč – komentované prohlídky v areálu Žireč a Domova sv. Josefa"
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T10:21:06.7089805"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T11:44:32.4484005"
            }
        }
    ]


| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Turistický cíl |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |

### Detail aktivity (turistického cíle)

Metoda slouží pro získání detailu publikované aktivity:

    curl --location --request POST 'https://api.kudyznudy.cz/content/activities' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "id": "630586e5-9541-4099-84ef-f8a35dc8b745"
    }'

Výstupem je JSON objekt. Podle typu přístupu je uživateli vrácen plnohodnotný objekt nebo jenom jeho veřejná část.

Plnohodnotný objekt (v případně plného přístupu):

    {
    "@context": "https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil.jsonld",
    "typ": "Turistický cíl",
    "typ_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky",
        "https://www.kudyznudy.cz/co-chcete-delat/zivotni-styl"
    ],
    "kategorie_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky/cirkevni-pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/organizovane-vylety-a-exkurze",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/netradicni-prohlidky"
    ],
    "id": "630586e5-9541-4099-84ef-f8a35dc8b745",
    "iri": "https://www.kudyznudy.cz/aktivity/komentovane-prohlidky-arealu-zirec-a-domova-sv-jos",
    "název": {
        "cs": "Bylinkové zahrady Žíreč – komentované prohlídky v areálu Žireč a Domova sv. Josefa"
    },
    "vytvořeno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-10-10T10:21:06.7089805"
    },
    "aktualizováno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-10-10T11:44:32.4484005"
    },
    "popis": {
        "cs": "Zažijte příběh Areálu Žireč. Navštívíte jedinečné cyklomuzeum, kavárnu i bylinkovou zahradu v místech bývalé jezuitské rezidence. Zaposloucháte se do zvuku unikátní zvonkohry a projdete se po místech, kde působili jezuité. Poznáte dávnou historii i nový příběh pomoci lidem s roztroušenou sklerózou."
    },
    "dlouhý_popis": {
        "cs": "<a href=\"~/aktivity/barokni-areal-zirec\">Areál Žireč</a> se nachází v části obce Žireč ve Dvoře Králové nad Labem. Můžete navštívit krásnou bylinkovou zahradu a posedět v ní s šálkem kávy z kavárny Damián nebo se projít v rozlehlém přírodním parku. Areál je zároveň domovem pro lidi nemocné roztroušenou sklerózou mozkomíšní, kteří žijí v Domově sv. Josefa. Hlavní budova Domova je bývalou barokní jezuitskou rezidencí. Svojí návštěvou podpoříte péči o nemocné i lidi se sníženou pracovní schopností, kteří vás mohou obsloužit v jednotlivých provozech. Návštěvy a komentované prohlídky jsou možné domluvit po celý rok.<br />\r\n<br />\r\nV Areálu naleznete největší cykloexpozici ve střední Evropě. V kostele sv. Anny se nalézá unikátní barokní zvonový klavír na světě &ndash; jediný svého druhu, dále vzácné varhany, jeden z mála dochovaných deskových jezuitských betlémů a mnoho dalších zajímavých předmětů."
    },
    "aktivní_česko": false,
    "bezbariériový_přístup": true,
    "ekologicky_šetrné": false,
    "vhodné_pro_děti": true,
    "vhodné_pro_dospělé": false,
    "vhodné_pro_firmení_akce": false,
    "vhodné_pro_zvířata": true,
    "za_každého_počasí": true,
    "časová_náročnost": 1.5,
    "registrace": null,
    "pořadatel": {
        "typ": "Osoba",
        "název": {
            "cs": "Areál Žireč, Domov sv. Josefa"
        },
        "geometrie": {
            "typ": "Lokalita",
            "souřadnice": [
                50.413444519042969,
                15.853754043579102
            ],
            "kraj": {
              "cs": "Královéhradecký kraj"
            },
            "oblast": {
              "cs": "Podkrkonoší – Podzvičinsko"
            },
            "město": {
              "cs": "Dvůr Králové nad Labem"
            }
        },
        "adresa": {
            "typ": "Kontaktní adresa",
            "ulice": "Žireč 1",
            "obec": "Dvůr Králové nad Labem",
            "psč": "54404",
            "kontakt": {
                "typ": "Kontakt",
                "email": "mailto:kollova@dsj-zirec.cz",
                "mobil": "tel:+420491610533",
                "facebook": "https://www.facebook.com/arealzirec",
                "instagram": null,
                "twitter": null,
                "url": "https://www.arealzirec.cz"
            }
        },
        "kontaktní_osoba": {
            "typ": "Kontaktní osoba",
            "jméno": "Andrea Kollová",
            "email": "mailto:kollova@dsj-zirec.cz",
            "mobil": "tel:+420730144334"
        }
    },
    "umístění": [],
    "kategorie_vstupného": {
        "typ": "Vstupné",
        "cena": {
            "typ": "Částka",
            "výše": 130.0,
            "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
        },
        "snížená_cena": {
            "typ": "Částka",
            "výše": 90.0,
            "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
        },
        "rodinné_vstupné": null,
        "poznámka": {
            "cs": "Nebo pouze kostel 50/30 či cykloexpozice 90/60 Info a rezervace na 731 604 204"
        }
    },
    "otevírací_doba": null,
    "příloha": [
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/a9/a9afa917-20d3-44f7-83be-4abf7f5ceccb.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/17/17609ec2-a7bf-46c7-bfe0-5dea754c9365.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Zvonový klavír"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/52/5219812f-c614-4114-a321-2b6b30d288f2.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0b/0b5e7a00-6158-4e2b-9e35-041cb719d7cd.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Kavárna Damián"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/4b/4b9637d3-70d6-4fc7-9e91-2d5dd68f7cd6.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/a0/a0791a5c-84f0-416e-b460-cb746baf3871.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Expozice cyklistiky"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/1c/1c2cd7d9-36d8-4a7b-9705-f7b4b8b224e7.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Kostel sv. Anny"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/5f/5ff23727-9b12-46d6-858e-ddd92fc20795.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Areál Žireč"
            },
            "popis": null
        }
    ],
    "certifikace": [
    {
      "typ": "Certifikace",
      "id": "84cdadc6-1e81-4040-9b86-9fd3f6afbb2c",
      "název": {
        "cs": "aktivní Česko"
      }
    },
    {
      "typ": "Certifikace",
      "id": "bc0f8dd4-243f-4ff8-97df-858ec5b3343c",
      "název": {
        "cs": "ekologické cestování"
          }
        }
      ]
    }

| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil.jsonld](https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil.jsonld)) |
| typ | Turistický cíl |
| typ_turistického_cíle | Pole typů turistického cíle (první je hlavní) |
| kategorie_turistického_cíle | Pole kategorií typů turistických cílů |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |
| popis | Krátká anotace (perex) – prostý text |
| dlouhý_popis | HTML text pokračující za krátkým popisem |
| aktivní_česko | Aktivní Česko (true / false) |
| bezbariériový_přístup | Bezbariérový přístup (true / false) |
| ekologicky_šetrné | Ekologicky šetrné (true / false) |
| vhodné_pro_děti | Vhodné pro děti (true / false) |
| vhodné_pro_dospělé | Vhodné pro dospělé (true / false) |
| vhodné_pro_firmení_akce | Vhodné pro firemní akce (true / false) |
| vhodné_pro_zvířata | Vhodné pro zvířata (true / false) |
| za_každého_počasí | Za každého počasí (true / false) |
| časová_náročnost | Počet hodin potřebných k návštěvě místa |
| registrace | URL adresa registrace na externím webu |
| pořadatel | Adresa, a kontakt na pořadatele |
| umístění | Místa konání (adresy a kontakty) |
| kategorie_vstupného | Ceny jednotlivých typů vstupného |
| otevírací_doba | Definice otevírací doby v jednotlivých dnech |
| příloha | Pole obrázků a jejich metadat |
| certifikace | Seznam platných certifikací záznamu |

Veřejná část objektu (v případě omezeného Public přístupu):

    {
    "@context": "https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil-public.jsonld",
    "typ": "Turistický cíl",
    "typ_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky",
        "https://www.kudyznudy.cz/co-chcete-delat/zivotni-styl"
    ],
    "kategorie_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky/cirkevni-pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/organizovane-vylety-a-exkurze",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/netradicni-prohlidky"
    ],
    "id": "bb3f60b0-c668-4f31-a13c-780bb1b7c87f",
    "iri": "https://www.kudyznudy.cz/aktivity/poutni-kostel-sv-jana-nepomuckeho-na-zelene-hore",
    "název": {
        "cs": "Zelená hora – poutní kostel sv. Jana Nepomuckého u Žďáru n. S."
    },
    "vytvořeno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2010-04-11T10:21:57"
    },
    "aktualizováno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-07-14T05:32:48.4597958"
    },
    "popis": {
        "cs": "Kostel sv. Jana Nepomuckého na Zelené hoře ve Žďáru nad Sázavou je národní kulturní památka zapsaná na Seznamu světového dědictví UNESCO. Byl postaven na začátku 20. let 18. století a je nepochybně nejosobitějším dílem J. B. Santiniho Aichla."
        },
    "geometrie": {
        "typ": "Lokalita",
        "souřadnice": [
                49.5828245,
                15.9375815
        ],
        "kraj": {
          "cs": "Kraj Vysočina"
        },
        "oblast": {
          "cs": "Koruna Vysočiny"
        },
        "město": {
          "cs": "Žďár nad Sázavou"
        }
    },
    "příloha": [
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/8b/8b880b32-2c6d-49d1-816b-9e0f3d165fa2.jpg?v=20220714053247",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
            "název": {
                "cs": "Poutmí kostel ve Ždáru_foto Libor Svácek"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/f3/f30fbc88-9eb2-4a03-96b3-a69a36bfb79c.jpg?v=20220714053247",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
            "název": {
                "cs": "Autor Chris Borg"
            },
            "popis": {
                "cs": "Autor Chris Borg"
            }
        }
        ],
    "certifikace": [
        {
          "typ": "Certifikace",
          "id": "84cdadc6-1e81-4040-9b86-9fd3f6afbb2c",
          "název": {
            "cs": "aktivní Česko"
          }
        },
        {
          "typ": "Certifikace",
          "id": "bc0f8dd4-243f-4ff8-97df-858ec5b3343c",
          "název": {
            "cs": "ekologické cestování"
          }
        }
      ]
    }

| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil-public.jsonld](https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil-public.jsonld)) |
| typ | Turistický cíl |
| typ_turistického_cíle | Pole typů turistického cíle (první je hlavní) |
| kategorie_turistického_cíle | Pole kategorií typů turistických cílů |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |
| popis | Krátká anotace (perex) – prostý text |
| geometrie | Lokalita a souřadnice GPS |
| příloha | Pole obrázků a jejich metadat |
| certifikace | Seznam platných certifikací záznamu |

## Akce (Událost)

Akce (událost) je dalším typem obsahu publikovaného na portálu Kudy z nudy. Vyznačuje se časově omezenou dobou trvání.

Akce naleznete v sekci portálu [Kalendář akcí](https://www.kudyznudy.cz/kalendar-akci).

### Seznam akcí (událostí)

Metoda vrací seznam publikovaných akcí.

Seznam akcí získáte voláním:

    curl --location --request GET 'https://api.kudyznudy.cz/content/events' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'

Získání akcí změněných po určitém čase pak pomocí volání:

    curl --location --request POST 'https://api.kudyznudy.cz/content/events' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "date": "2022-10-10T09:23:40.085Z"
    }'

Výstup služeb seznamu akcí – pole JSON objektů:

    [
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Událost",
            "id": "e5ac8753-ef5d-478c-96f7-7e804c5ba4d2",
            "iri": "https://www.kudyznudy.cz/akce/tantra-joga-mohendzodaro-zena-kraska-zena-osklivka",
            "název": {
                "cs": "Mohendžodáro – žena kráska – žena ošklivka"
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-06T19:07:08.1191666"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T12:37:01.712556"
            }
        },
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Událost",
            "id": "bb96c39e-3b0f-4035-877b-befe1b684536",
            "iri": "https://www.kudyznudy.cz/akce/dynovy-podzim-v-trojske-botanicke-zahrade",
            "název": {
                "cs": "Dýňový podzim v trojské botanické zahradě "
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2016-08-24T17:25:36"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T12:34:16.884373"
            }
        }
    ]

| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Událost |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |

### Detail akce (události)

Služba pro získání detailu publikované akce:

    curl --location --request POST 'https://api.kudyznudy.cz/content/events' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc"
    }'

Výstupem služby detailu akce je JSON objekt. Podle typu přístupu je uživateli vrácen plnohodnotný objekt nebo jenom jeho veřejná část.

Plnohodnotný objekt (v případně plného přístupu):

    {
        "@context": "https://www.kudyznudy.cz/kzn/context/v1/udalost.jsonld",
        "typ": "Událost",

    "doba_trvání": [
            {
                "typ": "Časová specifikace",

    "časový_interval": {
                    "typ": "Časový interval",
                    "začátek": {
                        "typ": "Časový okamžik",

    "datum_a_čas": "2022-10-29T10:00:00"
                    },
                    "konec": {
                        "typ": "Časový okamžik",

    "datum_a_čas": "2022-10-30T17:00:00"
                    }
                }
            }
        ],

    "typ_události": [
            "https://www.kudyznudy.cz/kalendar-akci/festivaly",
            "https://www.kudyznudy.cz/kalendar-akci/vystavy"
        ],
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc",
        "iri": "https://www.kudyznudy.cz/akce/prague-car-festival-2021",
        "název": {
            "cs": "Prague Car Festival 2022"
        },
        "vytvořeno": {
            "typ": "Časový okamžik",

    "datum_a_čas": "2021-09-21T18:57:59.4463373"
        },
        "aktualizováno": {
            "typ": "Časový okamžik",

    "datum_a_čas": "2022-06-03T02:33:10.3900509"
        },
        "popis": {
            "cs": "Největší výstava ve střední a východní Evropě se zaměřením na tuning, motorsport, historické automobily a motocykly."
        },

    "dlouhý_popis": {
            "cs": "Jedenáctý ročník akce evropského významu nabídne vystavovatelům i návštěvníkům \<strong\>jedinečnou podívanou \</strong\>na široké spektrum unikátních automobilů a motocyklů. Výstava spojuje oblasti upravených a sportovních vozů, světa motorsportu a historie vozidel. \<strong\>Prague Car Festival\</strong\> představí na ploše přes 30 000 m\<sup\>2\</sup\> \<strong\>nejlepší tuning z celé Evropy\</strong\>, českou i zahraniční špičku automobilových a motocyklových závodů a průřez bohatou historií vývoje automobilů a motocyklů \<strong\>od předválečného období až po současnost\</strong\>.\r\n\r\n\<h3\>\<br /\>\r\n\<strong\>BSR Tuning Expo\</strong\>\</h3\>\r\nVýstava BSR Tuning Expo nabízí skvosty z české tuningové scény a celé Evropy, profesionální tuningové firmy, sportovní automobily a americké vozy tvoří hlavní obsahovou část výstavních hal. Návštěvníci uvidí přes 500 jedinečných exponátů. K tomu soutěže, workshopy, akční jízdy a srazy na venkovní ploše a také doprovodný program pro celou rodinu vytvářejí jedinečnou příležitost pro návštěvníky všech generací."
        },

    "aktivní_česko": false,
    
    "bezbariériový_přístup": true,
    
    "ekologicky_šetrné": false,
    
    "smetana_200": false,
    
    "vhodné_pro_děti": true,
    
    "vhodné_pro_dospělé": false,
    
    "vhodné_pro_firmení_akce": false,
    
    "vhodné_pro_zvířata": true,
    
    "za_každého_počasí": true,

    "časová_náročnost": 2,
        "registrace": {
            "cs": "https://praguecarfestival.cz/vstupenky/"
        },
        "pořadatel": {
            "typ": "Osoba",
            "název": {
                "cs": "Prague Car Festival s.r.o."
            },
            "geometrie": {
                "typ": "Lokalita",
                "souřadnice": [
                    50.12938386099312,
                    14.551111294871998
                ],
                "kraj": {
                  "cs": "Praha"
                },
                "oblast": {
                  "cs": "Praha"
                },
                "město": {
                  "cs": "Praha 9"
                }                
            },
            "adresa": {
                "typ": "Kontaktní adresa",
                "ulice": "Hornopočernická 13",
                "obec": "Praha",
                "psč": "19700",
                "kontakt": {
                    "typ": "Kontakt",
                    "email": "mailto:office@praguecarfestival.cz",
                    "mobil": "tel:+420608320336",
                    "facebook": null,
                    "instagram": null,
                    "twitter": null,
                    "url": "https://praguecarfestival.cz/"
                }
            },

    "kontaktní_osoba": {
                "typ": "Kontaktní osoba",
                "jméno": "Martin Apeltauer",
                "email": "mailto:apeltauerm@gmail.com",
                "mobil": "tel:+420608320336"
            }
        },
        "umístění": [
            {
                "typ": "Místo konání",
                "název": {
                    "cs": "PVA EXPO PRAHA Letňany"
                },
                "geometrie": {
                    "typ": "Lokalita",
                    "souřadnice": [
                        50.129423083832535,
                        14.514344635522487
                    ],
                    "kraj": {
                      "cs": "Praha"
                    },
                    "oblast": {
                      "cs": "Praha"
                    },
                    "město": {
                      "cs": "Praha 9"
                    }
                },
                "adresa": {
                    "typ": "Kontaktní adresa",
                    "ulice": "Beranových 667",
                    "obec": "Praha 9",
                    "psč": "19900"
                },

    "kontaktní_osoba": null
            }
        ],

    "kategorie_vstupného": {
            "typ": "Vstupné",
            "cena": {
                "typ": "Částka",
                "výše": 300,
                "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
            },

    "snížená_cena": {
                "typ": "Částka",
                "výše": 200,
                "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
            },

    "rodinné_vstupné": null,
            "poznámka": {
                "cs": "Předprodej on-line za zvýhodněnou cenu."
            }
        },

    "otevírací_doba": null,
        "příloha": [
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/70/70d1a90e-d5c2-4fd1-becb-c531e3ec65be.jpg?v=20220603023310",

    "autor_díla": null,

    "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            },
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0f/0ff52200-7302-4553-8412-81a307c00d4b.jpg?v=20220603023310",

    "autor_díla": null,

    "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            }
        ],
    "certifikace": [
        {
          "typ": "Certifikace",
          "id": "84cdadc6-1e81-4040-9b86-9fd3f6afbb2c",
          "název": {
            "cs": "aktivní Česko"
          }
        },
        {
          "typ": "Certifikace",
          "id": "bc0f8dd4-243f-4ff8-97df-858ec5b3343c",
          "název": {
            "cs": "ekologické cestování"
          }
        }
      ]
    }

| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/udalost.jsonld](https://www.kudyznudy.cz/kzn/context/v1/udalost.jsonld)) |
| typ | Událost |
| doba_trvání | Začátek a konec akce |
| typ_události | Pole typů události (první je hlavní) |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |
| popis | Krátká anotace (perex) – prostý text |
| dlouhý_popis | HTML text pokračující za krátkým popisem |
| aktivní_česko | Aktivní Česko (true / false) |
| bezbariériový_přístup | Bezbariérový přístup (true / false) |
| ekologicky_šetrné | Ekologicky šetrné (true / false) |
| smetana_200 | Projekt Smetana200 (true / false) |
| vhodné_pro_děti | Vhodné pro děti (true / false) |
| vhodné_pro_dospělé | Vhodné pro dospělé (true / false) |
| vhodné_pro_firmení_akce | Vhodné pro firemní akce (true / false) |
| vhodné_pro_zvířata | Vhodné pro zvířata (true / false) |
| za_každého_počasí | Za každého počasí (true / false) |
| časová_náročnost | Počet hodin potřebných k návštěvě místa |
| registrace | URL adresa registrace na externím webu |
| pořadatel | Adresa, a kontakt na pořadatele |
| umístění | Místa konání (adresy a kontakty) |
| kategorie_vstupného | Ceny jednotlivých typů vstupného |
| otevírací_doba | Definice otevírací doby v jednotlivých dnech |
| příloha | Pole obrázků a jejich metadat |
| certifikace | Seznam platných certifikací záznamu |

Veřejná část objektu (v případě omezeného Public přístupu):

    {
        "@context": "https://www.kudyznudy.cz/kzn/context/v1/udalost-public.jsonld",
        "typ": "Událost",
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc",
        "iri": "https://www.kudyznudy.cz/akce/prague-car-festival-2021",
        "název": {
            "cs": "Prague Car Festival 2022"
        },
        "doba_trvání": [
            {
                "typ": "Časová specifikace",
                "časový_interval": {
                    "typ": "Časový interval",
                    "začátek": {
                        "typ": "Časový okamžik",
                        "datum_a_čas": "2022-10-29T10:00:00"
                    },
                    "konec": {
                        "typ": "Časový okamžik",
                        "datum_a_čas": "2022-10-30T17:00:00"
                    }
                }
            }
        ],
        "typ_události": [
            "https://www.kudyznudy.cz/kalendar-akci/festivaly",
            "https://www.kudyznudy.cz/kalendar-akci/vystavy"
        ],
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc",
        "iri": "https://www.kudyznudy.cz/akce/prague-car-festival-2021",
        "název": {
            "cs": "Prague Car Festival 2022"
        },
        "vytvořeno": {
            "typ": "Časový okamžik",
            "datum_a_čas": "2021-09-21T18:57:59.4463373"
        },
        "aktualizováno": {
            "typ": "Časový okamžik",
            "datum_a_čas": "2022-06-03T02:33:10.3900509"
        },
        "popis": {
            "cs": "Největší výstava ve střední a východní Evropě se zaměřením na tuning, motorsport, historické automobily a motocykly."
        },
        "geometrie": {
            "typ": "Lokalita",
            "souřadnice": [
                50.12938386099312,
                14.551111294871998
            ],
            "kraj": {
              "cs": "Praha"
            },
            "oblast": {
              "cs": "Praha"
            },
            "město": {
              "cs": "Praha 9"
            }
        },
        "příloha": [
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/70/70d1a90e-d5c2-4fd1-becb-c531e3ec65be.jpg?v=20220603023310",
                "autor_díla": null,
                "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            },
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0f/0ff52200-7302-4553-8412-81a307c00d4b.jpg?v=20220603023310",
                "autor_díla": null,
                "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            }
        ],
        "certifikace": [
        {
          "typ": "Certifikace",
          "id": "84cdadc6-1e81-4040-9b86-9fd3f6afbb2c",
          "název": {
            "cs": "aktivní Česko"
          }
        },
        {
          "typ": "Certifikace",
          "id": "bc0f8dd4-243f-4ff8-97df-858ec5b3343c",
          "název": {
            "cs": "ekologické cestování"
          }
        }
      ]
    }

| **Hodnota** | **Popis hodnoty** |
| --- | --- |
| @context | Popis formátu dat – šablona jsonld (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/udalost-public.jsonld](https://www.kudyznudy.cz/kzn/context/v1/udalost-public.jsonld)) |
| typ | Událost |
| doba_trvání | Začátek a konec akce |
| typ_události | Pole typů události (první je hlavní) |
| id | Unikátní GUID záznamu |
| iri | Unikání adresa záznamu (URL detailu článku na Kudy z nudy) |
| název | Název článku (titulek) |
| vyvořeno | Datum a čas vytvoření |
| aktualizováno | Datum a čas poslední změny |
| popis | Krátká anotace (perex) – prostý text |
| geometrie | Lokalita a souřadnice GPS |
| příloha | Pole obrázků a jejich metadat |
| certifikace | Seznam platných certifikací záznamu |
