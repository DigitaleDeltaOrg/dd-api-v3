Integrate the following:

# EDMX notes

The EDMX is the heart of DD-API V3: it defines the query interface and the export definition.

It consists of two main parts.

## Reference

A Reference is a classification: something that is defined somewhere else.
In DD-API V3 almost everything is a reference:

- FoI (Feature of Interest, Measurement object, location, sensor)
- UoM (Unit of Measure)
- Compartment
- Parameter specification (chemical, biological, physical, taxon type, taxon group)
- Organisation (owner, sampler, analyst)
- Limit Symbol
- ...

Each reference has a type (which is also a reference).

The types of reference are described in the official property-list.
If the property-list has source columns, the source columns point to a URL containing valid values for it.
If the Standard-column in the property-list for the property is set to 'Standard', then the value **must** conform to one of the sources as specified.

## Observation

An observation describes what was measured (but also when, by whom, where and how).
It consists of the following blocks:

### Parameter

Parameter, despite its official OGC name, is a dictionary of key/value pairs.
All keys in the Parameter block MUST be present in Reference, and also its values MUST be present in Reference.

### Metadata
Metadata is also a dictionary of key/value pairs.
All keys in the Metadata block must be present in Reference. The values, however, do not.

### Result

The result block is fixed. If additions are required, the Architecture Board will take the request into consideration.

The following XML shows a minimal EDMX for the Digitale Delta API:

Extension points are marked with:                 

``` <!-- Additional properties here --> ```

The EDMX is **ONLY** allowed to be extended at those points.
Properties added there must follow these rules:

- **MUST** be present in the official property-list. If the list does not contain the property, contact the Digitale Delta Architecture Board.
- Content for the properties in the Observation/ParameterContainer **MUST** have a presence in the Reference list of the implementation.
- For properties in the official property-list that specify one or more sources, the Code field **MUST** have content that is present in one of the listed sources.

These rules make sure that there is no room for interpretation.



``` XML

<edmx:Edmx xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx" Version="4.0">
    <edmx:DataServices>
        <Schema xmlns="http://docs.oasis-open.org/odata/ns/edm" Namespace="DigitaleDelta">
            <EntityType Name="Reference">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="Organisation" Type="Edm.String"/>
                <Property Name="Code" Type="Edm.String" Nullable="false"/>
                <Property Name="Geometry" Type="Edm.GeometryPoint"/>
                <Property Name="Description" Type="Edm.String" Nullable="false"/>
                <!-- Additional properties here -->
            </EntityType>
            <EntityType Name="Observation">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="ResultTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="PhenomenonTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="ValidTime" Type="Edm.DateTimeOffset"/>
                <Property Name="ResultQuality" Type="Collection(DigitaleDelta.DqElement)"/>
                <Property Name="Parameter" Type="DigitaleDelta.ParameterContainer" Nullable="false"/>
                <Property Name="Metadata" Type="DigitaleDelta.MetadataContainer"/>
                <NavigationProperty Name="RelatedObservations" Type="Collection(DigitaleDelta.RelatedObservation)"/>
                <NavigationProperty Name="Foi" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Result" Type="DigitaleDelta.Result" Nullable="false"/>
            </EntityType>
            <EntityType Name="RelatedObservation" BaseType="DigitaleDelta.Observation">
                <Property Name="Role" Type="Edm.String"/>
            </EntityType>
            <ComplexType Name="DqElement"/>
            <ComplexType Name="ParameterContainer">
                <Property Name="Parameter" Type="Edm.String"/>
                <!-- Additional properties here -->
            </ComplexType>
            <ComplexType Name="MetadataContainer">
                <!-- Additional properties here -->
            </ComplexType>
            <EntityType Name="Result">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Truth" Type="Edm.Boolean"/>
                <Property Name="Count" Type="Edm.Int64"/>
                <Property Name="Measure" Type="DigitaleDelta.Measure"/>
                <Property Name="Vocab" Type="DigitaleDelta.CategoryVerb"/>
                <Property Name="Geometry" Type="Edm.GeometryPoint"/>
                <NavigationProperty Name="Timeseries" Type="DigitaleDelta.TimeseriesResult"/>
                <!-- Additional result TYPES here -->
            </EntityType>
            <ComplexType Name="Measure">
                <Property Name="Uom" Type="Edm.String" Nullable="false"/>
                <Property Name="Value" Type="Edm.Decimal" Nullable="false" Scale="Variable"/>
            </ComplexType>
            <ComplexType Name="CategoryVerb">
                <Property Name="Vocabulary" Type="Edm.String" Nullable="false"/>
                <Property Name="Term" Type="Edm.String" Nullable="false"/>
            </ComplexType>
            <EntityType Name="TimeseriesResult">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="MetaData" Type="DigitaleDelta.TimeseriesMetadata"/>
                <Property Name="DefaultPointMetaData" Type="DigitaleDelta.PointMetadata"/>
                <Property Name="Points" Type="Collection(DigitaleDelta.PointData)" Nullable="false"/>
            </EntityType>
            <ComplexType Name="TimeseriesMetadata" Abstract="true">
                <Property Name="TemporalExtent" Type="Edm.String"/>
                <Property Name="BaseTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="Spacing" Type="Edm.String"/>
                <Property Name="CommentBlock" Type="DigitaleDelta.CommentBlock"/>
                <Property Name="CommentBlocks" Type="Collection(DigitaleDelta.CommentBlock)"/>
                <Property Name="IntendedObservationSpacing" Type="Edm.String"/>
                <Property Name="Cumulative" Type="Edm.Boolean"/>
                <Property Name="AccumulationAnchorTime" Type="Edm.DateTimeOffset"/>
                <Property Name="StartAnchorPoint" Type="Edm.DateTimeOffset"/>
                <Property Name="EndAnchorPoint" Type="Edm.DateTimeOffset"/>
                <Property Name="MaxGapPeriod" Type="Edm.DateTimeOffset"/>
                <NavigationProperty Name="Status" Type="DigitaleDelta.Reference"/>
            </ComplexType>
            <ComplexType Name="CommentBlock">
                <Property Name="ApplicablePeriod" Type="Edm.String" Nullable="false"/>
                <Property Name="Comment" Type="Edm.String" Nullable="false"/>
            </ComplexType>
            <ComplexType Name="PointMetadata">
                <Property Name="Comment" Type="Edm.String"/>
                <Property Name="Accuracy" Type="DigitaleDelta.Measure"/>
                <Property Name="AggregationDuration" Type="Edm.DateTimeOffset"/>
                <NavigationProperty Name="Quality" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Uom" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="InterpolationType" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="NilReason" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="CensoredReason" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="RelatedObservation" Type="DigitaleDelta.Observation"/>
                <NavigationProperty Name="Qualifier" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Processing" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Source" Type="DigitaleDelta.Reference"/>
            </ComplexType>
            <ComplexType Name="PointData">
                <Property Name="Time" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="Value" Type="Edm.Double" Nullable="false"/>
                <Property Name="MetaData" Type="DigitaleDelta.PointMetadata"/>
            </ComplexType>
        </Schema>
        <Schema xmlns="http://docs.oasis-open.org/odata/ns/edm" Namespace="Default">
           <EntityContainer Name="Container">
                <EntitySet Name="references" EntityType="DigitaleDelta.Reference">
                    <!-- Additional properties here -->
                </EntitySet>
                <EntitySet Name="observations" EntityType="DigitaleDelta.Observation">
                    <NavigationPropertyBinding Path="Foi" Target="references"/>
                    <NavigationPropertyBinding Path="RelatedObservations" Target="observations"/>
                </EntitySet>
            </EntityContainer>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>
```

An extended example is shown below. It consists of additions of properties for a wide range of ecological measurements. 

``` XML
<edmx:Edmx xmlns:edmx="http://docs.oasis-open.org/odata/ns/edmx" Version="4.0">
    <edmx:DataServices>
        <Schema xmlns="http://docs.oasis-open.org/odata/ns/edm" Namespace="DigitaleDelta">
            <EntityType Name="Reference">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="Organisation" Type="Edm.String"/>
                <Property Name="Code" Type="Edm.String" Nullable="false"/>
                <Property Name="Geometry" Type="Edm.GeometryPoint"/>
                <Property Name="Description" Type="Edm.String" Nullable="false"/>
                <Property Name="ParameterType" Type="Edm.String"/>
                <Property Name="TaxonRank" Type="Edm.String"/>
                <Property Name="TaxonAuthors" Type="Edm.String"/>
                <Property Name="TaxonNameNl" Type="Edm.String"/>
                <Property Name="TaxonStatusCode" Type="Edm.String"/>
                <Property Name="CasNumber" Type="Edm.String"/>
                <Property Name="Role" Type="Edm.String"/>
                <NavigationProperty Name="TaxonGroup" Type="DigitaleDelta.Reference">
                    <ReferentialConstraint Property="TaxonGroupId" ReferencedProperty="Id"/>
                </NavigationProperty>
                <NavigationProperty Name="TaxonType" Type="DigitaleDelta.Reference">
                    <ReferentialConstraint Property="TaxonTypeId" ReferencedProperty="Id"/>
                </NavigationProperty>
                <NavigationProperty Name="TaxonParent" Type="DigitaleDelta.Reference">
                    <ReferentialConstraint Property="TaxonParentId" ReferencedProperty="Id"/>
                </NavigationProperty>
            </EntityType>
            <EntityType Name="Observation">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="ResultTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="PhenomenonTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="ValidTime" Type="Edm.DateTimeOffset"/>
                <Property Name="ResultQuality" Type="Collection(DigitaleDelta.DqElement)"/>
                <Property Name="Parameter" Type="DigitaleDelta.ParameterContainer" Nullable="false"/>
                <Property Name="Metadata" Type="DigitaleDelta.MetadataContainer"/>
                <NavigationProperty Name="RelatedObservations" Type="Collection(DigitaleDelta.RelatedObservation)"/>
                <NavigationProperty Name="Foi" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Result" Type="DigitaleDelta.Result" Nullable="false"/>
            </EntityType>
            <EntityType Name="RelatedObservation" BaseType="DigitaleDelta.Observation">
                <Property Name="Role" Type="Edm.String"/>
            </EntityType>
            <ComplexType Name="DqElement"/>
            <ComplexType Name="ParameterContainer">
                <Property Name="Parameter" Type="Edm.String"/>
                <Property Name="Compartment" Type="Edm.String"/>
                <Property Name="AnalysisPackage" Type="Edm.String"/>
                <Property Name="KrwWatertype" Type="Edm.String"/>
                <Property Name="StowaWatertype" Type="Edm.String"/>
                <Property Name="Quantity" Type="Edm.String"/>
                <Property Name="Unit" Type="Edm.String"/>
                <Property Name="Project" Type="Edm.String"/>
                <Property Name="Note" Type="Edm.String"/>
                <Property Name="Method" Type="Collection(Edm.String)"/>
                <Property Name="MeasurementObject" Type="Edm.String"/>
                <Property Name="MonitoringNetwork" Type="Edm.String"/>
                <Property Name="Relation" Type="Edm.String"/>
                <Property Name="Organisation" Type="Edm.String"/>
                <Property Name="MeasurementPackage" Type="Edm.String"/>
                <Property Name="Sampler" Type="Edm.String"/>
                <Property Name="Analyst" Type="Edm.String"/>
                <Property Name="Checker" Type="Edm.String"/>
                <Property Name="MeasurementSetNumber" Type="Edm.String"/>
                <Property Name="CollectionReference" Type="Edm.String"/>
                <Property Name="LimitSymbol" Type="Edm.String"/>
                <Property Name="Purpose" Type="Edm.String"/>
                <Property Name="Ecotope" Type="Edm.String"/>
                <Property Name="ObservationType" Type="Edm.String"/>
                <Property Name="EcotopeType" Type="Edm.String"/>
                <Property Name="SampleComment" Type="Edm.String"/>
                <Property Name="Standard" Type="Edm.String"/>
                <Property Name="Actuator" Type="Edm.String"/>
                <Property Name="SensorThing" Type="Edm.String"/>
                <Property Name="ParameterType" Type="Edm.String"/>
                <Property Name="Observed" Type="Edm.String"/>
                <Property Name="LifeForm" Type="Edm.String"/>
                <Property Name="LifeStage" Type="Edm.String"/>
                <Property Name="Gender" Type="Edm.String"/>
                <Property Name="Quality" Type="Edm.String"/>
                <Property Name="LengthClass" Type="Edm.String"/>
                <Property Name="Statistics" Type="Edm.String"/>
                <Property Name="Sediment" Type="Edm.String"/>
                <Property Name="Wavelength" Type="Edm.String"/>
                <Property Name="Appearance" Type="Edm.String"/>
                <Property Name="Individuals" Type="Edm.String"/>
                <Property Name="GrainSizeFraction" Type="Edm.String"/>
                <Property Name="GrainDiameter" Type="Edm.String"/>
                <Property Name="Condition" Type="Edm.String"/>
                <Property Name="QualityAssessment" Type="Edm.String"/>
                <Property Name="ValuationTechnique" Type="Edm.String"/>
                <Property Name="Habitat" Type="Edm.String"/>
                <Property Name="MeasurementPosition" Type="Edm.String"/>
                <Property Name="ValuationMethod" Type="Edm.String"/>
                <Property Name="ValueProcessingMethod" Type="Edm.String"/>
                <Property Name="TaxonType" Type="Edm.String"/>
                <Property Name="TaxonGroup" Type="Edm.String"/>
                <Property Name="TaxonParent" Type="Edm.String"/>
            </ComplexType>
                <ComplexType Name="MetadataContainer">
                <Property Name="CollectionReference" Type="Edm.String"/>
                <Property Name="MeasurementSetNumber" Type="Edm.String"/>
                <Property Name="SampleComment" Type="Edm.String"/>
            </ComplexType>
            <EntityType Name="Result">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Truth" Type="Edm.Boolean"/>
                <Property Name="Count" Type="Edm.Int64"/>
                <Property Name="Measure" Type="DigitaleDelta.Measure"/>
                <Property Name="Vocab" Type="DigitaleDelta.CategoryVerb"/>
                <Property Name="Geometry" Type="Edm.GeometryPoint"/>
                <NavigationProperty Name="Timeseries" Type="DigitaleDelta.TimeseriesResult"/>
            </EntityType>
            <ComplexType Name="Measure">
                <Property Name="Uom" Type="Edm.String" Nullable="false"/>
                <Property Name="Value" Type="Edm.Decimal" Nullable="false" Scale="Variable"/>
            </ComplexType>
            <ComplexType Name="CategoryVerb">
                <Property Name="Vocabulary" Type="Edm.String" Nullable="false"/>
                <Property Name="Term" Type="Edm.String" Nullable="false"/>
            </ComplexType>
            <EntityType Name="TimeseriesResult">
                <Key>
                    <PropertyRef Name="Id"/>
                </Key>
                <Property Name="Id" Type="Edm.String" Nullable="false"/>
                <Property Name="Type" Type="Edm.String" Nullable="false"/>
                <Property Name="MetaData" Type="DigitaleDelta.TimeseriesMetadata"/>
                <Property Name="DefaultPointMetaData" Type="DigitaleDelta.PointMetadata"/>
                <Property Name="Points" Type="Collection(DigitaleDelta.PointData)" Nullable="false"/>
            </EntityType>
            <ComplexType Name="TimeseriesMetadata" Abstract="true">
                <Property Name="TemporalExtent" Type="Edm.String"/>
                <Property Name="BaseTime" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="Spacing" Type="Edm.String"/>
                <Property Name="CommentBlock" Type="DigitaleDelta.CommentBlock"/>
                <Property Name="CommentBlocks" Type="Collection(DigitaleDelta.CommentBlock)"/>
                <Property Name="IntendedObservationSpacing" Type="Edm.String"/>
                <Property Name="Cumulative" Type="Edm.Boolean"/>
                <Property Name="AccumulationAnchorTime" Type="Edm.DateTimeOffset"/>
                <Property Name="StartAnchorPoint" Type="Edm.DateTimeOffset"/>
                <Property Name="EndAnchorPoint" Type="Edm.DateTimeOffset"/>
                <Property Name="MaxGapPeriod" Type="Edm.DateTimeOffset"/>
                <NavigationProperty Name="Status" Type="DigitaleDelta.Reference"/>
            </ComplexType>
            <ComplexType Name="CommentBlock">
                <Property Name="ApplicablePeriod" Type="Edm.String" Nullable="false"/>
                <Property Name="Comment" Type="Edm.String" Nullable="false"/>
            </ComplexType>
            <ComplexType Name="PointMetadata">
                <Property Name="Comment" Type="Edm.String"/>
                <Property Name="Accuracy" Type="DigitaleDelta.Measure"/>
                <Property Name="AggregationDuration" Type="Edm.DateTimeOffset"/>
                <NavigationProperty Name="Quality" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Uom" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="InterpolationType" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="NilReason" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="CensoredReason" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="RelatedObservation" Type="DigitaleDelta.Observation"/>
                <NavigationProperty Name="Qualifier" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Processing" Type="DigitaleDelta.Reference"/>
                <NavigationProperty Name="Source" Type="DigitaleDelta.Reference"/>
            </ComplexType>
            <ComplexType Name="PointData">
                <Property Name="Time" Type="Edm.DateTimeOffset" Nullable="false"/>
                <Property Name="Value" Type="Edm.Double" Nullable="false"/>
                <Property Name="MetaData" Type="DigitaleDelta.PointMetadata"/>
            </ComplexType>
        </Schema>
        <Schema xmlns="http://docs.oasis-open.org/odata/ns/edm" Namespace="Default">
            <EntityContainer Name="Container">
                <EntitySet Name="references" EntityType="DigitaleDelta.Reference">
                    <NavigationPropertyBinding Path="TaxonGroup" Target="references"/>
                    <NavigationPropertyBinding Path="TaxonParent" Target="references"/>
                    <NavigationPropertyBinding Path="TaxonType" Target="references"/>
                    <Annotation Term="Org.OData.Capabilities.V1.SortRestrictions">
                        <Record>
                            <PropertyValue Property="Sortable" Bool="true"/>
                            <PropertyValue Property="AscendingOnlyProperties">
                                <Collection/>
                            </PropertyValue>
                            <PropertyValue Property="DescendingOnlyProperties">
                                <Collection/>
                            </PropertyValue>
                            <PropertyValue Property="NonSortableProperties">
                                <Collection>
                                    <PropertyPath>TaxonTypeId</PropertyPath>
                                    <PropertyPath>TaxonGroupId</PropertyPath>
                                    <PropertyPath>TaxonParentId</PropertyPath>
                                    <PropertyPath>Geometry</PropertyPath>
                                </Collection>
                            </PropertyValue>
                        </Record>
                    </Annotation>
                </EntitySet>
                <EntitySet Name="observations" EntityType="DigitaleDelta.Observation">
                    <NavigationPropertyBinding Path="Foi" Target="references"/>
                    <NavigationPropertyBinding Path="RelatedObservations" Target="observations"/>
                </EntitySet>
            </EntityContainer>
        </Schema>
    </edmx:DataServices>
</edmx:Edmx>

```


