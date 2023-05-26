# Bulk-processing

Observations can allow bulk-processing: either add or remove observations.
Updates in bulk are not defined, due to performance reasons. It is much faster to remove and add observations that have changed.
This also follows the 'observation is immutable' tenet.

Due to the expected volume of the request, the operations are asynchronous.

Bulk-processing is not easily implemented in REST. I.e. the ```DELETE``` verb does not allow a payload.

By using existing REST verbs a little different from their intended use in REST, however, it is implementable using ```POST``` verbs.

## Bulk management observations

### Phase 1: client requesting

The client can request a bulk-add operation by sending a ```POST```request to the `/observation/bulk` endpoint.
Observations are stored in the body of the ```POST```request in the [bulk-schema format](Schemas/bulk-schema.json).

This payload is regarded as an 'upsert'. All entitites in the payload will first be removed from the storage, if they exist.

Then all entities in the payload will be added to the storage.

The payload of the request differs in some ways from the response retrieved from OData:

- Observations are not nested (expanded)
- A separate part of the body defines the relations between observations
- There are no references present

The server will authenticate the client.
If the client is not authenticated, the server will respond with a 401 Unauthorized response.

The server will next check if the client is authorized to perform bulk-add operations.
If the client is not authorized, the server will respond with a 403 Forbidden response.

Then the server will check the size of the payload.
If the payload is too large, the server will respond with a 413 Payload Too Large response.

If the client passes the checks, the server will respond with a 202 Accepted response with a body consisting of a transaction-id.

The server will log the action and associate it with the transaction-id.

[![bulk-add](https://mermaid.ink/img/pako:eNp1kU9PwzAMxb9K5CurtGk79YA0sQtC_JEYHCAcTOOt0dK4ylKk0fa7k7QdK5vIKe_pZzvxqyFjRZDC1mGZi_VKWhHO8v2zMrsElfoQSXK9rHxO1usMPQ3AyKnHoo28aB7Yj13VLKazy8qe_QtGxU5_jwZ1sv69DSNeLB6d2H5-xp96D8hNTtnuCQ-GUfXs2Kk7IQY1jLi1X2h07D69rOiRo1gzC4NuS81iNv8Pfu26Pd5deYd2j5nXbBMdXgMTKMgVqFWIoo7lEsJSCpKQhquiDVbGS5C2DWj4Nz8fbAapdxVNoCpVWN1KYwixgHSDZh_cEu0b80mT0p7dfR93l3r7A8utr6g?type=png)](https://mermaid.live/edit#pako:eNp1kU9PwzAMxb9K5CurtGk79YA0sQtC_JEYHCAcTOOt0dK4ylKk0fa7k7QdK5vIKe_pZzvxqyFjRZDC1mGZi_VKWhHO8v2zMrsElfoQSXK9rHxO1usMPQ3AyKnHoo28aB7Yj13VLKazy8qe_QtGxU5_jwZ1sv69DSNeLB6d2H5-xp96D8hNTtnuCQ-GUfXs2Kk7IQY1jLi1X2h07D69rOiRo1gzC4NuS81iNv8Pfu26Pd5deYd2j5nXbBMdXgMTKMgVqFWIoo7lEsJSCpKQhquiDVbGS5C2DWj4Nz8fbAapdxVNoCpVWN1KYwixgHSDZh_cEu0b80mT0p7dfR93l3r7A8utr6g)

The client can use the transaction-id to [check the status](#request-status).

### Phase 2: server processing

Observations are stored in the body of the ```POST```request in the [bulk-schema format](Schemas/bulk-schema) format.

Existing observations that are in the payload are not overwritten, but removed and added again.

Observations, whose Id is listed in the remove property, will be removed.

Important: the process is transactional: if one operation fails, processing is reversed.
Partial processing is not allowed.

The following procedure describes a suggested implementation:

- Log the action and associate it with the transaction-id.
- Validate all references of observations and related observations in the payload for *at least* existence.
- Validate all other data in the payload.
- If validation failed, store the response so that the client can retrieve it later and stop processing.
- Determine ids of the observations in the payload, including the ids of the related observations.
- Create a transaction.
- Remove all relations for which the ids are already present in the storage medium.
- Remove all observations for which the ids are already present in the storage medium.
- Add all observations to the storage medium.
- Add all relations to the storage medium.
- Remove all observations whose Id is listed in the ```remove``` property.
- If any of the steps failed, roll back the transaction, log the result of the action and return an error response containing the processing errors.
- Commit the transaction.
- Store a response (HTTP status code + body) and associate it with the transaction-id.

The server will always associate the result of the action with the transaction-id.

## Request status

The endpoint for this operation is /observation/bulk/status.

- A client can request the status of an operation by providing the transaction-id, retrieved from either the [bulk add](#bulk-add-observations) or [bulk-remove](#bulk-remove-observations) operations by means of a GET verb.
- The server will authenticate the client.
- If the client is not authenticated, the server will respond with a 401 Unauthorized response.
- The server will next check if the client is authorized to perform bulk-add operations. If the client is not authorized, the server will respond with a 403 Forbidden response.
- The server will then check if the transaction-id is known.
- If the transaction-id is not known, the server will respond with a 404 Not Found response.
- If the transaction-id is known, the server will return the status of the transaction with transaction-id.

[![bulk-status](https://mermaid.ink/img/pako:eNp9kcFOwzAMhl8l8nmVQOzUA9LEhMQBLh0cIBxM49FobVKlzgGyvjtJW2gHaDnZfz77l-0ApVUEObw7bCux20oj4tu8vPn6kHWM7LtXkWXXG88VGdYlMk3MQgnLpE-8OD5YXqrquL64_Fs5sqdgyqzTnwujIQ0_0WTxaPBbSe2vfvFz7wl5wlqraLFzaDosWVtzp8aif7_CTUXlQZxo83S31ptkuz7TYYALto5UMSxTGlhBQ65BreLaQ6qVEKdvSEIeQ0V79DVLkKaPaBzQFh-mhJydpxX4NnlsNcaDNZDvse6i2qJ5tnbOSenoeT-edrhw_wVR06wc?type=png)](https://mermaid.live/edit#pako:eNp9kcFOwzAMhl8l8nmVQOzUA9LEhMQBLh0cIBxM49FobVKlzgGyvjtJW2gHaDnZfz77l-0ApVUEObw7bCux20oj4tu8vPn6kHWM7LtXkWXXG88VGdYlMk3MQgnLpE-8OD5YXqrquL64_Fs5sqdgyqzTnwujIQ0_0WTxaPBbSe2vfvFz7wl5wlqraLFzaDosWVtzp8aif7_CTUXlQZxo83S31ptkuz7TYYALto5UMSxTGlhBQ65BreLaQ6qVEKdvSEIeQ0V79DVLkKaPaBzQFh-mhJydpxX4NnlsNcaDNZDvse6i2qJ5tnbOSenoeT-edrhw_wVR06wc)

## Request history

The endpoint for this operation is /observation/bulk/history.

To be transparent, all __actions__ need to be logged. The requester can at all times, request the history by means of a GET verb.

TODO: describe request
TODO: describe response
