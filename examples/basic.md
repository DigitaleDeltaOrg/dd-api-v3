# Basis Voorbeelden

## Je eerste API aanroep
```csharp
// Basis URL met een eenvoudige query voor waterhoogte observaties
GET https://api.dd.nld/v3/observations?$filter=type eq 'waterHeight'
```

## Filteren van resultaten
```csharp
// Filteren op locatie en tijd
GET https://api.dd.nld/v3/observations?$filter=location eq 'HOEK-V-HLD' and timestamp gt 2024-03-01T00:00:00Z
```

## Selecteren van specifieke velden
```csharp
// Alleen relevante velden ophalen
GET https://api.dd.nld/v3/observations?$select=timestamp,value,location
```

## Paginering
```csharp
// Eerste pagina met 10 resultaten
GET https://api.dd.nld/v3/observations?$top=10

// Volgende pagina gebruik makend van de skiptoken uit de vorige response
GET https://api.dd.nld/v3/observations?$top=10&$skiptoken=dlG5jZXNzZnVsbHkgZGVjb2RlZCB0aGlzIHN0cmluZyw
```

De `$skiptoken` waarde wordt altijd door de server meegegeven in de response als er meer resultaten beschikbaar zijn. 
Dit is veiliger en efficiënter dan `$skip` omdat:
1. De server controle heeft over de paging
2. Het consistente resultaten garandeert, zelfs als er tussentijds data wordt toegevoegd
3. Het performanter is bij grote datasets

# Excel Integratie

Excel gebruikers kunnen op verschillende manieren met de DD API werken:

## Power Query (aanbevolen)
Power Query is de meest krachtige manier om data uit de DD API in Excel te krijgen:
- Ondersteunt OData native
- Kan omgaan met paginering
- Verversbaar
- Data transformatie mogelijkheden
- Filtermogelijkheden direct in de query

## Direct URLs
Voor simpele queries kan een directe URL in Excel gebruikt worden:
```excel
=WEBSERVICE("https://api.dd.nld/v3/observations?$filter=location eq 'HOEK-V-HLD'&$top=100")
```

## Voorbeeldbestanden
We bieden voorbeeldbestanden aan met:
- Voorgeconfigureerde Power Query connecties
- Voorbeeld dashboards
- Uitleg over verversing van data
- Tips voor grote datasets

## Aandachtspunten
- Beperk het aantal rijen per request (`$top`)
- Gebruik filters (`$filter`) om alleen relevante data op te halen
- Let op de update-frequentie (niet elke seconde verversen)
- Overweeg het gebruik van caching voor veelgebruikte queries

# Business Intelligence Tools

## Tableau
### Directe OData Connectie
```sql
// Connector type: Web Data Connector (WDC) of Generic OData
Server: https://api.dd.nld/v3
Table: observations
```

### Voorbeeld Custom SQL
```sql
// Gebruik een custom query voor specifieke data
SELECT 
    timestamp,
    location,
    value,
    type
WHERE 
    type = 'waterHeight' 
    AND timestamp >= dateadd(hour,-24,now())
```

## Power BI
### Native OData Connectie
```powerquery
let
    Source = OData.Feed("https://api.dd.nld/v3/observations"),
    Filtered = Table.SelectRows(Source, each [type] = "waterHeight"),
    SelectedColumns = Table.SelectColumns(Filtered, {"timestamp", "location", "value"})
in
    SelectedColumns
```

### Verversings-instellingen
- Scheduled refresh voor Premium workspaces
- Incrementele verversing mogelijk op timestamp
- Gateway vereisten voor on-premise oplossingen

# QGIS Integratie

## Meetlocaties uit Referenties
### Via References endpoint
```plaintext
URL: https://api.dd.nld/v3/references/locations
Parameters:
- $select=id,name,coordinates,type
```
Dit geeft direct alle beschikbare meetlocaties met hun eigenschappen, zonder filter nodig.

## Actuele Waarden
```plaintext
URL: https://api.dd.nld/v3/latest
Parameters:
- $select=location,value,timestamp
- $expand=location
```

## Praktische Workflow
1. Laad eerst de referentie meetlocaties (statische laag)
2. Gebruik latest endpoint voor actuele waardes
3. Join op basis van location identifier
4. Ververs alleen de latest data periodiek

## Tips
- References data hoeft maar 1x geladen te worden
- Gebruik references/parameters voor beschikbare meettypes
- Latest endpoint is veel sneller dan observations voor actuele situatie


# ArcGIS Integratie

## Basisopzet
### Feature Layer via References
```plaintext
URL: https://api.dd.nld/v3/references/locations
- Projectie: RD New (EPSG:28992)
- Format: GeoJSON/Feature Layer
```

## Data Verversing
### Via GeoProcessing Tool
- Gebruik 'URL to JSON' tool
- Schedule verversing voor latest endpoint
```plaintext
URL: https://api.dd.nld/v3/latest
```

## Symbologie
- Graduated symbols op basis van waarde
- Verschillende symbolen per parametertype
- Dynamic label classes voor actuele waardes

## Tips
- Gebruik de references endpoints voor stabiele basislagen
- Cache alleen de statische data (references)
- Latest endpoint voor real-time updates
- Join & Relates op location identifier
