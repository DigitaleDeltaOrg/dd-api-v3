<link rel="stylesheet" type="text/css" href="/custom.css">

# Referentietypes

DD API V3 kent de volgende referentietypes.

Dit overzicht is ook als [CSV bestand]() beschikbaar.
Ook wordt deze in [JSON formaat]() aangeboden. 

## Referentietypes voor Parameter

| ReferenceType         | Betekenis                                                                                      | Bron     | Gebruikt voor |
|-----------------------|------------------------------------------------------------------------------------------------|----------|---------------|
| AnalysisPackage       | Combinatie van grootheden/eenheden/parameter types waarmee analyses kunnen worden uitgevoerd   | AquaDesk | Alle          |
| Appearance            | Verschijningsvorm                                                                              | Aquo     | Biologie      |
| Compartment           | Compartiment                                                                                   | Aquo     | Alle          |
| Condition             | Hoedanigheid                                                                                   | Aquo     | Alle          |
| Ecotope               | Ecotoop                                                                                        | Aquo     | Alle          |
| Gender                | Geslacht                                                                                       | Aquo     | Biologie      |
| GrainDiameter         | Korreldiameter                                                                                 | Aquo     | Alle          |
| GrainSizeFraction     | Korrelgroottefractie                                                                           | Aquo     | Alle          |
| Grootheid             | Geobserveerde entiteit waarover iets gezegd wordt                                              | Aquo     | Alle          |
| Habitat               | Habitat                                                                                        | Aquo     | Alle          |
| Individuals           | Individuen                                                                                     | Aquo     | Biologie      |
| KRWWaterType          | Watertype volgens KaderRichtlijn Water                                                         | Aquo     | Alle          |
| LengthClass           | Lengteklasse                                                                                   | Aquo     | Alle          |
| LengthClassInCm       | Lengteklasse in CM                                                                             | RWS      | Biologie      |
| LengthClassInMm       | Lengteklasse in MM                                                                             | RWS      | Biologie      |
| LifeForm              | Levensvorm                                                                                     | Aquo     | Biologie      |
| LifeStage             | Levensstadium                                                                                  | Aquo     | Biologie      |
| LimitSymbol           | Limiet symbool (t.b.v. grenswaarde)                                                            | Aquo     | Alle          |
| Literature            | Literatuur                                                                                     | Aquo     | Alle          |
| MeasurementPackage    | Combinatie van grootheden/eenheden/parameter types waarmee validaties kunnen worden uitgevoerd | AquaDesk | Alle          |
| MeasurementPosition   | Meetpositie                                                                                    | Aquo     | Alle          |
| Method                | Analysemethode                                                                                 | Aquo     | Alle          |
| MonitoringNetwork     | Meetnet                                                                                        | Aquo     | Alle          |
| Organisation          | Organisatie                                                                                    | Aquo     | Alle          |
| Parameter             | Gemeten parameter                                                                              | Aquo     | Alle          |
| ParameterType         | Parametertype                                                                                  | Aquo     | Alle          |
| Purpose               | (Meet)doel                                                                                     | Aquo     | Alle          |
| Quality               | Hoedanigheid                                                                                   | Aquo     | Alle          |
| QualityAssessment     | Kwaliteitsoordeel                                                                              | Aquo     | Alle          |
| Sediment              | Sediment                                                                                       | Aquo     | Biologie      |
| Statistics            | Statistiek                                                                                     | Aquo     | Biologie      |
| StowaWaterType        | Watertype volgens STOWA                                                                        | Aquo     | Alle          |
| TaxonGroup            | Groepindeling van het taxon                                                                    | TWN      | Biologie      |
| TaxonParent           | Bovenliggend niveau van het taxon                                                              | TWN      | Biologie      |
| TaxonSynonym          | Gecorrigeerde naam van het taxon                                                               | TWN      | Biologie      |
| TaxonType             | Hoofdgroep waartoe het taxon behoort                                                           | TWN      | Biologie      |
| ValueProcessingMethod | Waardebewerkingsmethode                                                                        | Aquo     | Alle          |
| ValuationMethod       | Waardebeoordelingsmethode                                                                      | Aquo     | Alle          |
| ValuationTechnique    | Waardebepalingstechniek                                                                        | Aquo     | Alle          |
| Wavelength            | Golflengteklasse                                                                               | Aquo     | Biologie      |
| WidthClassInCm        | Breedteklasse in CM                                                                            | RWS      | Biologie      |
| WidthClassInMm        | Breedteklasse in MM                                                                            | RWS      | Biologie      |

## Referentietypes voor Result

| ReferenceType | Betekenis       | Bron |                | Gebruikt voor | 
|---------------|-----------------|------|----------------|---------------|
| Unit (Uom)    | Gemeten eenheid | Aquo | Result/Measure | Alle          |

## Referentietypes voor FoI (Feature of interest)

| ReferenceType     | Betekenis          | Bron | Soort |
|-------------------|--------------------|------|-------| 
| MeasurementObject | Locatie/meetobject | Aquo | Alle  |

## Referentietypes voor Metadata

| ReferenceType      | Betekenis                                  | Bron     | Gebruikt voor | 
|--------------------|--------------------------------------------|----------|---------------| 
| Project            | Project waarvoor de observatie is verricht | AquaDesk | Alle          |
| Checker            | Gevalideerd door                           | AquaDesk | Alle          |
| Sampler            | Bemonsterd door                            | AquaDesk | Alle          |
| Analyst            | Geanalyseerd door                          | AquaDesk | Alle          |
| Sample             | Monsteraanduiding                          | AquaDesk | Alle          |
| SampleComment      | Opmerking bij monster                      | AquaDesk | Alle          |
| ObservationComment | Opmerking bij meting                       | AquaDesk | Alle          |
| SetComment         | Opmerking bij 'meetsets'                   | AquaDesk | Alle          |

## Versiehistorie Document
| Versie | Datum      | Wijzigingen     |
|--------|------------|-----------------|
| 3.0.0  | 2025-01-13 | Initiële versie |
