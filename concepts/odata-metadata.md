<link rel="stylesheet" type="text/css" href="/custom.css">

# OData Metadata

De metadata van OData ($metadata) is een bestand dat definieert welke entiteiten en eigenschappen een OData service beschikbaar stelt.

Waar OData metadata meestal het opslagmodel van een service reflecteert, hanteren we in DD API een andere aanpak: wij definiëren de metadata vooraf.

Dit stelt de aanroepende service in staat om precies te weten welke entiteiten beschikbaar zijn in het aanbiedende systeem, wat de consistentie waarborgt.

## Eigenschappen

Binnen de entiteiten zijn slechts enkele eigenschappen verplicht. 
Let wel: alle eigenschappen die je gebruikt, moeten voorkomen in de officiële lijsten die hier op GitHub zijn gepubliceerd.

### Voorbeeld
Veel eigenschappen zijn biologisch van aard, zoals:
- Geslacht
- Levensstadium
- Lengte
- Gewicht

Ook als je systeem alleen chemisch/fysische metingen bevat (zoals pH, temperatuur, debiet, waterhoogte of zuurstofgehalte), gebruik je deze standaard eigenschappen. 
Dit zorgt ervoor dat alle systemen dezelfde 'taal' spreken.

Mis je een eigenschap? Neem dan contact op. We onderzoeken dan of:
- de eigenschap onder een bestaande definitie valt, of
- er een nieuwe eigenschap moet worden aangemaakt

> **Belangrijk**: Maak nooit zelf nieuwe eigenschappen aan!

Een overzicht van de (momenteel) valide referentietypes staan [hier](reference-types.md). 
Een CSV versie is [hier](v3/docs/specifications/domainlist.csv) te vinden.

# Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |


TODO: toevoegen forecastTime aan Metadata


ensemble filter

/observations?
$filter=
meta/ForecastTime eq 2024-03-20T12:00:00Z and
foi/code eq 'NLRWSOS_HVH' and
parameter/Parameter eq 'waterLevel' and
ValidTime ge 2024-03-20T12:00:00Z and
ValidTime le 2024-03-21T12:00:00Z
&$expand=Members($select=MemberId,Value,Quality;$orderby=MemberId)
&$select=ValidTime,Parameter,Location
&$orderby=ValidTime
)
