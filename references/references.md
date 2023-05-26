# References

Tne following table shows the standard vocabulary used as key in the 'parameter' dictionary.

| Category              | Dutch name                 | Description                                                               | Source                                            |
|-----------------------|----------------------------|---------------------------------------------------------------------------|---------------------------------------------------|
| parameter             | Parameter                  | Chemical, Physical or Biological parameter                                | Aquo table parameter, excl. grootheid, TaxaInfo   |
| taxongroup (1) (2)    | Taxongroep                 | Biotaxon group                                                            | TaxaInfo                                          |
| taxontype (1) (2)     | Taxontype                  | Biotaxon type                                                             | TaxaInfo                                          |
| quantity              | Grootheid                  | Quantity                                                                  | Aquo table parameter/grootheid                    | 
| capacity              | Hoedanigheid               | Aquo table Hoedanigheid                                                   |
| lifestage (2)         | Levensstadium              | Stage of life                                                             | Aquo table BiologischeKenmerken/levensstadium     |
| lifeform (2)          | Levensvorm                 | Life form                                                                 | Aquo table BiologischeKenmerken/levensvorm        |
| lengthclass (2)       | Lengteklasse               | Classification of length                                                  | Aquo table BiologischeKenmerken/lengteklasse      |
| gender (2)            | Geslacht                   | Gender                                                                    | Aquo table BiologischeKenmerken/geslacht          |
| appearance (2)        | Verschijningsvorm          | Appearance                                                                | Aquo table BiologischeKenmerken/verschijningsvorm |
| behaviour (2)         | Gedrag                     | Behaviour                                                                 | Aquo table BiologischeKenmerken/gedrag            |
| valuationmethod       | Waardebepalingsmethode     | Valuation method                                                          | Aquo table Waardebepalingsmethode                 |
| valuationtechnique    | Waardebepalingstechniek    | Valuation technique                                                       | Aquo table Waardebepalingstechniek                |
| valueprocessingmethod | Waardebewerkingsmethode    | Value processing method                                                   | Aquo table Waardebewerkingsmethode                |
| capacity              | Hoedanigheid               | Capacity                                                                  | Aquo table Hoedanigheid                           |
| samplingdevice        | Bemonsteringsapparaat      | Sampling device                                                           | Aquo table Bemonsteringsapparaat                  |
| samplingmethod        | Bemonsteringsmethode       | Sampling method                                                           | Aquo table Bemonsteringsmethode                   |
| compartment           | Compartiment               | Compartment                                                               | Aquo table Compartiment                           |
| validity              | Juistheidsoordeel          | Validity of the observation                                               | Aquo table Juistheidsoordeel                      |
| validityprocedure     | JuistheidsoordeelProcedure | Procedure for assessment of the validity                                  | Aquo table JuistheidsoordeelProcedure             |
| qualityassessment     | Kwaliteitsoordeel          | Assessment of the quality of the observation result                       | Aquo table Kwaliteitsoordeel                      |
| color                 | Kleur                      | Color                                                                     | Aquo table Kleur                                  |
| odor                  | Geur                       | Odor                                                                      | Aquo table Geur                                   |
| measurementdevice     | Meetapparaat               | Measurement device                                                        | Aquo table Meetapparaat                           |
| samplingtype          | Bemonsteringstype          | Sampling type                                                             | Aquo table Bemonsteringstype                      |
| sampletype            | Monstertype                | Type of sample                                                            | Aquo table Monstertype                            |
| organ                 | Orgaan                     | Sampled organ                                                             | Aquo table Orgaan                                 |
| thing                 |                            | Reserved as a reference to the specific Thing entity for Sensor Things    | Organisation specific                             |
| actuator              |                            | Reserved as a reference to the specific Actuator entity for Sensor Things | Organisation specific                             |
| flowdirection         | stromingsrichting          | Direction of the water flow                                               | Aquo table Stromingsrichting                      |
| condition             | Conditie                   |                                                                           | Aquo table ...                                    |
| habitat               | Habitat                    |                                                                           | Aquo table ...                                    |
| sampler               | Monsternemer               | Organisation that performed the sampling                                  | Aquo table ...                                    |
| analyst               | Analyst                    | Organisation that performed the analysis                                  | Aquo table ...                                    |
| foi                   | Meetobject                 | Feature of Interest: Reference to the location of where sampling was done | Organisation specific                             | Organisation-specific |
| statistic             | Statistische parameter     |                                                                           | Aquo table StatistischeParameter                  | 

(1) Convenience filter for biotaxon
(2) Biotaxon only

Query examples:

parameter/lifeform eq 'LV-CEL'
parameter/quantity eq 'AANTL'
parameter/color eq 'GEEL'


RelatedObservationRollen: meer rollen nodig dan in Aquo zit.




parameter/lifeform eq 'urn:aquo:biologischekenmerken:levensvorm:LV-CEL'