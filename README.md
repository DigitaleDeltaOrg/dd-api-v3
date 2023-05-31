# DD-API V3

The DD-API V3 is an API for water management in the Netherlands. 
It is based on standards:

- [Observations, Measurements and Sampling (in preview)](https://github.com/opengeospatial/om-swg) is an [OGC](https://www.ogc.org/) standard and soon-to-be-[ISO](https://www.iso.org/home.html) standard for exchanging observation data.
- [OData](https://odata.org) is an [ISO/IEC]() approved [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=odata) [standard](https://standards.iso.org/ittf/PubliclyAvailableStandards/c069208_ISO_IEC_20802-1_2016.zip) for searching and filtering data.
- [MQTT](https://mqtt.org/) is an open [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=odata) standard and an [ISO](https://www.iso.org/home.html) recommendation [ISO/IEC 20922](https://www.iso.org/standard/69466.html#:~:text=ISO%2FIEC%2020922%3A2016%20is%20a%20Client%20Server%20publish%2Fsubscribe%20messaging,designed%20so%20as%20to%20be%20easy%20to%20implement.) for subscriptions in changes of data.

Another standard is integrated into this standard: [SensorThings API](http://opengeospatial.github.io/e-learning/sta/text/main.html#:~:text=The%20SensorThings%20API%20allows%20for%20the%20access%20and,of%20being%20identified%20and%20integrated%20into%20communication%20networks.).
Management of entities (Thing and Actuator) are meshed with the [reference system](/required/reference.md) and it's specific data format is merged with the [observation responses](/Schemas/observation-schema.json).
Control of Thing and Actuator are kept in their original endpoints.

The [OM&S](https://github.com/opengeospatial/om-swg) implementation is a *strict* subset:

- Conforms to [OM&S](https://github.com/opengeospatial/om-swg)
- Is a *minimal* requirement
- Leaves little to no room for interpretation

The API defines three pieces of information to make sure that the experience of the user is the same across all 
different implementations of the DD-API V3.

- [OData definition](#odata-definition)
- [OAS definition](#oas-definition)
- [Semantic definition](#semantic-definition)

Sample code in C# for the Proof of Concept is maintained [here](https://github.com/DigitaleDeltaOrg/dd-api-v3-CSharp).

## Definitions

All the definition specifications have a semantic version system according to [semver.org](https://semver.org/).
Patches are not used, only **MAJOR** and *MINOR* versions.

The **MAJOR** version is always the 4-digit year of the specification.

*MINOR* versions are not allowed to break anything for implementations that use the same **MAJOR** version.
*MINOR* versions are only allowed to **ADD** to the specification and *cannot* modify behavior.
The implementor **MUST** implement a new **MAJOR** version within three months of the publication here on GitHub.
The implementor is *encouraged* to support a new *MINOR* version as soon as possible.
Older **MAJOR** version **MUST** be supported at least for ONE YEAR after a *MAJOR** version is released.
This means that two **MAJOR** versions can co-exist.

The client can request a specific **MAJOR** version of **OData** by specifying the appropriate header:

- odata

the 'major-version' header with a specific version, i.e. 2023.

The responses will always return the specific versions for the data:

```
@sem=2023.01
@odata=2023.01
```

This information is provided once per result page.

Some other items may be given in the responses, depending on the implementation.

In addition, [KennisPlaform API's Rule 57](https://publicatie.centrumvoorstandaarden.nl/api/adr/#api-57) must be followed.
This means that the response header *must* have property API-Version set with the semantic version of the OAS specification.

## OData definition

The OData definition is standard across all DD-API V3 implementations. It's implementation is **mandatory** 
and specifies the **minimal** implementation required for data query and retrieval.
This will be known as 'NL Profiel Waterbeheer'.

An OData definition is written in [Conceptual Schema Definition Language]
(https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html) (CSDL)
and defines an Entity Data Model.

Compliance may be checked by comparing the definition in this GitHub with the generated definition 
at the /odata/$metadata endpoint.

See [CSDL](https://docs.oasis-open.org/odata/odata-csdl-json/v4.01/odata-csdl-json-v4.01.html) for background 
information and the relation between EDM and CSDL.

## OAS definition

All implementations are required to implement the following endpoints:

The [query endpoints](required/query.md) for **observation** and **reference** are **mandatory**.

The following functionalities are *optional*:
- [data management](optional/data-management.md)
- [SensorThings (STA)](optional/sensorthings.md)
- [Subscription endpoints](optional/subscriptions.md)

Optional modules *must* follow the definitions.

Compliance may be checked by comparing the store definition in this GitHub with the generated definition at the 
/v3/$openapi endpoint and the /v3/odata/$metadata endpoint.

Some frameworks generate the OAS specification automatically from an OData specification. These may produce the OAS at the $openapi endpoint.
However,  [Kennisplatform API's Rule 51](https://publicatie.centrumvoorstandaarden.nl/api/adr/#api-51) specifies that the OAS definition must be published at openapi.json or openapi.xml.
An option is to copy the $openapi to openapi.json on the correct endpoint to satisfy [Kennisplatform API's Rule 51](https://publicatie.centrumvoorstandaarden.nl/api/adr/#api-51).

## Semantic definition

The semantic definition specifies where references are defined. The implementation is **mandatory**. 
For instance: parameters are defined in Aquo, quantities **must** exist in group 'quantity' within the 
Aquo Parameter table, biological taxa are defined in TWN, Length classes are defined as 'length class' in Aquo.
This ensures that data is the same across the systems and have the same meaning.

Organisations can have there own classifications, next to the standard definition. 
However, if the semantic standard defines an entity, the use of that entity is **mandatory**. 
So: standard always outweighs an organisation-standard.
It is the responsibility of the implementor/data maintainer that the correct mapping is performed.

## Authentication

Kennisplatform API allows only authentication using OAUTH2, with two specific flows:

- Authorisation code flow (user authorisation)
- Client credentials flow (machine-to-machine)

In addition, OpenID Connect is allowed to facilitate user authorisation.
API keys are not allowed for authorization purposes.

## GeoJSON

GeoJSON does not support Coordinate Reference Systems (CRS) (anymore). 
To solve CRS issues in GeoJSON, follow https://docs.geostandaarden.nl/crs/crs/, section 3.4.2.
In short: all GeoJSON instances in a response have exactly the same CRS. 
The default CRS is WGS84.
The client can request a different CRS by adding a HTTP header named Accept-Crs with the desired CRS.
The response will add an HTTP header named Content-Crs to express that the response is in a different CRS than default.
The request of the client *may* not be honored if the server is not able to convert to the requested CRS.
The API is **required** to support **at least** the following CRS **by their urn**:

- WGS84 (urn:ogc:def:crs:EPSG::4326)
- ETRS89 (urn:ogc:def:crs:EPSG::4258)
- RD (urn:ogc:def:crs:EPSG::28992)
- Web Mercator (urn:ogc:def:crs:EPSG::3857)

## Proof of Concept

In the /Source/DotNet part of this GitHub repository, a simple implementation of a proof of concept is available in .NET 7/C# 11.
It implements a simple yet functional and fast storage model using PostgreSQL 15. The implementor can, of course, implement their own storage model or extend their own platform to provide the functionality.

The management and subscription endpoints are not yet implemented in the Proof of Concept. That part is Work in Progress.

## TODO

- As long as there is no official TimeseriesML JSON encoding, define our own encoding, based on
  [om-json](https://github.com/peterataylor/om-json).
- Convert to the new Kennisplatform API's modular naming strategy.
- Link to Kennisplatform API's OData module, when available.

## History

- 20230226: Move ObservedProperty, observingProcedure, observer and host are moved to 'Parameter', 
  to simplify filtering. Foi stays a separate entity, since it involves geometry *and* it is required.
  'Parameter' is used to keep the search interface clean. OMS allows this as Parameter is defined as a multi-name/value property.
- 20230227: Add 'ParameterDetails' to expand the details of the parameter.
- 20230228: Add 'Metadata' to add additional information that is not referable.
- 20230301: Change PoC to new parameter specification. This also applies to the PoC suggested database model.
- 20230410: Rename UniAPI to DD-API V3, in de description and in code.
- 20230315: Revisit the UML. Base for SensorThings API and data management (including Thing and Actuator).
- 20230427: Improved documentation
- 20230510: Clarify OData => Kennisplatform API's rules

