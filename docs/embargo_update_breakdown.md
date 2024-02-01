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
    - [peak_list_embargo_release_ready](#compounds_peak_lists_peak_list_embargo_release_ready)
    - [peak_list_npmrd_db_release_status](#compounds_peak_lists_peak_list_npmrd_db_release_status)
  - [nmr_metadata](#compounds_nmr_metadata)
    - [spectrum_uuid](#compounds_nmr_metadata_spectrum_uuid)
    - [extracted_experiment_folder](#compounds_nmr_metadata_extracted_experiment_folder)
    - [experiment_type](#compounds_nmr_metadata_experiment_type)
    - [filetype](#compounds_nmr_metadata_filetype)
    - [spectrum_embargo_release_ready](#compounds_nmr_metadata_spectrum_embargo_release_ready)
    - [spectrum_npmrd_db_release_status](#compounds_nmr_metadata_spectrum_npmrd_db_release_status)


NOTE: Fields marked with a `response_field` tag indicate that they  will be empty in the initial json that is pushed to NP-MRD but is expected to be updated (as part of the ingestion process) so that when this JSON is returned it can act as confirmation sent back to the deposition system. This is to simplify the json exchange process so that only one JSON is needed.

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
  - Description: The current embargo status of in the NP-MRD Database FOR THIS SUBMISSION. Acts as a general guide for everything in this submission, however, there can be exceptions, i.e. there is an embargo on this submisison but one of the compounds sent already exists in the database.
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
        {
            "submission_uuid": "No data associated with the provided submission_uuid could be found",
            "embargo_status": "Embargo status is not one of the valid options"
        }
        ```
    - Example (no error):
        ```
        {}
        ```
    - Type: object
    - Error Types:
        - `c_values`: Something is wrong with the carbon values.
        - `h_values`: Something is wrong with the hydrogen values.
        - `metadata`: Something is wrong with the peak list metadata.
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

- compound_embargo_release_ready <a name="compounds_compound_embargo_release_ready"></a>
  - Description: Whether or not THIS COMPOUND in THIS SUBMISSION is ready to be released according any embargoes on the submission. If false this compound's NP-Card should be put into an embargoed (not publicly available) state. If true the compound's NP-Card should be made publicly available. If the provided compound already exists in the main database FROM A DIFFERENT SOURCE this bool should be ignored. If it already exists FROM THE SAME SOURCE the value should be overwritten.
  - Example: true
  - type: bool

- compound_npmrd_db_release_status <a name="compounds_compound_npmrd_db_release_status"></a>
  - Description: The current embargo status of in the NP-MRD Database FOR THIS COMPOUND. If the compound already existed (prior to this submission) it should be classified as `released` regardless of the release status of its peaks and for the rest of the submission.
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

    - peak_list_embargo_release_ready <a name="compounds_peak_lists_peak_list_embargo_release_ready"></a>
      - Description: Whether or not THIS PEAK LIST is ready to be released according any embargoes on the submission. If false this peak list should be put into an embargoed (not publicly available) state. If true the peak list should be made publicly available.
      - Example: true
      - type: bool
    
    - peak_list_npmrd_db_release_status <a name="compounds_peak_lists_peak_list_peak_list_npmrd_db_release_status"></a>
      - Description: The current embargo status of in the NP-MRD Database FOR THIS PEAK LIST. Should follow the submission embargo status (unless there is an error).
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
      - Description: Whether or not THIS SPECTRUM is ready to be released according any embargoes on the submission. If false this spectrum should be put into an embargoed (not publicly available) state. If true the spectrum should be made publicly available.
      - Example: true
      - type: bool
    
    - peak_list_npmrd_db_release_status <a name="compounds_nmr_metadata_spectrum_peak_list_npmrd_db_release_status"></a>
      - Description: The current embargo status of in the NP-MRD Database FOR THIS SPECTRUM. Should follow the submission embargo status (unless there is an error).
      - Example: `embargoed`
      - type: string
      - One of: `embargoed`, `released`, `withdrawn`, or ``
      - `response_field`