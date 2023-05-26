# SensorThings

DD-API V3 integrates SensorThings API functionality. However, to mash SensorThings API in, some concessions have been
made:

Actuator and Thing are defined as references. Modifications to them are handled by the references endpoint.
Instead of data streams, either CoverageJSON of TimeseriesML are used.


TODO: SensorThings API uses (possibly) API keys for authentication. These have to be replaced by OAUTH2 client
credentials flow.
