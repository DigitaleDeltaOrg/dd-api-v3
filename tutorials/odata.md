<link rel="stylesheet" type="text/css" href="/custom.css">

# OData

## Inleiding

Dit artikel geldt voor ***alle*** scenario's voor het opvragen van data. 
Het beschrijft de werking van OData, geeft voorbeelden van zoekopdrachten en voorbeeld antwoorden.

Het legt uit wat ```$filter```, ```$select```, ```$top``` en ```$skiptoken``` doen, de standaard elementen die geïmplementeerd moeten worden in een DD API V3, om te voldoen aan de eisen.

## Endpoints (eindpunten)

Voor deze voorbeelden wordt de open data gebruikt die in de DD API V3 implementatie van EcoSys zit, die data vanuit AquaDesk ontsluit.

De URL is: 🔗[https://ddapi.aquadesk.nl/v3/odata](https://ddapi.aquadesk.nl/v3/odata).

Een deel van de data is dus vrij beschikbaar. Andere delen van de data zijn alleen via authenticatiemethoden te bereiken.

Voor het opvragen van data via de DD API V3, zijn er twee zogenaamde 'endpoints', die onafhankelijk van elkaar werken.
Het references 'endpoint' is bedoeld om te achterhalen wat het onderliggende systeem aan definities heeft: dat gaat om eenheden, grootheden, parameters, typeringen, meetlocaties, meetnetten, etc.

Deze informatie kan bijvoorbeeld gebruikt worden voor catalogi of om lijsten op te bouwen, zoals een lijst van meetobjecten (voor geo-viewers), of een lijst van biologische taxa.

De URL daarvan is /references, dus in het voorbeeld URL: 🔗[https://ddapi.aquadesk.nl/v3/odata/references](https://ddapi.aquadesk.nl/v3/odata/references).

Meetresultaten (observations) zijn bereikbaar op een andere plek: /observations. Dus in het voorbeeld URL: 🔗[https://ddapi.aquadesk.nl/v3/odata/observations](https://ddapi.aquadesk.nl/v3/odata/observations).
Meetresultaten zijn er in verschillende soorten:

- Measure, een combinatie van eenheid en meetwaarde
- Count, alleen een geheel getal
- Vocab, een combinatie van woordenboek-naam en woord
- Truth, alleen een true (waar) of false (onwaar) waarde
- Tijdreeksen, verwachtingen en ensembles (via CoverageJSON)

De insteek van DD API V3 is een interpretatie van OM&S. OM&S houdt rekening met veel invalshoeken, waarvoor uitwisseling tussen data uit de verschillende invalshoeken, moeilijk wordt.
DD API V3 houdt zich alleen bezig met Observations, terwijl de data uit Samples aan Observations is toegevoegd.

Hierdoor is het mogelijk om chemie (geen samples) te combineren met ecologie (wél samples).

Beide soorten metingen hebben in het DD API V3 scenario dus dezelfde invalshoek, waardoor de informatie uit meerdere systemen gemakkelijk gecombineerd kan worden.

Het opvragen van data gebeurd door middel van OData.

## OData

🔗[OData](https://www.odata.org) is een standaard ISO/W3C standaard voor het zoeken naar informatie.

De basis daarvoor is een technisch formaat, wat beschrijft welke data het bronsysteem aanbiedt.

Systemen welke dit formaat kennen, kunnen dat formaat gebruiken om data te kunnen verwerken.

Dit is één van de belangrijke aspecten van DD API V3: het kan het technische formaat vergelijken met de standaard van DD API V3, en kan controleren wat klopt en wat niet.

Hierdoor kan worden bepaald of er aan de standaard wordt voldaan.

OData heeft de mogelijkheid om data te filteren (```$filter```), te selecteren (```$select```) wat terug moet worden gegeven in de antwoorden en te rapporteren hoeveel elementen aan de zoekopdracht voldoen (```$count```). Daarnaast kan worden aangegeven hoeveel elementen moeten worden teruggegeven (```$top```).

Hoe ziet een aanroep eruit? OData zoekopdrachten worden via een URL uitgevoerd, zodat ze ook via webbrowsers kunnen worden overgestuurd.

Het ziet er dan zo uit:

`?$filter=...&$select=...&$count=...&$top=...`

`$filter`, `$select`, `$count` en `$top` worden fragmenten genoemd.

Geen van de fragmenten is verplicht. Wanneer ze er wél zijn, dan moet het eerste fragment na een ```?``` komen en de navolgende fragmenten na een ```&```. Dat is niet bepaald door OData, maar door het HTTP-protocol waarmee webbrowsers werken.

Met drie puntjes wordt aangegeven dat daar een waarde moet komen te staan wanneer het fragment is opgegeven.
Ieder fragment heeft een eigen 'taal'.


### $filter

Met ```$filter``` wordt aangegeven aan welke criteria de data moet voldoen.
Het is gebaseerd op het Engels en bestaan veelal uit drie onderdelen:

- Naam van het element waarop gefilterd moet worden
- Vergelijking
- Waarde waarmee vergeleken moet worden

Vergelijkingen zijn:

| Vergelijking | Betekenis                                 | 
|--------------|-------------------------------------------|
| `eq`         | equals: is gelijk aan, komt exact overeen |
| `ne`         | not equals, ongelijk aan                  |
| `gt`         | greater than, groter dan                  |
| `ge`         | greater/equal, groter dan of gelijk aan   |
| `lt`         | less than, kleiner dan                    |
| `le`         | less/equal, kleiner dan of gelijk aan     |
| `in`         | in list, moet staan in de opgegeven lijst |

Niet alle vergelijkingen zijn geldig voor alle data types.

Daarnaast zijn er nog functies in OData. De standaard functie (*die geïmplementeerd moeten worden!*) zijn:

| Functie              | Betekenis                                                                                               |
|----------------------|---------------------------------------------------------------------------------------------------------| 
| `startswith`         | tekst begint met                                                                                        |,
| `endswith`           | tekst eindigt met                                                                                       |,
| `contains`           | tekst bevat                                                                                             |,
| `trim`               | verwijder spaties aan begin en eind                                                                     |,
| `date`               | alleen volledige datum van een datum/tijd veld, dus tijd wordt verwijderd                               |,
| `time`               | alleen volledige tijd van een datum/tijd veld, dus datum wordt verwijderd                               |,
| `year`               | alleen jaartal van een datum/tijd veld                                                                  |,
| `month`              | alleen maandnummer van een datum/tijd veld                                                              |,
| `day`                | alleen dagnummer van een datum/tijd veld                                                                |,
| `hour`               | alleen uur van een datum/tijd veld                                                                      |,
| `minute`             | alleen minuut van een datum/tijd veld                                                                   |,
| `second`             | alleen seconde van een datum/tijd veld                                                                  |,
| `totaloffsetminutes` | alleen totaal aantal minuten van een datum/tijd veld                                                    |,
| `ceiling`            | naar boven afronden                                                                                     |,
| `floor`              | naar beneden afronden                                                                                   |,
| `round`              | afronden                                                                                                |,
| `tolower`            | tekst omzetten naar kleine letters                                                                      |,
| `toupper`            | tekst omzetten naar hoofdletters                                                                        |,
| `length`             | lengte van de tekst berekenen                                                                           |,
| ~~geo.distance~~     | ~~geografische afstand in meters~~                                                                      |,
| ~~geo.intersects~~   | ~~geografie bevindt zich in aangegeven [WKT (Well Known Text)](https://github.com/opengeospatial/wkt)~~ | 


De meeste frameworks implementeren dit al standaard. Het kan alleen zijn dat namen van de equivalenten in de database-taal anders zijn en geconverteerd moeten worden in de databaselaag.

geo.intersects en geo.distance zijn verwijderd als functies die standaard ondersteund moeten worden. Dit heeft een aantal redenen:

- Deze methoden ondersteunden alleen specifieke geografische vormen, dus niet de generieke 'geography'. Omdat deze vormen een specialisatie van geography waren (zoals GeographyPoint of GeographyPolygon), waren ze in praktijk niet bruikbaar, omdat het vaak gaat om een mix van geografische specialisaties.
- Deze methoden waren nogal gebonden aan SQL Server
- De syntax is onhandig

Daarom zijn deze functies vervangen door vereenvoudigde custom OData functies:

| Functie               | Betekenis                                                               |
|-----------------------|-------------------------------------------------------------------------| 
| `intersects(wkt=...)` | kijkt of de opgegeven wkt overlapt met de geografie van de FoI          |
| `distance(wkt=...)`   | berekend de afstand tussen opgegeven wkt en met de geografie van de FoI |

In de tabel hierboven wordt parameter `wkt` vereist. 
WKT staat voor Well Known Text en is een leesbare vorm voor het opgeven van een geometrie of geografie. 
Een praktisch document staat [hier](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometr](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry).

Er hoeft niet aan te worden gegeven welk OData property voor deze functies gebruikt hoeft te worden, omdat er altijd maximaal één geo-object in een entiteit staat (waarop gezocht kan worden).
Ook is de syntax van 'SRID=...;Polygon'POINT(..)'' niet meer nodig, minder aanhalingstekens, geen geo-type meer, geen SRID meer.
Het is eenvoudiger in gebruik, wat implementatie gemakkelijker maakt.


### Data types

OData heeft de volgende data types:

| Data type  | Beschrijving                                                                                                                                                                                                                                                                            | 
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tekst      | Tussen aanhalingstekens                                                                                                                                                                                                                                                                 | 
| Numeriek   | Amerikaans formaat. Geen duizendtallen. Decimalen gescheiden door punt                                                                                                                                                                                                                  |
| Datum/tijd | Amerikaans formaat. Let op: géén aanhalingstekens!                                                                                                                                                                                                                                      |

### $select

Met $select kan worden aangegeven welke eigenschappen terug moeten komen in de responses (antwoorden van de zoekopdracht).

### $count

Met ```$count``` kan worden opgevraagd hoeveel observaties/referenties voldoen aan de zoekopdracht.

Het aantal wordt als extra onderdeel van het antwoord teruggegeven en alleen wanneer de waarde op 'true' staat.
Dus ```$count=true```.

### $top

Met ```$top``` worden alleen het opgegeven aantal resultaten teruggegeven.

Het aantal wordt als extra onderdeel van het antwoord teruggegeven en alleen wanneer de waarde op 'true' staat.
Dus ```$count=true```.

De volgende voorbeelden zijn gebaseerd op de open data in AquaDesk. AquaDesk heeft mogelijk data ter beschikking die andere systemen niet (nodig) hebben, zoals de Taxa Waterbeheer Nederland-lijst.

### $skip

Het ```$skip```-token  wordt *niet* ondersteund. Deze wordt vervangen door ```$skiptoken```

### $skiptoken

Het ```$skiptoken```-token wordt gebruikt om de volgende pagina informatie op te halen. 

> _Probeer deze niet zelf te bedenken: deze wordt door de service zelf gegenereerd en meegegeven int de response._

Het proberen een eigen bedachte ```$skiptoken``` op te geven, resulteert in het teruggeven van de eerste pagina data.
