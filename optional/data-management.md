# Data Management

Data management allows adding, modifying or deleting of data, either by individual updates (one entity at the time), or bulk updates (multiple additions or removals).
Data management will **not** allow batch-processing. In batch-processing several different kinds of actions are performed.

Notes:

- Observations are considered immutable. Incorrect observations will have to be removed and re-added.
- References are *not* considered immutable. For instance: the name of a taxon may change, but the taxon itself remains the same.
- Removal of a reference is *only allowed* if no observation in the system uses the reference.

Management of individual entities is described [here](data-management-individual.md).
Bulk-processing is described [here](data-management-bulk.md)

Data management is *optional*. It is also possible to only implement either bulk- or non-bulk operations.

For all operations, security in the form of OAUTH2 Authorization Code flow is required.
