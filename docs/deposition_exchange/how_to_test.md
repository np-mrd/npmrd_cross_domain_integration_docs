# Testing Data Exchange

Various test functions are setup on the deposition platform data exchange API swagger UI. Note that to access the swagger UI you must request its url and security key from a developer on the exchange platform.

## TESTING: Getting Submission UUIDs to Test With
A list of submission uuids that can be used for testing can be obtained by querying the endpoint `get_test_ready_submission_uuids`. This will provide uuids for submisisons of various classes that can be used for testing.

## TESTING: Confirming Status of Submission Entries
The deposition status of all compounds in a submission can be checked via the `check_send_status_of_compounds` endpoint. This can be used to confirm if a deposition was successful or not.

### TESTING: Generate Example
These functions all take a uuid value and return an example json for an exchange that was performed without any errors. Functions exist that allow you to generate...

- Exchange JSON (all submission types)
- Exchange Report JSON (all submission types)
- Embargo Update JSON (embargo expired presubmissions and presubmission_matches only)
- Identifier Update JSON (presubmission_matches only)

### TESTING: Send Json to NP-MRD
These functions can be used to trigger a full data exchange API call can be found in the `TESTING: Send Json to NP-MRD` section of the swagger UI (note these are only visible on the dev site).

- Exchange JSON (all submission types)
- Embargo Update JSON (embargo expired presubmissions and presubmission_matches only)
- Identifier Update JSON (presubmission_matches only)