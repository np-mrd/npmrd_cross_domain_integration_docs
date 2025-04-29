# Update Compound In Deposition Db JSON

### Updated to Reflect Version: 1.01

## Purpose
Json used to take name, species, genus, or structure updates that has been performed on the database website and send them back to the deposition website. This allows both the database site and the deposition system to keep up to date with one another.

The provided json will take any fields that changed in the database website (from when they were when originally sent from the deposition system) and update them in the deposition system.

As of version 1.01 currently **can only update the following fields**...
- compound_name
- compound_smiles
- compound_inchikey (taken verbatim if provided, else generated from smiles)
- npmrd_id
- genus
- species

## Usage Flow
Database System → Deposition System

The API endpoint that accepts this json should be called whenever a compound with a deposition system compound_uuid is updated in the database site.

- JSON Type: Array of object entries.
- Source: NP-MRD Database System.
- Target: NP-MRD Deposition System.
- Request Type: POST.
- Response Type: Success assumed based on HTTP status code returned.
- Response Format: None.

## API Endpoint
This json should be sent to the endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_update_compound_in_deposition_db_json`.

Response HTTP Status Code:
- Success: 200
- Error: 400+: a number of 400+ response error codes that are thrown if there are errors updating data in the database


## Testing
This json can be generated for any entry in the deposition database at the API endpoint `[DEPOSITION_SITE_EXCHANGE_API]/update_compound_in_deposition_db` as a `POST REQUEST`. The json should be included in the request body. 

## Example Json
```
{
  "submission_uuid": "5fe927ac-beff-42d8-80c0-38be7b36c7dc",
  "compounds": [
    {
      "compound_name": "TEST COMP 1 kz2d017",
      "compound_uuid": "WYMTuDRk2Q",
      "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCCCCNCCCCCC",
      "compound_inchikey": "SEJHMTMZTDBHAM-UHFFFAOYSA-N",
      "npmrd_id": "NP9999999",
      "genus": "TEST GENUS kz2d017",
      "species": "TEST SPECIES kz2d017"
    },
    {
      "compound_name": "TEST COMP 2 kz2d017",
      "compound_uuid": "Cmip6VtE2G",
      "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCC=CCCNCCCCCC",
      "compound_inchikey": "YRNOKDXIABASKX-UHFFFAOYSA-N",
      "npmrd_id": "NP9999999",
      "genus": "TEST GENUS kz2d017",
      "species": "TEST SPECIES kz2d017"
    }
  ]
}
```


## JSON Breakdown
- [json_version](#json_version)
- [submission_uuid](#submission_uuid)
- [compounds](#compounds)
  - [compound_name](#compounds_compound_name)
  - [compound_uuid](#compounds_compound_uuid)
  - [compound_smiles](#compounds_compound_smiles)
  - [compound_inchikey](#compounds_compound_inchikey)
  - [npmrd_id](#compounds_npmrd_id)
  - [genus](#compounds_genus)
  - [species](#compounds_species)

- submission_uuid <a name="submission_uuid"></a>
  - Description: Indicates which entry the fields should be updated for.
  - Example: `97d6db8a-631d-43bf-8afd-3208f79ec8d3`
  - type: string
  - MaxLength: 36
  - MinLength: 36


## compounds <a name="compounds"></a>
- type: list (of dict)
- Example:
    ```
    [
        {
            "submission_uuid": "5fe927ac-beff-42d8-80c0-38be7b36c7dc",
            "compounds": [
                {
                    "compound_name": "TEST COMP 1 kz2d017",
                    "compound_uuid": "WYMTuDRk2Q",
                    "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCCCCNCCCCCC",
                    "compound_inchikey": "SEJHMTMZTDBHAM-UHFFFAOYSA-N",
                    "npmrd_id": "NP9999999",
                    "genus": "TEST GENUS kz2d017",
                    "species": "TEST SPECIES kz2d017"
                },
                {
                    "compound_name": "TEST COMP 2 kz2d017",
                    "compound_uuid": "Cmip6VtE2G",
                    "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCC=CCCNCCCCCC",
                    "compound_inchikey": "YRNOKDXIABASKX-UHFFFAOYSA-N",
                    "npmrd_id": "NP9999999",
                    "genus": "TEST GENUS kz2d017",
                    "species": "TEST SPECIES kz2d017"
                }
            ]
        }
    ]
    ```

- json_version <a name="json_version"></a>
  - Description: The version of the json format. Increments whenever the json format is updated
  - Example: `1.01`
  - type: float

- compound_uuid <a name="compounds_compound_uuid"></a>
  - Description: The compound_uuid of the compound that is being updated. Must match an entry in the Deposition system.
  - Example: `SD0z84d9Ds`
  - type: string
  - MaxLength: 10
  - MinLength: 10

- compound_name <a name="compounds_compound_name"></a>
  - Description: The name of the compound as it now exists in the database system.
  - Example: `Exampleamide A`
  - type: string
  - MaxLength: 1000

- compound_smiles <a name="compounds_compound_smiles"></a>
  - Description: The SMILES string of the compound structure as it now exists in the database system.
  - Example: `OC1=CC=C(C[C@H](N(C([C@H](C)N(C([C@H](NC([C@@H]([C@H](CC)C)NC([C@@H](CC(C)C)N2C)=O)=O)C)=O)C)=O)C)C(N(O)CC(N[C@H](C2=O)C)=O)=O)C=C1`
  - Type: string

- compound_inchikey <a name="compounds_compound_inchikey"></a>
  - Description: The InChIkey of the compound structure as it now exists in the database system.
  - Example: `VQOIHQFCIVFBEC-IQPAJRPASA-N`
  - type: string
  - MinLegnth: 27
  - MaxLength: 27

- npmrd_id <a name="compounds_npmrd_id"></a>
  - Description: The NP-MRD ID as it now exists in the database system.
  - Example: `NP0000001`
  - type: string
  - MinLegnth: 9
  - MaxLength: 9

- genus <a name="compounds_genus"></a>
  - Description: The genus name for the producing organism. Capitalized
  - Example: `Streptomyces`

- species <a name="compounds_species"></a>
  - Description: The species name for the producing organism as it now exists in the database system. Not capitalized
  - Example: `coelicolor`

