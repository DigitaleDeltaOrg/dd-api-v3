# Reference

A reference is anything that can be classified within the implementation that can occur in the following properties of an [observation](observation.md):

- foi (feature of interest)
- parameter/*
- result/measure/uom (unit of measurement)
- result/vocab/vocabulary
- result/vocab/verb
- result/timeseries/TimeseriesMetadata/Status
- result/timeseries/PointMetadata/Accuracy/measure/uom
- result/timeseries/PointMetadata/Quality
- result/timeseries/PointMetadata/Uom
- result/timeseries/PointMetadata/InterpolationType
- result/timeseries/PointMetadata/NilReason
- result/timeseries/PointMetadata/CensoredReason
- result/timeseries/PointMetadata/Qualifier
- result/timeseries/PointMetadata/Processing

This can be either a standard, or part of organisation-specific data, 
if the [semantic definition](../definition/semantic/v2023.01/semantic.json) does not provide the same or approximate 
definition.

A reference should **always** convey meaning, so it should not solely be a UUID.
If a UUID is used, 
the reference should make use of a fragment (#) to add a human-readable text explaining the information.

The definition of reference can be found [here](../definition/csdl/v2023.01/csdl.xml).
Optional properties are allowed.

A feature of interest *should* include a link to a NEN3610:2022 compliant page, 
detailing information about the measurement location.
Information can include information such as: 

- area usage
- monitoring networks
- reporting networks
- water types
- ecotopes
- schematics
- photos
- documents

Reference can include implementation-specific reference types, 
such as monitoring networks, reporting networks, taxon types, taxon groups, etc.
References can be used to improve filtering of data, thus making the use of the API easier.
I.e. filtering on a monitoring network is easier than specifying a feature of interest list.




