<link rel="stylesheet" type="text/css" href="/custom.css">

# Datamodel Digitale Delta API V3

## Inhoudsopgave
- [Samenvatting](#samenvatting)
- [Introductie](#introductie)
- [Hoofdcomponenten](#hoofdcomponenten)
- [Gedetailleerde Beschrijving](#gedetailleerde-beschrijving)
    - [Referentiestructuur](#referentiestructuur)
    - [Observatiestructuur](#observatiestructuur)
- [Data Types](#data-types)
- [Resultaat Types](#result)
- [Bijzonderheden](#bijzonderheden)
- [Praktijkvoorbeelden](#praktijkvoorbeelden)
- [Woordenlijst](#woordenlijst)

<a id="samenvatting"></a>
## Samenvatting
De Digitale Delta API V3 faciliteert de uitwisseling van watergerelateerde meetgegevens tussen verschillende organisaties in Nederland. Het uitwisselmodel bestaat uit twee kerncomponenten:

- **Referenties**: Definieert de structuur en context van metingen
- **Observaties**: Bevat de daadwerkelijke meetgegevens

Het model is gebaseerd op internationale standaarden (OM&S en OData) en biedt een flexibele maar gestructureerde manier om waterdata uit te wisselen.

<a id="introductie"></a>
## Introductie
Het datamodel in DD API V3 is een virtueel datamodel dat de invoer- en uitvoerkant beschrijft, zonder zich bezig te houden met opslagstructuren.

### Doel
De Digitale Delta API maakt het mogelijk om watergerelateerde meetgegevens uit te wisselen tussen verschillende organisaties, zoals:
- Waterstanden
- Waterkwaliteit
- Neerslag
- Andere watermetingen

### Scope
Het document vermeldt de term 'data-model', maar het is feitelijk een uitwisselmodel, gebaseerd op OM&S en OData. 
Het specificeert **niet** hoe data moet worden opgeslagen, maar beschrijft wat **de consument** te zien krijgt.

<a id="hoofdcomponenten"></a>
## Hoofdcomponenten

Het model bestaat uit twee hoofdonderdelen: Referenties en Observaties. Je kunt het vergelijken met een recept:
- Referenties zijn de ingrediëntenlijst en keukenspullen
- Observaties zijn de metingen die we daarmee doen

<img src="/concepts/data-model.svg" width="100%" alt="Data model DD API V3"/>

<a id="gedetailleerde-beschrijving"></a>
## Gedetailleerde Beschrijving

<a id="referentiestructuur"></a>
### Referentiestructuur
Referenties vormen het woordenboek voor DD API V3. Ze beschrijven wat het systeem kent, zoals eenheden, grootheden, parameters en meetobjecten.

| Eigenschap                | Type              | Betekenis                                                                       |
|---------------------------|-------------------|---------------------------------------------------------------------------------|
| **Id**                    | [Text](#text)     | Uniek Id van de referentie                                                      |
| **Type**                  | [Text](#text)     | Type referentie (bijv. Quantity of MeasurementObject)                           |
| **Code**                  | [Text](#text)     | Code van de referentie                                                          |
| **Description**           | [Text](#text)     | Omschrijving                                                                    |
| **Organisation**          | [Text](#text)     | Beherende organisatie                                                           |
| **Geography**             | GeoJSON           | Geografie in GeoJSON (verplicht voor meetobjecten en optioneel voor meetnetten) |
| **OrganisationNamespace** | [Text](#text)     | Officiële Aquo-code waterbeheerder                                              |
| _Details_                 | _JSON, optioneel_ | _Vrije details over de referentie_                                              |

<a id="observatiestructuur"></a>
### Observatiestructuur
Observations zijn gebaseerd op de ISO/OGC standaard [<img src="../../external.svg" with="15" height="15">OM&S](https://www.ogc.org/standard/om/).

1. Een enkele meetwaarde (bijvoorbeeld een waterstand op één tijdstip)
2. Een tijdreeks/verwachting/ensemble in CoverageJSON formaat (bijvoorbeeld waterstanden over een periode)
3. Een berekend resultaat

De relatie tussen observaties (`RelatedObservation`) wordt dan gebruikt om bijvoorbeeld:


| Eigenschap           | Type                                | Betekenis                                           |
|----------------------|-------------------------------------|-----------------------------------------------------|
| **Id**               | [Text](#text)                       | Unieke identificatie                                |
| **ResultTime**       | [DateTime](#datetime)               | Tijdstip van meting                                 |
| **PhenomenonTime**   | [TimePeriod](#timeperiod)           | Periode van monstername                             |
| _ValidTime_          | [TimePeriod](#timeperiod)           | Geldigheidsperiode                                  |
| **Type**             | [Text](#text)                       | Type resultaat (count/truth/measure/vocab/coverage) |
| **FoI**              | [Reference](#reference)             | Meetobject/meetlocatie                              |
| **Parameter**        | [ReferenceList](#referencelist)     | Gestandaardiseerde context                          |
| _Metadata_           | [ValueList](#valuelist)             | Niet-gestandaardiseerde context                     |
| **Result**           | [Result](#result)                   | Meetresultaat                                       |
| _RelatedObservation_ | [ObservationList](#observationlist) | Gerelateerde observaties                            |

Eigenschappen in **vet** zijn verplicht.

<a id="datatypes"></a>
## Data Types
### Basis types

<a id="text"></a>
#### Text
Een reeks letters, cijfers en symbolen. Vermijd speciale tekens en aanhalingstekens.

<a id="datetime"></a>
#### DateTime
Datum/tijd volgens [<img src="../../external.svg" with="15" height="15">ISO 8601](https://nl.wikipedia.org/wiki/ISO_8601) standaard.

### Samengestelde types
<a id="timeperiod"></a>
#### TimePeriod
Een datumbereik met:
- BeginPosition: starttijdstip
- EndPosition: eindtijdstip
  Beide volgens ISO 8601.
- TimePeriod komt uit de GML standaard

<a id="reference"></a>
#### Reference
Een veldnaam/veldwaarde-combinatie die verwijst naar een definitie in de referentielijst.

<a id="referencelist"></a>
#### ReferenceList
Een lijst van Reference objecten. Wordt gebruikt om meerdere referenties te groeperen.

<a id="coverage"></a>
#### Coverage
Een Coverage is een flexibele datastructuur voor complexe meetgegevens volgens [<img src="../../external.svg" with="15" height="15">CoverageJSON](https://docs.ogc.org/cs/21-069r2/21-069r2.html). Deze wordt gebruikt in drie hoofdscenario's:

##### 1. Tijdreeksen
Voor opeenvolgende metingen over tijd, bijvoorbeeld uurlijkse temperatuurmetingen.
- Heeft één tijdsas ('t')
- Bevat één waarde per tijdstip
- Geschikt voor historische data

##### 2. Verwachtingen
Voor voorspellingen met mogelijk meerdere scenario's.
- Heeft een tijdsas ('t')
- Kan een realization-as hebben voor verschillende scenario's
- Geschikt voor weersverwachtingen of modelberekeningen

##### 3. Ensembles
Voor sets van gerelateerde metingen of berekeningen.
- Combineert meerdere dimensies
- Kan zowel tijd als andere parameters bevatten
- Geschikt voor complexe modeluitkomsten

Belangrijke onderdelen:
- **domain**: definieert de assen (tijd, locatie, scenario's)
- **ranges**: bevat de daadwerkelijke meetwaarden
- **unit**: specificatie van de gebruikte eenheid

<a id="valuelist"></a>
#### ValueList
Een lijst van key-value paren waarbij:
- key: moet voorkomen in de referentielijst
- value: vrij formaat, kan ook complexe JSON bevatten

<a id="result"></a>
## Result

Resultaten kunnen verschillende vormen hebben:

### Measure
Bevat een gemeten waarde en bijbehorende eenheid.

#### Voorbeeld

```json
{ 
    "result": { 
        "measure": 23.4, 
        "uom": "C" 
    }
}
```

### Count
Bevat alleen een geheel getal.

#### Voorbeeld

```json
{ 
    "result": { 
        "count": 123
    }
}
```

### Vocab
Bevat een type en waarde combinatie uit een vocabulaire.

#### Voorbeeld

```json
{ 
    "result": { 
        "vocab": "KLEUR",
        "verb": "GEEL"
    }
}
```

### Truth
Bevat true (waar) of false (onwaar).

#### Voorbeeld

```json
{ 
    "result": { 
        "truth": "true"
    }
}
```

### Coverage
Complexe datastructuur voor tijdreeksen, verwachtingen en ensembles volgens [<img src="../../external.svg" with="15" height="15">CoverageJSON](https://docs.ogc.org/cs/21-069r2/21-069r2.html).

#### Voorbeeld tijdreeks
```json
{
    "result": {
        "type": "Coverage",
        "domain": {
            "type": "Domain",
            "axes": {
                "t": {
                    "values": [
                        "2024-03-20T10:00:00Z",
                        "2024-03-20T11:00:00Z",
                        "2024-03-20T12:00:00Z"
                    ]
                }
            }
        },
        "ranges": {
            "temperatuur": {
                "values": [
                    18.5,
                    19.2,
                    20.1
                ],
                "unit": "°C"
            }
        }
    }
}
```

#### Voorbeeld Verwachting
```json
{
    "result": {
        "type": "Coverage",
        "domain": {
            "type": "Domain",
            "axes": {
                "t": {
                    "values": [
                        "2024-03-21T00:00:00Z",
                        "2024-03-21T06:00:00Z"
                    ]
                },
                "realization": {
                    "values": [
                        1,
                        2,
                        3
                    ]
                }
            }
        },
        "ranges": {
            "waterhoogte": {
                "values": [
                    [
                        1.2,
                        1.3,
                        1.4
                    ],
                    [
                        1.3,
                        1.5,
                        1.6
                    ]
                ],
                "unit": "m"
            }
        }
    }
}
```

<a id="relatedobservation"></a>
#### RelatedObservation

Beschrijft de relatie tussen observaties. Een observatie kan het resultaat zijn van andere observaties (bijvoorbeeld bij aggregaties) of anderszins gerelateerd zijn.

Properties:
- **role** (string): Beschrijft de aard van de relatie (bijvoorbeeld: "derivedFrom", "partOf", "qualityControl")
- **target** (string[]): Array van IDs van de gerelateerde observaties

Voorbeeld:
```json
{
    "role": "derivedFrom",
    "target": ["obs-123", "obs-124", "obs-125"] 
}
```

## Veelvoorkomende Valkuilen

### DateTime formaat
- Gebruik altijd UTC (aangeduid met 'Z')
- Vermijd lokale tijdzones
- Gebruik consistente precisie (seconden of milliseconden)

### Referenties
- Controleer of referenties bestaan vóór gebruik
- Let op hoofdlettergevoeligheid
- Gebruik alleen toegestane types

### Coverage gebruik
- Zorg dat arrays in 'values' en tijdstippen even lang zijn
- Gebruik de juiste dimensies voor je use case
- Let op de volgorde van waarden in multi-dimensionale arrays

### Parameter
- De key én value moeten beide in de referentietabel staan
- Wordt gebruikt voor gestandaardiseerde context
- Is verplicht voor bepaalde observatie-types

### Metadata
- Alleen de keys moeten in de referentietabel staan
- Values zijn vrij formaat
- Wordt gebruikt voor niet-gestandaardiseerde aanvullende informatie
- Beperk de omvang voor betere performance

<a id="woordenlijst"></a>
## Woordenlijst

| Term | Betekenis                            |
|------|--------------------------------------|
| OM&S | Observations, Measurements & Samples |
| FoI  | Feature of Interest (meetobject)     |
| GML  | Geography Markup Language            |
| UoM  | Unit of Measure (meeteenheid)        |

[🏠 Terug naar index](/dd-api-v3-docs/index.md)

## Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |


