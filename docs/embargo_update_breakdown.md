## Purpose
When submitting data as a presubmission, users have the option to set an embargo, preventing the data from being viewable by all users of the NP-MRD database. During the embargo period, the data is accessible only to the depositor and authorized NP-MRD administrators. The deposition platform tracks the embargo status, and when the data is ready for release, this json is sent to update its status. Once released, the data becomes viewable in the NP-MRD Database for all users.

## Usage Flow
This API call is triggered through a cronjob in the deposition backend once every hour. It pushes the json to the database website with the expectation of an immediate ingestion response. The response is expected to contain an updated version of the exchange json which reflects the ingestion status of compounds.

- Source: NP-MRD Deposition Platform
- Target: NP-MRD Database Platform. Is sent to the database site endpoint `[DATABASE_SITE_API_PATH]/external_depositions/update_embargo_status`
- Response Type: Immediate response contains ingestion status
- Response Format: Modify the provided exchange json to a response version and return it.

## Testing
This json can be generated for any entry in the deposition database at the API endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_embargo_update_json`.

On the dev server this test json can be generated and automatically pushed to the Target endpoint on the NP-MRD database website via the API call `[DEPOSITION_SITE_EXCHANGE_API]/send_npmrd_exchange_embargo_update_json_to_npmrd`.

## Example json


### Send json

```
[
  {
    "submission_uuid": "5fe927ac-beff-42d8-80c0-38be7b36c7dc",
    "embargo_status": "release_immediately",
    "embargo_date": null,
    "embargo_release_ready": true,
    "embargo_npmrd_db_release_status": "",
    "embargo_npmrd_db_ingestion_successful": null,
    "embargo_errors": [],
    "compounds": [
      {
        "compound_name": "TEST COMP 1 kz2d017",
        "compound_uuid": "WYMTuDRk2Q",
        "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCCCCNCCCCCC",
        "compound_inchikey": "SEJHMTMZTDBHAM-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "compound_embargo_release_ready": true,
        "compound_npmrd_db_release_status": "",
        "peak_lists": [
          {
            "peak_list_uuid": "WYMTuDRk2Q-8qaaX",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": ""
          }
        ],
        "nmr_metadata": [
          {
            "spectrum_uuid": "WYMTuDRk2Q-2A6cg",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native",
            "spectrum_embargo_release_ready": true,
            "spectrum_npmrd_db_release_status": ""
          }
        ]
      },
      {
        "compound_name": "TEST COMP 2 kz2d017",
        "compound_uuid": "Cmip6VtE2G",
        "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCC=CCCNCCCCCC",
        "compound_inchikey": "YRNOKDXIABASKX-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "compound_embargo_release_ready": true,
        "compound_npmrd_db_release_status": "",
        "peak_lists": [
          {
            "peak_list_uuid": "Cmip6VtE2G-3JFgy",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": ""
          },
          {
            "peak_list_uuid": "Cmip6VtE2G-KotG6",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": ""
          }
        ],
        "nmr_metadata": [
          {
            "spectrum_uuid": "Cmip6VtE2G-JQWaq",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native",
            "spectrum_embargo_release_ready": true,
            "spectrum_npmrd_db_release_status": ""
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
    "submission_uuid": "5fe927ac-beff-42d8-80c0-38be7b36c7dc",
    "embargo_status": "release_immediately",
    "embargo_date": null,
    "embargo_release_ready": true,
    "embargo_npmrd_db_release_status": "released",
    "embargo_npmrd_db_ingestion_successful": false,
    "embargo_errors": [
      {
        "type": "embargo_error",
        "message": "TEST ERROR: Embargo release couldn't be processed"
      }
    ],
    "compounds": [
      {
        "compound_name": "TEST COMP 1 kz2d017",
        "compound_uuid": "WYMTuDRk2Q",
        "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCCCCNCCCCCC",
        "compound_inchikey": "SEJHMTMZTDBHAM-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "compound_embargo_release_ready": true,
        "compound_npmrd_db_release_status": "released",
        "peak_lists": [
          {
            "peak_list_uuid": "WYMTuDRk2Q-8qaaX",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": "released"
          }
        ],
        "nmr_metadata": [
          {
            "spectrum_uuid": "WYMTuDRk2Q-2A6cg",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native",
            "spectrum_embargo_release_ready": true,
            "spectrum_npmrd_db_release_status": "released"
          }
        ]
      },
      {
        "compound_name": "TEST COMP 2 kz2d017",
        "compound_uuid": "Cmip6VtE2G",
        "compound_smiles": "C=CCCC=CCCCCCCCCCCCC(=O)CCCCC=CCCNCCCCCC",
        "compound_inchikey": "YRNOKDXIABASKX-UHFFFAOYSA-N",
        "npmrd_id": "NP9999999",
        "compound_embargo_release_ready": true,
        "compound_npmrd_db_release_status": "released",
        "peak_lists": [
          {
            "peak_list_uuid": "Cmip6VtE2G-3JFgy",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": "released"
          },
          {
            "peak_list_uuid": "Cmip6VtE2G-KotG6",
            "peak_list_embargo_release_ready": true,
            "peak_list_npmrd_db_release_status": "released"
          }
        ],
        "nmr_metadata": [
          {
            "spectrum_uuid": "Cmip6VtE2G-JQWaq",
            "extracted_experiment_folder": "1H_1D",
            "experiment_type": "1D",
            "filetype": "Varian_native",
            "spectrum_embargo_release_ready": true,
            "spectrum_npmrd_db_release_status": "released"
          }
        ]
      }
    ]
  }
]
```


## json Breakdown

The json is exchanged as an **array** of entries containing the following. Each entry in the array exists on a **submission** basis with further nesting to hold individual compound information. Note that the final array may include multiple submissions worth of entries, each as their own (outermost) array entry.

NOTE: Fields marked with a `response_field` tag indicate that they  will be empty in the initial json that is pushed to NP-MRD but is expected to be updated (as part of the ingestion process) so that when this JSON is returned it can act as confirmation sent back to the deposition system. This is to simplify the json exchange process so that only one JSON is needed.

## Embargo Update Json

- [submission_uuid](#submission_uuid)
- [embargo_status](#embargo_status)
- [embargo_date](#embargo_date)
- [embargo_release_ready](#embargo_release_ready)
- [embargo_npmrd_db_release_status](#embargo_npmrd_db_release_status)
- [embargo_npmrd_db_ingestion_successful](#embargo_npmrd_db_ingestion_successful)
- [embargo_errors](#embargo_errors)
- [compounds](#compounds)
  - [compound_name](#compounds_compound_name)
  - [compound_uuid](#compounds_compound_uuid)
  - [compound_smiles](#compounds_compound_smiles)
  - [compound_inchikey](#compounds_compound_inchikey)
  - [npmrd_id](#compounds_npmrd_id)
  - [compound_embargo_release_ready](#compounds_compound_embargo_release_ready)
  - [compound_npmrd_db_release_status](#compounds_compound_npmrd_db_release_status)
  - [peak_lists](#compounds_peak_lists)
    - [peak_list_uuid](#compounds_peak_lists_peak_list_uuid)
    - [nuclues](#compounds_peak_lists_nuclues)
    - [peak_list_embargo_release_ready](#compounds_peak_lists_peak_list_embargo_release_ready)
    - [peak_list_npmrd_db_release_status](#compounds_peak_lists_peak_list_npmrd_db_release_status)
  - [nmr_metadata](#compounds_nmr_metadata)
    - [spectrum_uuid](#compounds_nmr_metadata_spectrum_uuid)
    - [extracted_experiment_folder](#compounds_nmr_metadata_extracted_experiment_folder)
    - [experiment_type](#compounds_nmr_metadata_experiment_type)
    - [filetype](#compounds_nmr_metadata_filetype)
    - [spectrum_embargo_release_ready](#compounds_nmr_metadata_spectrum_embargo_release_ready)
    - [spectrum_npmrd_db_release_status](#compounds_nmr_metadata_spectrum_npmrd_db_release_status)
  - [assignment_data](#compounds_assignment_data)
    - [assignment_uuid](#compounds_assignment_data_assignment_uuid)
    - [nuclues](#compounds_assignment_data_nuclues)
    - [assignment_data_embargo_release_ready](#compounds_assignment_data_assignment_data_embargo_release_ready)
    - [assignment_data_npmrd_db_release_status](#compounds_assignment_data_assignment_data_npmrd_db_release_status)


- submission_uuid <a name="submission_uuid"></a>
  - Description: Internal reference ID for the deposition system. This is the primary key for submission-based data storage. Fixed length 36 character uuid string.
  - Example: `97d6db8a-631d-43bf-8afd-3208f79ec8d3`
  - type: string
  - MaxLength: 36
  - MinLength: 36

- embargo_status <a name="submission_embargo_status"></a>
  - Description: User-specified field for embargo status. Allows users to set release condition for their data. 
  - One of: `release_immediately`, `do_not_release`, `embargo_until_date`, or `embargo_until_publication`
    - `publish` indicates that a user wishes to release this data immediately. This is the default value in the deposition system.
    - `embargo_until_date` indicates that the <i>embargo_date</i> field must be checked for the release date to make a submission public. It should be withheld from public access until then.
    - `embargo_until_publication` indicates to withhold the data from public access until the article is confirmed to be published OR a user manually releases the data. We will know an article has been published when a DOI is identified and attached to the specific compound/deposition by the deposition system.
  - Example: `embargo_until_publication`
  - type: string
  - MaxLength: 30

- embargo_date <a name="embargo_date"></a>
  - Description: Embargo date for public release of the data
  - Example: `2023-07-22`
  - type: string
  - MaxLength: 10

- embargo_release_ready <a name="embargo_release_ready"></a>
  - Description: Whether or not ALL COMPOUNDS, PEAKS, AND SPECTRA in THIS SUBMISSION are ready to be released according any embargoes on the submission. If false this compound's NP-Card should be put into an embargoed (not publicly available) state. If true the compound's NP-Card should be made publicly available. If the provided compound already exists in the main database FROM A DIFFERENT SOURCE this bool should be ignored. If it already exists FROM THE SAME SOURCE the value should be overwritten.
  - Example: true
  - type: bool

- embargo_npmrd_db_release_status <a name="embargo_npmrd_db_release_status"></a>
  - Description: The current embargo status of in the NP-MRD Database FOR THIS SUBMISSION. Acts as a general guide for everything in this submission, however, there can be exceptions, i.e. there is an embargo on this submission but one of the compounds sent already exists in the database. Updated by the database site and sent back to the deposition site.
  - Example: `embargoed`
  - type: string
  - One of: `embargoed`, `released`, `withdrawn`, or ``
  - `response_field`

- embargo_npmrd_db_ingestion_successful <a name="embargo_npmrd_db_ingestion_successful"></a>
  - Description: Whether or not the ingestion of the provided embargo information ON A SUBMISSION LEVEL was completed successfully or not.
  - Example: `ingested`
  - Type: array
  - One of: `ingested`, `not_ingested`
  - `response_field`

- embargo_errors <a name="embargo_errors"></a>
    - Description: A field to detail any errors in the embargo ingestion process.
    - Example (error):
        ```
        [
            {"submission_uuid": "No data associated with the provided submission_uuid could be found"},
            {"embargo_status": "Embargo status is not one of the valid options"}
        ]
        ```
    - Example (no error):
        ```
        []
        ```
    - Type: object
    - Error Types:
        - `c_values`: Something is wrong with the carbon values.
        - `h_values`: Something is wrong with the hydrogen values.
        - `metadata`: Something is wrong with the peak list metadata.
    - `response_field`

## compounds <a name="compounds"></a>
type: list

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

- compound_embargo_release_ready <a name="compounds_compound_embargo_release_ready"></a>
  - Description: Whether or not THIS COMPOUND in THIS SUBMISSION is ready to be released according any embargoes on the submission. If false this compound's NP-Card should be put into an embargoed (not publicly available) state. If true the compound's NP-Card should be made publicly available. If the provided compound already exists in the main database FROM A DIFFERENT SOURCE this bool should be ignored. If it already exists FROM THE SAME SOURCE the value should be overwritten. (For now) will never differ from `embargo_release_ready`.
  - Example: true
  - type: bool

- compound_npmrd_db_release_status <a name="compounds_compound_npmrd_db_release_status"></a>
  - Description: The current embargo status of in the NP-MRD Database FOR THIS COMPOUND. If the compound already existed (prior to this submission) it should be classified as `released` regardless of the release status of its peaks and for the rest of the submission. Updated by the database site and sent back to the deposition site.
  - Example: `embargoed`
  - type: string
  - One of: `embargoed`, `released`, `withdrawn`, or ``
  - `response_field`

  ### peak_lists <a name="compounds_peak_lists"></a>
  type: list

    - peak_list_uuid <a name="compounds_peak_lists_peak_list_uuid"></a>
      - Description: uuid value unique to the provided peak list. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
      - Example: `SD0z84d9Ds-D0nP9`
      - type: string
      - MaxLength: 16
    
    - nuclues <a name="compounds_peak_lists_peak_list_nuclues"></a>
      - Description: The nucleus associated with this peak list entry.
      - Example: `H`
      - type: string
      - One of: `H`, or `C`

    - peak_list_embargo_release_ready <a name="compounds_peak_lists_peak_list_embargo_release_ready"></a>
      - Description: Whether or not THIS PEAK LIST is ready to be released according any embargoes on the submission. If false this peak list should be put into an embargoed (not publicly available) state. If true the peak list should be made publicly available. (For now) will never differ from `embargo_release_ready`.
      - Example: true
      - type: bool
    
    - peak_list_npmrd_db_release_status <a name="compounds_peak_lists_peak_list_peak_list_npmrd_db_release_status"></a>
      - Description: The current embargo status of in the NP-MRD Database FOR THIS PEAK LIST. Should follow the submission embargo status (unless there is an error). Updated by the database site and sent back to the deposition site.
      - Example: `embargoed`
      - type: string
      - One of: `embargoed`, `released`, `withdrawn`, or ``
      - `response_field`

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
    
  - spectrum_embargo_release_ready <a name="compounds_nmr_metadata_spectrum_embargo_release_ready"></a>
      - Description: Whether or not THIS SPECTRUM is ready to be released according any embargoes on the submission. If false this spectrum should be put into an embargoed (not publicly available) state. If true the spectrum should be made publicly available. (For now) will never differ from `embargo_release_ready`.
      - Example: true
      - type: bool

  ### assignment_data <a name="compounds_assignment_data"></a>
  type: list

    - assignment_uuid <a name="nmr_data_assignment_data_assignment_uuid"></a>
        - Description: uuid value unique to the provided assignment data. Used as an identifier for this assignment data. 38 characters in length due to including a standard 36 character uuid as well as an additional dash and nucleus character. The first 36 characters are identical to the "sister assignment entry" (between C and H) to make matching the two of them easier.
        - Example: `ff29e8c3-bbcb-4165-9631-a103743dd703-c`
        - type: string
        - MaxLength: 38
    
    - nuclues <a name="compounds_assignment_data_nuclues"></a>
      - Description: The nucleus associated with this peak list entry.
      - Example: `H`
      - type: string
      - One of: `H`, or `C`

    - assignment_data_embargo_release_ready <a name="compounds_peak_lists_peak_list_embargo_release_ready"></a>
      - Description: Whether or not THIS ASSIGNMENT DATA is ready to be released according any embargoes on the submission. If false this data should be put into an embargoed (not publicly available) state. If true this data should be made publicly available. This value should override any previous versions of it that were sent. (For now) should never differ from `embargo_release_ready`.
      - Example: true
      - type: bool
    
    - assignment_data_npmrd_db_release_status <a name="compounds_assignment_data_assignment_data_npmrd_db_release_status"></a>
      - Description: The current embargo status of in the NP-MRD Database FOR THIS ASSIGNMENT DATA. Should follow the submission embargo status (unless there is an error). Updated by the database site and sent back to the deposition site.
      - Example: `embargoed`
      - type: string
      - One of: `embargoed`, `released`, `withdrawn`, or ``
      - `response_field`