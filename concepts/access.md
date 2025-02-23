<link rel="stylesheet" type="text/css" href="/custom.css">

# Authenticatie voor Digitale Delta API V3

De Digitale Delta API V3 specificatie ondersteunt verschillende manieren van authenticatie. 
Welke methode gebruikt wordt, is een keuze van de aanbieder. De noodzakelijke informatie, zoals token endpoint, verschilt ook per aanbieder.
Dit overzicht laat zien hoe je kunt authenticeren bij elk van deze methodes, zonder in technische details te verzanden.


## API Key Authenticatie
1. Ontvang een API key van de dienstaanbieder
2. Voeg bij elk request deze header toe:
```
X-API-KEY: jouw-api-key
```

## OAuth2 Authenticatie
1. Ontvang client credentials van de dienstaanbieder:
    - client_id
    - client_secret
2. Vraag een access token op bij het token endpoint
3. Gebruik het token in elke request:
```
Authorization: Bearer jouw-access-token
```

## mTLS (Mutual TLS)
1. Ontvang een client certificaat (.pfx/.p12 bestand) en wachtwoord
2. Configureer je client/applicatie met:
    - Client certificaat
    - Wachtwoord
    - Server certificaat (indien nodig)
3. De authenticatie verloopt automatisch via TLS handshake

## Geen Authenticatie
- Direct toegankelijk via HTTPS
- Geen extra headers nodig

> **Tip**: Test eerst met een eenvoudige HTTP client zoals Postman of cURL voordat je gaat implementeren.



## Geverifiëerde bronnen
> De DD-API endpoints worden via verschillende platforms momenteel al ontsloten:
> - 🔗[AquaDesk](https://live.aquadesk.nl) (beheerd door 🔗[EcoSys](https://ecosys.nl), toegang via 🔗[Aquon](https://aquon.nl) voor aangesloten waterschappen)
> - Direct bij de bronhouder (zoals 🔗[Rijkswaterstaat](https://rws.nl))
> - 🔗[Informatiehuis Water (IHW)](https://ihw.nl) platform

### Directe bronhouders

| Eigenaar                                       | Soort data                                                                                                           | DD API V3 versie | Authenticatiemethode | Aanvragen via | API basis URL                        |
|------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|------------------|----------------------|---------------|--------------------------------------|
| 🔗[Rijkswaterstaat](https://rws.nl)            | Publieke vogel/zoogdier-data van SOVON, vis-data van Wageningen Marine Research, Klein-biologische data uit AquaDesk | V3.0             | Geen                 |               | 🔗https://biodata-rws.aquadesk.nl/v3 |
| 🔗[Informatiehuis Water (IHW)](https://ihw.nl) | Waterkwaliteitsportaal: ecologisch, fysisch/chemisch                                                                 | V3.0             | X-API-KEY            | [In aanvraag] | [In aanvraag]                        |
| 🔗[Aquon](https://aquon.nl)                    | Data van het 🔗[Aquon](https://aquon.nl) sensor platform : fysisch/chemisch (observaties-based)                      | V3.0             | [In aanvraag]        | [In aanvraag] | 🔗https://api.aquon.nl/v3            |

### Via [AquaDesk](https://live.aquadesk.nl) platform (beheerd door [EcoSys](https://ecosys.nl)). Bevat ook niet-publieke data.

| Eigenaar                                                        | Soort data                                                                       | DD API V3 versie | Authenticatiemethode | Aanvragen via                       | API basis URL                  |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|------------------|----------------------|-------------------------------------|--------------------------------|
| Historische data Noord-Holland                                  | Ecologische data uit [AquaDesk](https://live.aquadesk.nl)                        | V3.0             | X-API-KEY            | geen aanvraag nodig, alleen API key | https://ddapi.aquadesk.nl/v3   |
| 🔗[Rijkswaterstaat](https://rws.nl)                             | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | aquadesk@rws.nl                     | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Hoogheemraadschap Hollands Noorderkwartier](https://hhnk.nl) | Ecologische en chemisch/fysische data uit 🔗[AquaDesk](https://live.aquadesk.nl) | V3.0             | X-API-KEY            | aquadesk@hhnk.nl                    | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Hoogheemraadschap De Stichtse Rijnlanden](https://hdsr.nl)   | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Hoogheemraadschap van Rijnland](https://rijnland.net)        | Ecologische en chemisch/fysische data uit 🔗[AquaDesk](https://live.aquadesk.nl) | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Waterschap De Dommel](https://dommel.nl)                     | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Waterschap Brabantse Delta](https://brabantsedelta.nl)       | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | aquadesk@hhdl.nl                    | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Hoogheemraadschap van Delfland](https://hhdelfland.nl)       | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Hoogheemraadschap van Schieland en ](https://hhsk.nl)        | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Waterschap Rivierenland](https://wsrl.nl)                    | Ecologische data uit 🔗[AquaDesk](https://live.aquadesk.nl)                      | V3.0             | X-API-KEY            | [In aanvraag]                       | 🔗https://ddapi.aquadesk.nl/v3 |
| 🔗[Waterschap Hollandse Delta](https://wshd.nl)                 | Ecologische en chemisch/fysische data uit 🔗[AquaDesk](https://live.aquadesk.nl) | V3.0             | X-API-KEY            | [In aanvraag]                       | [In aanvraag]                  |
| 🔗[Waterschap Scheldestromen](https://scheldestromen.nl)        | Ecologische en chemisch/fysische data uit 🔗[AquaDesk](https://live.aquadesk.nl) | V3.0             | X-API-KEY            | [In aanvraag]                       | [In aanvraag]                  |

### Gepland

| Eigenaar                            | Soort Data                        | DD API V3 versie |
|-------------------------------------|-----------------------------------|------------------|
| 🔗[Rijkswaterstaat](https://rws.nl) | WADAR (chemisch)                  | DD API V3 versie |
| 🔗[Rijkswaterstaat](https://rws.nl) | Landelijk Meetnet Water (fysisch) | DD API V3 versie |
| Nederlandse waterbeheerders         | Slim Watermanagement (fysisch)    | DD API V3 versie |

## Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |

