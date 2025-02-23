<link rel="stylesheet" type="text/css" href="/custom.css">

# Bekende implementaties

Dit zijn de bij ons bekende implementaties ban DD API V3.

Staat er een implementatie niet bij en wil je dat deze vermeldt wordt? 

Neem dan contact op met de [service van Informatiehuis Water](mailto:servicedesk@ihw.nl).

We gaan dan bekijken of de service voldoet aan de specificaties van DD API V3.


## Geverifiëerde bronnen
> De DD-API endpoints worden via verschillende platforms momenteel al ontsloten:
> - 🔗[AquaDesk](https://live.aquadesk.nl) (beheerd door 🔗[EcoSys](https://ecosys.nl), toegang via 🔗[Aquon](https://aquon.nl) voor aangesloten waterschappen)
> - Direct bij de bronhouder (zoals 🔗[Rijkswaterstaat](https://rws.nl))
> - 🔗[Informatiehuis Water (IHW)](https://ihw.nl) platform

### Directe bronhouders

| Eigenaar                                       | Soort data                                                                                                                         | DD API V3 versie | Authenticatiemethode | Aanvragen via | API basis URL                                                              |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|------------------|----------------------|---------------|----------------------------------------------------------------------------|
| 🔗[Rijkswaterstaat](https://rws.nl)            | - Publieke vogel/zoogdier-data van SOVON <br/>- Vis-data van Wageningen Marine Research <br/>- Klein-biologische data uit AquaDesk | V3.0             | Geen                 |               | 🔗[https://biodata-rws.aquadesk.nl/v3](https://biodata-rws.aquadesk.nl/v3) |
| 🔗[Informatiehuis Water (IHW)](https://ihw.nl) | Waterkwaliteitsportaal: <br/>- ecologisch<br/>- Fysisch/chemisch                                                                   | V3.0             | X-API-KEY            | [In aanvraag] | [In aanvraag]                                                              |
| 🔗[Aquon](https://aquon.nl)                    | Data van het 🔗[Aquon](https://aquon.nl) sensor platform: <br/>- Fysisch/chemisch                                                  | V3.0             | [In aanvraag]        | [In aanvraag] | 🔗[https://api.aquon.nl/v3](https://api.aquon.nl/v3)                       |

### Via [AquaDesk](https://live.aquadesk.nl) platform (beheerd door [EcoSys](https://ecosys.nl)). Bevat ook niet-publieke data.
Voor de data van AquaDesk is een API key noodzakelijk. Je kunt deze gratis aanmaken via de [AquaDesk portal](https://live.aquadesk.nl).

Standaard worden rechten voor de Historische data Noord-Holland toegekend.

De organisaties in onderstaande tabel kunnen rechten toevoegen aan je API key. Neem hiervoor contact op met de betreffende organisatie.


| Eigenaar                                                        | Soort data                          | DD API V3 versie | Authenticatiemethode | Aanvragen via    | API basis URL                                                   |
|-----------------------------------------------------------------|-------------------------------------|------------------|----------------------|------------------|-----------------------------------------------------------------|
| Historische data Noord-Holland                                  | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | -                | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Rijkswaterstaat](https://rws.nl)                             | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | aquadesk@rws.nl  | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Hoogheemraadschap Hollands Noorderkwartier](https://hhnk.nl) | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | aquadesk@hhnk.nl | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Hoogheemraadschap De Stichtse Rijnlanden](https://hdsr.nl)   | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Hoogheemraadschap van Rijnland](https://rijnland.net)        | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Waterschap De Dommel](https://dommel.nl)                     | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Waterschap Brabantse Delta](https://brabantsedelta.nl)       | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | aquadesk@hhdl.nl | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Hoogheemraadschap van Delfland](https://hhdelfland.nl)       | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Hoogheemraadschap van Schieland en ](https://hhsk.nl)        | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Waterschap Rivierenland](https://wsrl.nl)                    | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Waterschap Hollandse Delta](https://wshd.nl)                 | - Ecologisch<br/>- Fysisch/chemisch | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |
| 🔗[Waterschap Scheldestromen](https://scheldestromen.nl)        | - Ecologisch<br/>- fysisch/Fysisch  | V3.0             | X-API-KEY            | [In aanvraag]    | 🔗[https://ddapi.aquadesk.nl/v3]([https://ddapi.aquadesk.nl/v3) |

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

