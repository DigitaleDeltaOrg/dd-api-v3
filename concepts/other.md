<link rel="stylesheet" type="text/css" href="/custom.css">

# Diversen

Dit document belicht een aantal scenario's voor diverse onderwerpen.

## Null-onderdrukking

Alle responses van DD API zijn in JSON formaat. 

Een onhebbelijkheid is dat er veel eigenschappen kunnen zijn zonder waarde. 

Kent het systeem veel referenties in de parameter en metadata blokken, dan kunnen dit er veel zijn.

Dit worden als ```null``` weergegeven.

Door bij het request een bepaalde header met waarde mee te geven, worden velden met een null-waarde niet teruggegeven.

De header heet`prefer` en moet als waarde `omit-values=nulls` hebben.

## Coördinaten

Coördinaten van meetobjecten en observaties worden standaard uitgedrukt in stelsel ETRS89 met als eenheid graden.
Er kan ook altijd slechts één coördinatenstelsel per uitvoer worden gebruikt.

Dat laatste kunnen we niet voorkomen, omdat de GeoJSON standaard aangeeft dat de Crs (Coordinate Reference System) niet meer onderdeel is van de specificatie.

Daarom volgt DD API V3 de aanbeveling van GeoNovum: via een header kan worden aangegeven welk coördinatenstelsel gebruikt kan worden.

De aanbieder moet _proberen_ daaraan te voldoen.

De header heet `Accept-Crs` en de waarde moet een geldige EPSG waarde hebben in de vorm `ESPG:<srid>`.

In de response komt dan een header te staan genaamd `Content-Crs` met een waarde die aangeeft wat het coördinatenstelsel geworden is.

De EPSG definities zijn op de 🔗[EPSG-site](https://epsg.io) te vinden.

We raden aan om ten minste de volgende stelsels te implementeren:

| EPSG  | Omschrijving                                                             |
|-------|--------------------------------------------------------------------------|
| 28992 | Rijksdriehoeksstelsel (RD New) - Standaard voor Nederlands waterbeheer   |
| 4258  | ETRS89 - Voor Europese waterprojecten en grensoverschrijdend waterbeheer |
| 4326  | WGS 84 - 2D geografisch (GPS)                                            |
| 25831 | ETRS89 / UTM zone 31N (West-NL)                                          |
| 25832 | ETRS89 / UTM zone 32N (Oost-NL)                                          |
