## Purpose

In the deposition platform users are able to complete presubmissions without providing a publication identifier. The deposition platform has tools and pipelines in place to link these identifiers to presubmissions once they have been published. This json used to send doi, pii, and pmid values that have been retroactively ingested in this way to the Database website.

## Usage Flow
This API call is triggered through a cronjob in the deposition backend once every hour. It pushes the json to the database website with the expectation of an immediate ingestion response. The response is expected to contain an updated version of the exchange json which reflects the ingestion status of compounds.

- Source: NP-MRD Deposition Platform
- Target: NP-MRD Database Platform. Is sent to the database site endpoint `[DATABASE_SITE_API_PATH]/external_depositions/update_identifier`
- Response Type: Immediate response contains ingestion status
- Response Format: Modify the provided exchange json to a response version and return it.


## Testing
This json can be generated for any entry in the deposition database at the API endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_identifier_json`.

On the dev server this test json can be generated and automatically pushed to the Target endpoint on the NP-MRD database website via the API call `[DEPOSITION_SITE_EXCHANGE_API]/send_npmrd_exchange_identifier_json_to_npmrd`.

## Example json

### Send json

```
[
  {
    "submission_uuid": "3d4f4c42-96f6-4c26-b411-9f92bf58da96",
    "submission_type": "presubmission_match",
    "submission_doi": "10.999/test.2025.99999",
    "submission_pii": "s009999999999999",
    "submission_pmid": null,
    "doi_ingested": null,
    "pii_ingested": null,
    "pmid_ingested": null,
    "identifier_ingestion_errors": [],
    "compounds": [
      {
        "compound_name": "TEST COMP 2",
        "compound_uuid": "GrJXd6HDqY",
        "compound_smiles": "CC=CCC\\CCCC/CCC=CCCCC/CC=CCC\\CCCC/CCC=CCCCCCC",
        "compound_inchikey": "MNBSRDQSGVNSQD-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "peak_lists": [],
        "nmr_metadata": [
          {
            "spectrum_uuid": "GrJXd6HDqY-zNCYy",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native"
          }
        ]
      },
      {
        "compound_name": "TEST COMP 1",
        "compound_uuid": "YeybvJF6eu",
        "compound_smiles": "CC=CCC\\CCCC/CCC=CCCCC/CC=CCC\\CCCC/CCC=CCCCC",
        "compound_inchikey": "RREZTOXHAUZFQT-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "peak_lists": [],
        "nmr_metadata": [
          {
            "spectrum_uuid": "YeybvJF6eu-ibqUf",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native"
          }
        ]
      }
    ]
  }
]
```

### Response json

```
[
  {
    "submission_uuid": "3d4f4c42-96f6-4c26-b411-9f92bf58da96",
    "submission_type": "presubmission_match",
    "submission_doi": "10.999/test.2025.99999",
    "submission_pii": "s009999999999999",
    "submission_pmid": null,
    "doi_ingested": true,
    "pii_ingested": true,
    "pmid_ingested": null,
    "identifier_ingestion_errors": [
      {
        "type": "identifier_error",
        "message": "TEST ERROR: test identifier_error"
      }
    ],
    "compounds": [
      {
        "compound_name": "TEST COMP 2",
        "compound_uuid": "GrJXd6HDqY",
        "compound_smiles": "CC=CCC\\CCCC/CCC=CCCCC/CC=CCC\\CCCC/CCC=CCCCCCC",
        "compound_inchikey": "MNBSRDQSGVNSQD-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "peak_lists": [],
        "nmr_metadata": [
          {
            "spectrum_uuid": "GrJXd6HDqY-zNCYy",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native"
          }
        ]
      },
      {
        "compound_name": "TEST COMP 1",
        "compound_uuid": "YeybvJF6eu",
        "compound_smiles": "CC=CCC\\CCCC/CCC=CCCCC/CC=CCC\\CCCC/CCC=CCCCC",
        "compound_inchikey": "RREZTOXHAUZFQT-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "peak_lists": [
          {
            "peak_list_uuid": "YeybvJF6eu-zLqXn"
          }
        ],
        "nmr_metadata": [
          {
            "spectrum_uuid": "YeybvJF6eu-ibqUf",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native"
          }
        ]
      }
    ]
  }
]
```


## json Breakdown

The json is exchanged as an **array** of entries containing the following. Each entry in the array exists on a **submission** basis with further nesting to hold individual compound information. Note that the final array may include multiple submissions worth of entries, each as their own (outermost) array entry.

NOTE: Fields tagged with response_field will initially be empty in the JjsonSON data pushed to NP-MRD. These fields are intended to be populated during the ingestion process, so that when the json is returned, it serves as a confirmation back to the deposition system. This approach simplifies the exchange by requiring only a single json document throughout the process.

- [submission_uuid](#submission_uuid)
- [submission_type](#submission_type)
- [submission_doi](#submission_doi)
- [submission_pii](#submission_pii)
- [submission_pmid](#submission_pmid)
- [doi_ingested](#doi_ingested)
- [pii_ingested](#pii_ingested)
- [pmid_ingested](#pmid_ingested)
- [identifier_ingestion_errors](#identifier_ingestion_errors)
- [compounds](#compounds)
  - [compound_name](#compounds_compound_name)
  - [compound_uuid](#compounds_compound_uuid)
  - [compound_smiles](#compounds_compound_smiles)
  - [compound_inchikey](#compounds_compound_inchikey)
  - [npmrd_id](#compounds_npmrd_id)
  - [peak_lists](#compounds_peak_lists)
    - [peak_list_uuid](#compounds_peak_lists_peak_list_uuid)
  - [nmr_metadata](#compounds_nmr_metadata)
    - [spectrum_uuid](#compounds_nmr_metadata_spectrum_uuid)
    - [extracted_experiment_folder](#compounds_nmr_metadata_extracted_experiment_folder)
    - [experiment_type](#compounds_nmr_metadata_experiment_type)
    - [filetype](#compounds_nmr_metadata_filetype)

- submission_uuid <a name="submission_uuid"></a>
  - Description: Internal reference ID for the deposition system. This is the primary key for submission-based data storage. Fixed length 36 character uuid string.
  - Example: `97d6db8a-631d-43bf-8afd-3208f79ec8d3`
  - type: string
  - MaxLength: 36
  - MinLength: 36

- submission_type <a name="submission_type"></a>
  - Description: Type of data submission.
  - One of: `published_article`, `presubmission_article`, or `private_deposition`
    - `published_article` published papers that have dois
    - `presubmission_article` papers under review that do not yet have dois
    - `private_deposition` datasets not associated with academic publications
  - Example: `published_article`
  - type: string
  - MaxLength: 30

- submission_doi <a name="submission_doi"></a>
  - Description: the Digital Object Identifier for the associated publication. Most articles have this. Applies to all data for THIS SUBMISSION.
  - Example: `10.9999/npmr.99999999`
  - type: string or null
  - MaxLength: 1000

- submission_pii <a name="submission_pii"></a>
  - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house.  Applies to all data for THIS SUBMISSION.
  - Example: `s009999999999999`
  - type: string or null
  - MaxLength: 20

- submission_pmid <a name="submission_pmid"></a>
  - Description: The PubMed Central ID number for the associated publications. Some articles have this. Applies to all data for THIS SUBMISSION.
  - Example: `9999999`
  - type: int or null
  - MaxLength: 20

- doi_ingested <a name="doi_ingested"></a>
  - Description: Whether or not the provided `submission_doi` was ingested. It should be false if a doi was not provided OR if it failed to ingest. This value starts as null when generated in the deposition platform.
  - Example: true
  - Type: bool or null
  - One of: true, false, null
  - `response_field`

- pii_ingested <a name="pii_ingested"></a>
  - Description: Whether or not the provided `submission_pii` was ingested. It should be false if a pii was not provided OR if it failed to ingest. This value starts as null when generated in the deposition platform.
  - Example: true
  - Type: bool or null
  - One of: true, false, null
  - `response_field`

- pmid_ingested <a name="pmid_ingested"></a>
  - Description: Whether or not the provided `submission_pmid` was ingested. It should be false if a pmid was not provided OR if it failed to ingest. This value starts as null when generated in the deposition platform.
  - Example: true
  - Type: bool or null
  - One of: true, false, null
  - `response_field`

- identifier_ingestion_errors <a name="identifier_ingestion_errors"></a>
    - Description: A object of the errors relating to issues with ingestion of any identifier information.
    - Example: 
        - If error: 
        ```
        [
            {"format_error": "submission_doi is not a valid string"},
            {"format_error": "submission_pii is not a valid string"}
        ]
        ```
        - If no Error:
        ```
        []
        ```
    - Type: object
    - Error Types:
        - `format_error`: Something is wrong the format of one of the identifiers
    - `response_field`

## compounds <a name="compounds"></a>

- compound_name <a name="compounds_compound_name"></a>
  - Description: Common name for natural product. Can be a common name (as example), or an alpha-numeric code, or sometimes an IUPAC name. If compounds are not named in the paper they should be listed as 'Not named' to differentiate entries that are known to have no name from entries where the name is unknown (which should be Null)
  - Example: `Exampleamide A`
  - type: string
  - MaxLength: 1000

- compound_uuid <a name="compounds_compound_uuid"></a>
  - Description: This uuid value is a short uuid value (10 characters) used as a secondary identifier to identify a specific compound in a deposition entry. This is a new addition added so that we do not need to rely on inchikey or name for this purpose. Required to address issues like atropisomers with the same InChIkey
  - Example: `SD0z84d9Ds`
  - type: string
  - MaxLength: 10
  - MinLength: 10

- compound_smiles <a name="compounds_compound_smiles"></a>
  - Description: The SMILES string of the compound.
  - Example: `OC1=CC=C(C[C@H](N(C([C@H](C)N(C([C@H](NC([C@@H]([C@H](CC)C)NC([C@@H](CC(C)C)N2C)=O)=O)C)=O)C)=O)C)C(N(O)CC(N[C@H](C2=O)C)=O)=O)C=C1`
  - Type: string

- compound_inchikey <a name="compounds_compound_inchikey"></a>
  - Description: Standard structure InChIkey for structure
  - Example: `VQOIHQFCIVFBEC-IQPAJRPASA-N`
  - type: string
  - MinLegnth: 27
  - MaxLength: 27

- npmrd_id <a name="compounds_npmrd_id"></a>
  - Description: The primary key for the NP-MRD database. Format is 'NP' followed by 7 digits. Leading zeros must be included
  - Example: `NP0000001`
  - type: string
  - MinLegnth: 9
  - MaxLength: 9

  ### peak_lists <a name="compounds_peak_lists"></a>
  type: list

    - peak_list_uuid <a name="compounds_peak_lists_peak_list_uuid"></a>
      - Description: uuid value unique to the provided peak list. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
      - Example: `SD0z84d9Ds-D0nP9`
      - type: string
      - MaxLength: 16

  ### nmr_metadata <a name="compounds_nmr_metadata"></a>
  type: list

  - spectrum_uuid <a name="compounds_nmr_metadata_spectrum_uuid"></a>
      - Description: uuid value unique to the provided spectrum. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
      - Example: `SD0z84d9Ds-j3f90`
      - type: string
      - MaxLength: 16
    
  - extracted_experiment_folder <a name="compounds_nmr_metadata_extracted_experiment_folder"></a>
    - Description: Name of extracted data directory for this experiment in the extracted data directory for the compound
    - Example: `13C_1D`
    - type: string
    - MaxLength: 10000

  - experiment_type <a name="compounds_nmr_metadata_experiment_type"></a>
    - Description: Standardized experiment type name. Describes which experiment (1H, COSY etc) is present
    - Example: `1D`
    - type: string
    - MaxLength: 100

  - filetype <a name="compounds_nmr_metadata_filetype"></a>
    - Description: Specifies the format of the NMR data.
    - One of: `Varian_native`, `Bruker_native`, `JEOL_native`, `Jcampdx`, `Mnova`.
    - Example: `Bruker_native`
    - type: string
    - MaxLength: 30