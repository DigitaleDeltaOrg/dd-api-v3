<link rel="stylesheet" type="text/css" href="/custom.css">

# API Referentie Digitale Delta API V3

## Algemene Informatie
De Digitale Delta API V3 is een RESTful API die toegang biedt tot water-gerelateerde observaties en referentiedata. De API is beschikbaar onder de CC BY 4.0 licentie.

## Hoofdfunctionaliteiten

### 1. Observaties Ophalen
Via het endpoint `/v3/odata/observations` kunnen meetgegevens worden opgevraagd. Deze endpoint biedt:
- Paginering met maximaal 10.000 records per request
- Uitgebreide filtermogelijkheden
- Sortering van resultaten
- Mogelijkheid tot het expanderen van gerelateerde entiteiten

### 2. Referentiedata Ophalen
Via het endpoint `/v3/odata/references` kunnen referentiegegevens worden opgevraagd. 
Deze endpoint heeft vergelijkbare functionaliteit als de observations endpoint.

## Data Modellen

### Observaties
Een observatie bevat:
- Unieke identificatie
- Type meting
- Tijdstippen (resultaat, fenomeen, validiteit)
- Parameters (zoals chemische, biologische of fysieke metingen)
- Metadata
- Feature of Interest (meetlocatie)
- Meetresultaten

### Meetresultaten
Resultaten kunnen worden uitgedrukt als:
- Boolean waarden (Truth)
- Tellingen (Count)
- Metingen met eenheid (Measure)
- Vocabulaire termen (Vocab)
- Geografische data (Geography)
- Coverage data

### Parameters
Parameters bevatten informatie over:
- Het gemeten fenomeen
- Het watercompartiment
- De meetgrootheid
- De verantwoordelijke organisatie
- Eventuele limietwaarden

## Query Mogelijkheden
De API ondersteunt OData query parameters:
- `$top`: Maximum aantal resultaten
- `$skiptoken`: Paginering
- `$filter`: Filtering van resultaten
- `$orderby`: Sortering
- `$expand`: Uitklappen van gerelateerde data
- `$count`: Totaal aantal resultaten
- `$select`: Selectie van specifieke velden

## Geografische Ondersteuning
De API ondersteunt verschillende geografische datatypes:
- Punten
- Lijnen
- Polygonen
- Multi-geometrieën
- Geometriecollecties

## Foutafhandeling
De API gebruikt standaard OData foutmeldingen met:
- Foutcode
- Beschrijvend bericht
- Details over de fout
- Eventuele inner errors

## Versie Informatie
De API versie wordt meegegeven in de headers:
- API-Version
- API-Deprecation-Date
- API-End-of-Life-Date
- API-Next-Release
- API-Additional-Documentation

Deze API is specifiek ontworpen voor het delen van waterdata en biedt een rijke set aan functionaliteiten voor het opvragen en filteren van meetgegevens en referentiedata.





## Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |