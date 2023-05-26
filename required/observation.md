# Observation

An observation is a standard observation, according to [Observations, Measurements &amp; Sampling](https://portal.ogc.org/files/?artifact_id=41510).
The definition in CSDL (Conceptual Schema Definition Language) form for observation can be found [here](/definition/csdl/v2023.01/csdl.xml).

The supported observation types (result types) are:

- count
- measure
- timeseries
- truth
- categoryverb
- geometry

Timeseries are encoded in TimeseriesML/WaterML, capable of also handling grids, coverages, etc.

Observations in OMS have a limitation: 
only one value (a timeseries is considered a value) can be stored in an observation,
without having to model a specific new observation type.

This is solved by using RelatedObservations.

An observation in the DD-API V3 is a combination of the following properties:

- [Id](#Id)
- [Type](#Type)
- [ResultTime](#ResultTime)
- [PhenomenonTime](#PhenomenonTime)
- [ValidTime](#ValidTime)
- [Foi](#Foi)
- [Parameter](#Parameter)
- [Metadata](#Metadata)
- [Result](#Result)
- [RelatedObservation](#RelatedObservation)

## Id

The Id property is a unique string, within *at least* the context of the organisation to which the observation belongs. 
We **strongly** suggest the use of UUID v4.

## Type

The type defines the format of the [Result](#result). It can be one of the following:

- [categoryverb](#categoryverb) (a combination of a category and a verb)
- [count](#count) (an integer value)
- [coverage](#coverage) (CoverageJSON)
- [geometry](#geometry) (a geometry, encoded in GeoJSON)
- [measure](#measure) (a combination of a decimal value and a unit of measurement, where unit of measurement (uom) 
  is a reference)
- [timeseries](#timeseries) (a timeseries, encoded in TimeseriesML)
- [truth](#truth) (a boolean value)

## ResultTime

Analysis time of the observation.

## PhenomenonTime

Sample time of the observation.

## ValidTime

A set of date/time fields representing the period for which the observation is valid.

## Foi

Foi stands for Feature of interest.
In the DD-API V3 context, it is considered a measurement object representing the location where the sampling was performed.
There reason for this: 

- Foi, according to O&M and OM&S is very flexible. This makes is hard to filter and combine data from different sources 
  in a meaningful way
- Having FOI as a location makes filtering easier and more intuitive than filtering on coordinates
- In the context of the DD-API V3, location is always required
- The only reference type that has geometry, is location and the FoI **must** occur in the Reference system with 
  type ```location```.

The Foi property is a reference to a [reference](reference.md) **of type 'measurementobject'**.

## Parameter

The Parameter property (despite its name) consists of a dictionary of key/value pairs, 
where the key is a unique string within the observation 
and the value is an existing reference within the implementation or an external link, 
provided it conveys relevant meaning.
All keys in the dictionary form an additional piece of information observed for the observation that can be classified. 
The key represents the reference type, the value is a known reference within that reference type.

The Parameter property is *mandatory*, since it conveys the meaning of the observation.
Properties of the parameter can be any reference. Due to the nature of key/value pairs, a key can only occur once 
in the dictionary.

Note: data that can be used to identify **a person** are not allowed to be retrieved by the 
DD-API V3 to avoid AVG issues.

The properties within the parameter can also be used for convenience. For example: specifying a taxongroup makes it 
easy to filter all taxa belonging to the provided group.

## ParameterDetails

When the ```$expand``` parameter is used and its value is either * or contains ParameterDetails,
the result of the observation is expanded with the details of the parameter's properties.

Note: data that can be used to identify **a person** are not allowed to be retrieved by the 
DD-API V3 to avoid AVG issues.

## Metadata

The Metadata property consists of a dictionary of key/value pairs,
where the key is a unique string within the observation
and the value is a string with data that are not referable. 
I.e. the sample number from which the observation was taken.
All keys in the dictionary form an additional piece of information observed for the observation that can be classified.
The Metadata property is optional.

Note: data that can be used to identify **a person** are not allowed to be retrieved by the 
DD-API V3 to avoid AVG issues.

## RelatedObservation

RelatedObservation is a dictionary of key/value pairs,
where the key is a unique string within the observation
and the value is the id of an existing observation within the implementation that is related to the observation.
The key represents the type of relation (its role) with the source.

I.e. it can be used to provide an observed unit/value next to the specified unit/value
or to provide detailed coordinates for the location where the sample was taken (i.e. measurements on a boat).

The RelatedObservation property is *optional*.

## Result

The Result property is the value of the observation.
Its type defines the format of the result.
'Result' is a required property and must have one of the following types:

### Count

The count result type is an integer value.

### CategoryVerb

The Categoryverb result type is a combination of a category and a verb. Both are [references](reference.md).
A verb is a reference to a verb, a category is a reference to a category, where the verb is a value within the category.

### Coverage

Coverages are described in [CoverageJSON format](https://covjson.org/). It is an OGC Community Standard.

### Geometry

The geometry result type is a geometry, encoded in GeoJSON. Determine the impact of JSON-FG instead of GeoJSON.

### Measure

The measure result type is a combination of a decimal value and a unit of measurement, 
where unit of measurement (uom) is a reference.

### Timeseries

Timeseries are described [here](https://docs.ogc.org/is/15-043r3/15-043r3.html).
Timeseries are encoded in TimeseriesML. 

However, there is no official JSON encoding available yet.
A proper JSON encoding is needed for the DD-API V3.
[Here](https://github.com/peterataylor/om-json) is a proposal for a JSON encoding in the files:

- Temporal.json
- TimeseriesMetadata.json
- TimeseriesTVP.json
- TimeseriesMonitoringFeature.json

### Truth

The truth result type is a boolean value.

## Formats

This document describes the base format: Digital Delta JSON.
However, other formats can be allowed and specified by the ```$format``` URL fragment, or by providing the format using 
the ```Accept```-header. Note: ```$format``` takes priority over the ```Accept```-header.

Possible formats:

| Code    | Format description                                                 |
|---------|--------------------------------------------------------------------|
| v3      | default format: the new DD-API v3 format                           |
| v1      | original DD-API v1 format                                          |
| netcdf  | NetCdf (grids)                                                     |
| wk      | DD-Waterkwaliteit-API format                                       |
| covjson | CoverageJson (coverages)                                           |
| v3-flat | as v3, but timeseries measurements are flattened into observations |

These formats are *optional*.