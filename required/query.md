# Query endpoints

Query endpoints are used to retrieve data using OData. Implementation of these endpoints is **mandatory**.

All query endpoints use OData and **must** implement the following OData features:

- $filter
- $expand
- $select

The following filter operators **must** be implemented:
- eq (string, number, boolean, date, datetime, time, uuid)
- ne (string, number, boolean, date, datetime, time, uuid)
- gt (string, number, date, datetime, time)
- ge (string, number, date, datetime, time)
- lt (string, number, date, datetime, time)
- le (string, number, date, datetime, time)
- and
- or
- not
- in (string, number, boolean, date, datetime, time, uuid)
- ( and )

The following filter functions **must** be implemented:

- geo.distance (point, polygon)
- geo.intersects (point, polygon)
- contains (string, collection)
- startswith (string)
- endswith (string)
- length (string)
- indexof (string)
- substring (string)
- tolower (string)
- toupper (string)
- trim (string)
- year (date, datetime)
- month (date, datetime)
- day (date, datetime)
- hour (date, datetime, time)
- minute (date, datetime, time)
- second (date, datetime, time)
- round (number)
- floor (number)
- ceiling (number)
- isnull (string, number, boolean, date, datetime, time, guid)
- isnotnull (string, number, boolean, date, datetime, time, guid)

Specific implementations may implement additional domain-specific functions.
These will be specified in the OData definition.

## /v3/odata/reference

The /reference endpoint uses OData to query references. References are considered *immutable*.
References are described [here](reference.md).

For public data, no security is required.
For non-public data, an OAUTH2 Resource Owner flow is required for interactive sessions, or an OAUTH2 Authorization Code flow for non-interactive sessions, such as machine-to-machine scenarios.

## /v3/odata/observation

The /observation endpoint uses OData to query observations, measurements, samples, timeseries, grids, etc.
Observations are described [here](observation.md).

For public data, no security is required.
For non-public data, an OAUTH2 Resource Owner flow is required for interactive sessions, or an OAUTH2 Authorization Code flow for non-interactive sessions, such as machine-to-machine scenarios.

