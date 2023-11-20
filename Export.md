# Export dat z Kudy z nudy

Tento dokument popisuje v jakém formátu je možné data jednoduše exportovat z portálu Kudy z nudy. Portál Kudy z nudy podporuje export pro tyto typy obsahu: akce a aktivity. Hlavním rozdílem typů je doba trvání. Akce mají omezenou dobu trvání (v určitý okamžik začínají a v určitý končí). Aktivity jsou časově neomezené – jedná se o tzv. Turistické cíle. Z pohledu dat jsou si typy velmi podobné. Rozdílem je pouze existence doby trvání u akcí a jiná kategorizace u typu akce a jiná u typu aktivity.

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

Výstupem je JSON objekt:

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

Výstupem služby detailu akce je JSON objekt:

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
