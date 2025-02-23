<link rel="stylesheet" type="text/css" href="/custom.css">

# Headers

De volgende headers hebben betekenis in DD API V3

## Request-headers

### X-API-KEY

`X-API-KEY` wordt gebruikt om een API key door te geven voor authenticatie, wanneer het aangeroepen systeem die authenticatiemethode toestaat.

## Null-onderdrukking

De header `prefer` met waarde `omit-values=nulls` zorgt ervoor dat eigenschappen met een null waarde worden overgeslagen in de response.

### Geografie

`Accept-Crs` wordt gebruikt om aan te geven in welk coördinatenstelsel de client de GeoJSON antwoorden wil hebben, 
door als value een geldige EPSG mee te geven in notatie EPSG:<numerieke-epsg>.
Wanneer het aangeroepen systeem de waarde accepteert, dan zal deze dat met een `Content-Crs` header bevestigen.

Zonder `Accept-Crs` zal de geografie in `EPSG:4258` (ETRS89, in booggraden) worden teruggegeven.

## Response-headers

### Geografie

`Content-Crs` met de EPSG van de response wordt teruggegeven wanneer de aanvrager een `Accept-Crs` heeft gevraagd.

### Rate limiting

Wanneer rate limiting is geïmplementeerd, dan moeten de volgende headers worden teruggestuurd in de response, indien van toepassing.
We volgen hiermee de GitHub specificatie.

`X-RateLimit-Limit`: Maximum aantal requests toegestaan
`X-RateLimit-Remaining`: Aantal requests over in huidige tijdsvenster  
`X-RateLimit-Reset`: Tijdstip wanneer limit wordt gereset (Unix timestamp)
`X-RateLimit-Used`: Aantal requests gebruikt in huidige tijdsvenster
`X-RateLimit-Resource` Resource waar de limit voor geldt
`Retry-After`: Aantal seconden wachten bij overschrijding (alleen bij 403: forbidden)
