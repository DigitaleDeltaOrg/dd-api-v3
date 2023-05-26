# Subscriptions

Subscriptions is an optional module. It allows notifications to specific changes in data by means of the MQTT protocol.

https://github.com/node-red/designs/blob/master/designs/dynamic-mqtt-node.md

With subscriptions, the implementation acts as an [MQTT](https://en.wikipedia.org/wiki/MQTT) broker.
[MQTT](https://en.wikipedia.org/wiki/MQTT) is an [OASIS](https://en.wikipedia.org/wiki/OASIS_(organization) standard and an [ISO](https://en.wikipedia.org/wiki/International_Organization_for_Standardization) recommendation.

The implementation will regularly check the subscription list and perform the requested query. The implementation will then store the last time the query for the subscription was performed and the result was returned to the subscriber. Only changes reported after that timestamp will be returned at the next request.

The subscription will be in the form of an OData query.

The minimum subscription period will be 60 seconds.

All subscription requests must authorize via OAUTH2 Client Credential Flow.

The response format will be the same as the response format for [observation](../required/observation.md).

The request is expressed in OData.
When new data arrives, the server is responsible to query the subscriptions and report new data in observation format to the subscribers of said data.
This can be handled in the background, however.