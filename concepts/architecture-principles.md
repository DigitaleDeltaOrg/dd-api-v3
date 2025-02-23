<link rel="stylesheet" type="text/css" href="/custom.css">

# DD API V3 Architectuurprincipes

**Fundamentele uitgangspunten**
- [Pragmatische aanpak](#pragmatische-aanpak)
- [Scope](#scope)
- [Focus op uitwisselbaarheid](#focus-op-uitwisselbaarheid)
- [Twee-deling (referenties/observations)](#tweedeling)

**Architectuurkeuzes**
- [Keuze voor OData (en waarom)](#waarom-odata)
- [OM&S als basis (en waarom)](#oms-als-basis)
- [Vereenvoudigingen t.o.v. standaarden](#vereenvoudiging-tov-oms)
- [API-strategie alignment](#api-strategie)

**Standaardisatie**
- [Gemeenschappelijke $metadata definitie](#gemeenschappelijke-metadata-definitie)
- [OpenAPI Specification (OAS)](#openapi-specification-oas)
- [Subset van OData functionaliteit](#subset-van-odata-functionaliteit)
- [Vastgelegde vocabulaires](#vocabulaires)

**Architecturale impact**
- [Flexibiliteit voor implementaties]()
- [Schaalbaarheid]()
- [Interoperabiliteit]()
- [Toekomstbestendigheid]()

**Governance**
- [Beheer van gemeenschappelijke definities]()
- [Versiebeheer van specificaties]()
- [Updateproces voor metadata]()

<a id="pragmatische-aanpak"></a>
## Pragmatische aanpak
De DD API V3 is bewust ontwikkeld vanuit een **pragmatische aanpak**. 

Dit betekent dat we kiezen voor praktische oplossingen die direct waarde toevoegen voor onze gebruikers, in plaats van te verzanden in theoretische perfectie.

### Waarom pragmatisch?
- **Direct bruikbaar**: Functionaliteit die nú werkt is waardevoller dan perfecte functionaliteit in de toekomst
- **Iteratieve verbetering**: We bouwen voort op praktijkervaringen van gebruikers
- **Feedback gedreven**: Aanpassingen komen voort uit echte gebruikssituaties
- **Snelle time-to-market**: Nieuwe features kunnen snel beschikbaar komen
- **Flexibiliteit**: Makkelijker aanpasbaar aan veranderende behoeften

Dit betekent **niet** dat we concessies doen aan kwaliteit - we zorgen voor een robuuste basis die kan meegroeien met toekomstige wensen.

### Pragmatische API-specificatie vs theoretisch model

Een theoretische benadering van API-specificaties leidt vaak tot:
- 🔍 Overmatige focus op complete domeinmodellering
- 📚 Complexe datastructuren die alle mogelijke use-cases dekken
- 🕸️ Ingewikkelde relaties tussen resources
- ⏳ Lange doorlooptijd voordat implementaties kunnen starten

De pragmatische aanpak van DD API V3:
- 🎯 Focus op veelgebruikte scenario's en data-elementen
- 🔄 Iteratieve uitbreiding van functionaliteit
- 👥 Specificaties gebaseerd op concrete implementatie-ervaringen
- 🛠️ Ruimte voor verschillende implementatiestrategieën
- 📋 Heldere, implementeerbare contracten

Deze aanpak zorgt ervoor dat organisaties snel aan de slag kunnen met het implementeren van DD API V3, terwijl de specificatie meegroeit met de praktijkbehoeften.

<a id="scope"></a>
## Scope

De DD API specificatie definieert het gedrag van de API, zodat deze consistent werkt over alle implementaties hiervan.
Echter is er veel vrijheid in platforms, omgevingen, ontwikkeltalen, etc.

### ✅ WEL Gespecificeerd
- Contract tussen systemen
    - API endpoints gedrag
    - Query syntax & filters
    - Response formaten
    - Datastructuren voor communicatie
    - Metadata ondersteuning

### ❌ NIET Gespecificeerd
- Interne implementatie details
    - Opslagmethoden
    - Database keuze
    - Architectuur
    - Technische stack


<a id="focus-op-uitwisselbaarheid"></a>
## Focus op uitwisselbaarheid

De DD API V3 is ontworpen met uitwisselbaarheid als kerndoel. 
Dit wordt bereikt door een zorgvuldig gekozen subset van OM&S te implementeren die universeel toepasbaar is.

### Universele toepasbaarheid
- **Gemeenschappelijke basis**: Alle observation-types delen dezelfde basisstructuur
- **Generieke benadering**: Één consistente API-interface voor verschillende soorten waarnemingen
- **Maximale compatibiliteit**: Werkt voor alle types observaties zonder specifieke uitzonderingen

### Bewuste vereenvoudiging
- **Geen Samples niveau**: Dit niveau is bewust weggelaten omdat niet alle observation-types hiermee werken
- **Behoud van data**: Alle samples-gerelateerde informatie blijft beschikbaar via:
    - `Parameter` entiteiten
    - `Metadata` entiteiten

### Voordelen
- 🔄 Eenvoudiger integratie tussen verschillende systemen
- 📊 Consistente data-uitwisseling over alle observation-types
- 🛠️ Lagere implementatiedrempel
- 🔍 Betere voorspelbaarheid van API-gedrag
- 📈 Verhoogde herbruikbaarheid van code en componenten

Deze focus op uitwisselbaarheid zorgt voor een robuuste basis waarop verschillende implementaties kunnen voortbouwen, zonder concessies te doen aan de essentiële functionaliteit.

<a id="waarom-odata"></a>
## Waarom OData?

Filteren van data via een REST API is niet altijd gemakkelijk.

Voor hydrologische metingen is dit nog redelijk eenvoudig. Dat zou via 'vaste paden' kunnen, zoals gebruikt was in DD API V1 en V2.

Efficient filteren voor ecologische metingen is een heel ander verhaal: er zijn veel meer 'parameters' waarop geselecteerd kan worden.

Hiervoor zou een handje vol eindpunten niet genoeg zijn.
We hebben het dan over letterlijk duizenden en je moet daaruit precies de juiste selecteren waarop je wilt zoeken.
Het gecombineerd zoeken van hydrologische en ecologische metingen zou resulteren in het moeten doen van veel verschillende aanroepen van de service.

Daarom is er gekozen voor 🔗[OData](https://www.odata.org/blog/OData-Published-as-an-ISO-Standard/) als filtermechanisme.
Ook een sterk punt van OData is dat het 'weet' hoe de datastructuur eruit ziet, dankzij de metadata definitie van OData.
Veel tools, zoals GIS, AI en BI systemen, maar ook Excel, vragen die informatie op wanneer aangegeven is dat het om een OData-service gaat,
zodat ze de consument al kunnen leiden door het proces van het samenstellen van de filteropdrachten.

> Implementeren van OData is niet altijd eenvoudig. Echter hebben de grote frameworks programmabibliotheken die ermee om kunnen gaan.
Andere systemen kunnen gebruik maken van het feit dat de OData-taal een formele definitie heeft in de vorm van een EBNF schema.
Zo ongeveer iedere (redelijk moderne) programmeertaal heeft zogenaamde 'parsers' om met EBNF schema's om te gaan.

Een veel gehoorde klacht is dat filteren in OData moeilijk in te brengen is in bestaande systemen.
Dat is echter niet inherent aan het gebruik van OData, maar meer aan

DD API V3 specificeert een subset van OData, en de te gebruiken definitie in metadata formaat daarvan, wordt ook door DD API V3 gespecificeerd en regelmatig bijgewerkt, indien nodig.
Doordat de OData clients deze interpreteren, kunnen de clients precies achterhalen wat het systeem kent en kan.

De OData definitie in DD API V3 schrijft het gebruik van `$skiptoken` voor in plaats van `$skip`. De eerste is moderner en geeft betere performance.

Daarnaast worden de standaard geo-functies *niet* gebruikt, maar worden een tweetal 'custom' functies gebruikt die veel eenvoudiger in gebruik zijn en niet omgevinggebonden zijn.

<a id="oms-als-basis"></a>
## OM&S als basis

De keuze voor Observations & Measurements Standard (OM&S) als fundament voor de DD API V3 is specifiek afgestemd op de behoeften in het waterdomein.

### Waarom OM&S?
- 🌊 **Domein-specifiek**: Speciaal ontwikkeld voor milieu- en waterobservaties
- 📏 **Bewezen standaard**: ISO/OGC-standaard met brede acceptatie in het werkveld
- 🔬 **Complete dekking**: Voorziet in alle aspecten van waarnemingen in het waterdomein:
    - Meetgegevens en observaties
    - Meetprocessen en methodieken
    - Contextuele metadata
    - Kwaliteitsparameters

### Aansluiting bij waterbeheer
- **Hydrologische metingen**: Perfect voor tijdreeksen van waterstanden en debieten
- **Ecologische monitoring**: Ondersteunt complexe biologische waarnemingen
- **Waterkwaliteit**: Faciliteert chemische en fysische parameters
- **Klimaatdata**: Geschikt voor meteorologische waarnemingen
- **Verschillende tijdschalen**: Van real-time metingen tot historische reeksen

### Strategische voordelen
- 🌐 **Internationale aansluiting**:
    - Naadloze integratie met mondiale waterstandaarden
    - Deel van het OGC-ecosysteem voor geo-informatie
- ⚙️ **Toekomstbestendig**:
    - Actieve doorontwikkeling
    - Breed gedragen standaard
    - Flexibel voor nieuwe use cases

Deze standaard vormt een solide basis voor de DD API V3, waarbij we pragmatisch omgaan met de implementatie om maximale bruikbaarheid te garanderen.

<a id="tweedeling"></a>
## Tweedeling

Centraal in DD API V3 staan twee componenten: 
- referenties, de woordenboeken (begrippen)
- observations voor meetdata, gebaseerd op OM&S

Referenties zijn *geen* onderdeel van OM&S. OM&S laat volledig vrij hoe de begrippen worden gedefinieerd.

In de voorgaande Digitale Delta API's (DD API V1 & V2 en DD ECO API) had ieder soort attribuut een eigen endpoint.
Voor DD API V1 en V2 was dit redelijk eenvoudig, want er waren slechts weinig attributen.
DD ECO API had er *veel* meer.

Omdat sowieso er een aantal begrippen niet binnen alle systemen gelijk waren (bijvoorbeeld werden grootheden en parameters door elkaar gehaald)
was data niet altijd uitwisselbaar. 

In DD API V3 zitten alle attributen in dezelfde endpoint.

De rest van dit document gaat verder over observations.

<a id="vereenvoudiging-tov-oms"></a>
### Vereenvoudiging t.o.v. OM&S

- Alleen dictionary-structuren voor parameters en metadata
- Geen vrije uitbreidingen op schema-niveau
- Vooraf gedefinieerde keys voor bekende use-cases
- Feature of Interest is altijd meetlocatie
- Zo min mogelijk eigen uitbreidingen

De scope van DD API V3 is breed: observaties, tijdreeksen, verwachtingen en ensembles.
Om willen van de eenvoud en de globale uitwisselbaarheid, stellen we observaties in het midden. 

Immers alle bovenstaande hebben observations als gemeenschappelijk element.

**Samples** is _een perspectief_ op OM&S die we in deze fase niet _expliciet_ implementeren. 
Echter, alle informatie met betrekking tot Samples *blijft* behouden in `parameter` en `metadata`.

Eventueel kan in een nieuwere versie een meer Samples-gericht perspectief (uitwisseldoel) worden gemaakt, 
maar dat gaat mogelijk niet samen met tijdreeksen, verwachtingen en ensembles.

<a id="api-strategie"></a>
## API Strategie

1. **Conformiteit met Kennisplatform API's**
    - Volgt Nederlandse API Strategie
    - REST-principes als basis
    - JSON als standaard formaat
    - API-first benadering

2. **REST API Design Rules**
    - Gebruik van correcte HTTP-methodes
    - Duidelijke resource-naamgeving
    - Consistente URL-structuur
    - Standaard foutafhandeling (RFC 7807)

3. **OData-implementatie binnen API-strategie**
    - Aanvullend op basis REST-principes
    - Gestandaardiseerde query-mogelijkheden
    - Filtering en selectie via query parameters
    - Validatie van zoekopdrachten op basis van metadata
    - Paginering volgens standaard patronen

## Interactiepatronen

- REST-conforme endpoints
- Gestandaardiseerde fout-responses
- Filtering via query parameters
- Paginering volgens API-strategie richtlijnen

<a id="gemeenschappelijke-metadata-definitie"></a>
## Gemeenschappelijke $metadata definitie

De DD API V3 gebruikt één gemeenschappelijke $metadata definitie voor OData. Dit zorgt voor consistentie en voorspelbaarheid tussen verschillende implementaties.

### Kenmerken
- **Centraal beheerd**: Één bron van waarheid voor alle implementaties
- **Gestandaardiseerd**: Vaste structuur voor observations en referenties
- **Voorspelbaar**: Clients weten precies welke eigenschappen beschikbaar zijn
- **Uitbreidbaar**: Nieuwe concepten kunnen gecontroleerd worden toegevoegd

### Voordelen
- 🔄 Automatische client-generatie mogelijk
- 📊 Consistente data-uitwisseling tussen systemen
- 🛠️ Vereenvoudigde implementatie voor providers
- 🔍 Betere controle op datastructuren
- ⚡ Efficiëntere queries door bekende structuur

### Praktische implementatie
- De $metadata is beschikbaar via het standaard OData endpoint `/odata/v3/$metadata`
- Implementaties MOETEN deze exacte metadata definitie gebruiken
- Afwijkingen zijn NIET toegestaan om interoperabiliteit te waarborgen
- Updates worden centraal beheerd en gepubliceerd


<a id="openapi-specification-oas"></a>
## OpenAPI Specification (OAS)

Naast de OData $metadata levert DD API V3 ook een OpenAPI Specification (OAS). Dit maakt de API toegankelijk voor ontwikkelaars die gewend zijn aan REST APIs zonder OData ervaring.

### Doel
- **Complementair aan OData**: Biedt alternatieve toegang naast OData
- **Laagdrempelig**: Bekend formaat voor veel ontwikkelaars
- **Tooling support**: Breed ondersteund door ontwikkeltools
- **Documentatie**: Zelf-documenterend karakter van de API

### Kenmerken
- 📚 Complete API beschrijving in YAML/JSON formaat
- 🔍 Gedetailleerde endpoint documentatie
- 📋 Schema definities voor alle objecten
- 🔒 Security specificaties
- 📊 Request/response voorbeelden

### Praktische toepassing
- Beschikbaar via `/swagger.json` en `/swagger.yaml`
- Swagger UI voor interactieve documentatie
- Code generatie mogelijk voor diverse programmeertalen
- Automatische client generatie ondersteund

### Beperkingen
- OData query mogelijkheden zijn beperkt beschreven
- Complexe filters worden apart gedocumenteerd
- Focus ligt op basis REST operaties

<a id="subset-van-odata-functionaliteit"></a>
## Subset van OData functionaliteit

DD API V3 gebruikt bewust een beperkte subset van OData functionaliteit om complexiteit te reduceren en implementatie te vereenvoudigen.

Gebruik van `$skiptoken` lost ook een ander probleem op waar DD-API V1 en V2 last van hadden: de paginering werkte niet binnen alle systemen gelijk.

### Ondersteunde features
- **Query opties**:
    - `$select`: Specifieke properties ophalen
    - `$filter`: Filteren van resultaten
    - `$orderby`: Sortering
    - `$top`: Maximum aantal resultaten
    - `$skiptoken`: Server-gestuurde paginering
    - `$count`: Totaal aantal resultaten
    - `$expand`: Alleen voor observations binnen Feature of Interest

- **Operatoren**:
    - Vergelijkingen: eq, ne, gt, ge, lt, le
    - Logisch: and, or, not
    - Functies: startswith, endswith, contains
    - Collecties: any, all
    - Vereenvoudigde geo-functies:
        - intersects: Vereenvoudigde doorsnede test
        - distance: Vereenvoudigde afstandsberekening
        - (Let op: Dit zijn geen standaard OData geo-functies)
        - eenvoudiger in gebruik en niet systeem-afhankelijk

- **Specifieke expand support**:
    - `observations`: Binnen Feature of Interest
    - Andere expand scenarios worden NIET ondersteund

### Niet ondersteund
- `$skip`: Geen client-gestuurde paginering
- `$compute`: Geen berekende velden
- `$search`: Geen vrije tekst zoeken
- `$apply`: Geen aggregaties
- Complexe lambda operaties
- Delta queries
- Algemene expand operaties
- Standaard OData geo.* functies

<a id="vocabulaires"></a>
## Vocabulaires

De DD API maakt gebruik van gestandaardiseerde vocabulaires voor categorieën en parameters. 
Deze vocabulaires zijn essentieel voor een consistente interpretatie van de data.

### Categorieën
- Definieert het type Feature of Interest
- Verplicht veld voor elke Feature
- Waarden komen uit AQUO-standaard waar mogelijk
- Voorbeelden: meetpunt, meetnet

### Parameters
- Beschrijft wat er gemeten/geobserveerd wordt
- Verplicht voor elke Observation
- Gebaseerd op AQUO-parameters waar mogelijk
- Aangevuld met domein-specifieke parameters indien nodig

### Validatie
- Features zonder geldige categorie worden geweigerd
- Observations zonder geldige parameter worden geweigerd
- Case-sensitive matching
- Spaties en leestekens zijn significant

### Beheer
- Vocabulaires worden centraal beheerd
- Uitbreidingen mogelijk op verzoek
- Wijzigingen worden vooraf aangekondigd
- Versie informatie beschikbaar via API

## Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |
