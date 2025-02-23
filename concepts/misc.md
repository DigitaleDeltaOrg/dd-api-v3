### Parameter vs Result

#### Parameter

- Beschrijvende eigenschappen van de observatie
- Bijvoorbeeld: geslacht, levensstadium, habitat
- Opgeslagen in `parameter` structuur
- Wat in Aquo 'parameter' wordt genoemd, zit in DD API V3 onder `parameter/parameter`. Dit kan in het begin verwarrend zijn

Waarom is hiervoor gekozen? De rol van observedProperty is context-gevoelig.
Geobserveerde parameter gebruiken als observedProperty is ook niet altijd mogelijk, omdat er grootheden zijn zónder parameter, zoals pH.

> Discussie: grootheid is **altijd** aanwezig. Kan deze worden verheven tot observedProperty? Is dan bijvoorbeeld aantal per oppervlakte dan bruikbaar?

#### Metadata

- Voor opslaan van 'vrije' data
- De namen van de eigenschappen liggen wel vast

#### Result (Meetwaarden)
1. **Measure**
    - Gemeten waarde met eenheid
    - Voorbeeld: `{ "result": { "measure": 23.5, "unit": "°C" } }`

2. **Truth**
    - Ja/Nee waarde
    - Voorbeeld: `{ "value": true }`
    - Truth zal niet vaak gebruikt worden. Aquo-conforme metingen maken vaak gebruik van kwalificaties hiervoor

3. **Vocab**
    - Waarde uit gecontroleerd woordenboek
    - Voorbeeld: `{ "value": "aanwezig" }`
    - Vocab zal niet vaak gebruikt worden. Aquo-conforme metingen maken vaak gebruik van kwalificaties hiervoor

4. **Count**
    - Numerieke waarde zonder eenheid
    - Voorbeeld: `{ "value": 42 }`
        - Count zal niet vaak gebruikt worden. Aquo-conforme metingen maken vaak gebruik van eenheid `DIMSLS`  (dimensieloos)

5. **CoverageJSON**
    - Voor tijdsreeksen, verwachtingen en ensembles
    - Gestructureerd volgens CoverageJSON formaat

#### OData Query Voorbeelden op parameter
| Type      | Query Voorbeeld                               |
|-----------|-----------------------------------------------|
| Parameter | `$filter=parameter/parameter eq 'Abies alba'` |
| LifeStage | `$filter=parameter/lifestage ne 'JU'`         |
| Quantity  | `$filter=parameter/quantity eq 'AANTL'`       |
| TaxonType | `$filter=parameter/taxontype ne 'FYTPT'`      |

#### OData Query Voorbeelden op feature of interest (meetobject)
| Type    | Query Voorbeeld                                                      |
|---------|----------------------------------------------------------------------|
| Measure | `$filter=result/measure/value gt 20 and result/measure/unit eq '°C'` |
| Truth   | `$filter=result/truth eq true`                                       |
| Count   | `$filter=result/count gt 100`                                        |

#### OData Query Voorbeelden op resultaten
| Type    | Query Voorbeeld                                                      |
|---------|----------------------------------------------------------------------|
| Measure | `$filter=result/measure/value gt 20 and result/measure/unit eq '°C'` |
| Truth   | `$filter=result/truth eq true`                                       |
| Count   | `$filter=result/count gt 100`                                        |

#### OData Query Voorbeelden op tijd
| Type    | Query Voorbeeld                                                      |
|---------|----------------------------------------------------------------------|
| Measure | `$filter=result/measure/value gt 20 and result/measure/unit eq '°C'` |
| Truth   | `$filter=result/truth eq true`                                       |
| Count   | `$filter=result/count gt 100`                                        |

#### Measure Voorbeeld

...

#### Tijdsreeks Voorbeeld
```json
{
    "type": "Coverage",
    "domain": {
        "type": "Domain",
        "axes": {
            "t": {
                "values": ["2024-03-20T10:00:00Z", "2024-03-20T11:00:00Z", "2024-03-20T12:00:00Z"]
            }
        }
    },
    "ranges": {
        "temperatuur": {
            "values": [18.5, 19.2, 20.1],
            "unit": "°C"
        }
    }
}
```

#### Verwachting (Probabilistisch) Voorbeeld
```json
{
    "type": "Coverage",
    "domain": {
        "type": "Domain",
        "axes": {
            "t": {
                "values": ["2024-03-21T00:00:00Z", "2024-03-21T06:00:00Z"]
            },
            "realization": {
                "values": [1, 2, 3]
            }
        }
    },
    "ranges": {
        "waterhoogte": {
            "values": [
                [1.2, 1.3, 1.4],
                [1.3, 1.5, 1.6]
            ],
            "unit": "m"
        }
    }
}
```

#### OData Query Voorbeelden
| Scenario                             | Query                                                                     |
|--------------------------------------|---------------------------------------------------------------------------|
| Tijdsreeks waarden boven drempel     | `$filter=result/ranges/temperatuur/values/any(v: v gt 20)`                |
| Specifieke tijdstip in domain        | `$filter=result/domain/axes/t/values/any(t: t eq '2024-03-20T10:00:00Z')` |
| Verwachting met minimale waterhoogte | `$filter=result/ranges/waterhoogte/values/any(v: v/all(r: r gt 1.0))`     |

### Toepassingen
- Tijdreeksen: waterstand, temperatuur, luchtkwaliteit
- Verwachtingen: weersverwachting, waterstanden, getijden
- Ensembles (meetreeks-bundels, modellenreeks): klimaatscenario's, modelensembles

### Voordelen

- Eenvoudig te implementeren
- Voorspelbaar gedrag bij uitwisseling
- Makkelijk te valideren
- Efficiënte opslag en verwerking