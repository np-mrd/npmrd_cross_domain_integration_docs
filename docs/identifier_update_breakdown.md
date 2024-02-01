## Identifier Update Json

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
  - [compound_submission_doi](#compound_submission_doi)
  - [compound_submission_pii](#compound_submission_pii)
  - [compound_submission_pmid](#compound_submission_pmid)
  - [peak_lists](#compounds_peak_lists)
    - [peak_list_uuid](#compounds_peak_lists_peak_list_uuid)
    - [peak_list_submission_doi](#compounds_peak_lists_peak_list_submission_doi)
    - [peak_list_submission_pii](#compounds_peak_lists_peak_list_submission_pii)
    - [peak_list_submission_pmid](#compounds_peak_lists_peak_list_submission_pmid)
  - [nmr_metadata](#compounds_nmr_metadata)
    - [spectrum_uuid](#compounds_nmr_metadata_spectrum_uuid)
    - [extracted_experiment_folder](#compounds_nmr_metadata_extracted_experiment_folder)
    - [experiment_type](#compounds_nmr_metadata_experiment_type)
    - [filetype](#compounds_nmr_metadata_filetype)
    - [spectrum_submission_doi](#compounds_nmr_metadata_spectrum_submission_doi)
    - [spectrum_submission_pii](#compounds_nmr_metadata_spectrum_submission_pii)
    - [spectrum_submission_pmid](#compounds_nmr_metadata_spectrum_submission_pmid)

NOTE: Fields marked with a `response_field` tag indicate that they  will be empty in the initial json that is pushed to NP-MRD but is expected to be updated (as part of the ingestion process) so that when this JSON is returned it can act as confirmation sent back to the deposition system. This is to simplify the json exchange process so that only one JSON is needed.

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
  - type: string or None
  - MaxLength: 1000

- submission_pii <a name="submission_pii"></a>
  - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house.  Applies to all data for THIS SUBMISSION.
  - Example: `null`
  - type: string or None
  - MaxLength: 20

- submission_pmid <a name="submission_pmid"></a>
  - Description: The PubMed Central ID number for the associated publications. Some articles have this. Applies to all data for THIS SUBMISSION.
  - Example: `32856641`
  - type: int or None
  - MaxLength: 20

- doi_ingested <a name="doi_ingested"></a>
  - Description: Whether or not the provided `submission_doi` was ingested. If the value is null (there is no doi to ingest) then this value should be null. It should only be `not_ingested` if the value was not ingested for some reason.
  - Example: `ingested`
  - Type: array
  - One of: `ingested`, `not_ingested`, or null
  - `response_field`

- doi_ingested <a name="doi_ingested"></a>
  - Description: Whether or not the provided `submission_pii` was ingested. If the value is null (there is no pii to ingest) then this value should be null. It should only be `not_ingested` if the value was not ingested for some reason.
  - Example: `ingested`
  - Type: array
  - One of: `ingested`, `not_ingested`, or null
  - `response_field`

- pmid_ingested <a name="doi_ingested"></a>
  - Description: Whether or not the provided `submission_pmid` was ingested. If the value is null (there is no pmid to ingest) then this value should be null. It should only be `not_ingested` if the value was not ingested for some reason.
  - Example: `ingested`
  - Type: array
  - One of: `ingested`, `not_ingested`, or null
  - `response_field`

- identifier_ingestion_errors <a name="identifier_ingestion_errors"></a>
    - Description: A object of the errors relating to issues with ingestion of any identifier information.
    - Example: 
        - If error: 
        ```
        {
            "format_error": "submission_doi is not a valid string or null",
        }
        ```
        - If no Error:
        ```
        {}
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

- compound_submission_doi <a name="compounds_compound_submission_doi"></a>
  - Description: the Digital Object Identifier for the associated publication. Most articles have this. Applies to all data for THIS COMPOUND. (For now) will never differ from `submission_doi`.
  - Example: `10.9999/npmr.99999999`
  - type: string or None
  - MaxLength: 1000

- compound_submission_pii <a name="compounds_compound_submission_pii"></a>
  - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house.  Applies to all data for THIS COMPOUND. (For now) will never differ from `submission_pii`.
  - Example: `null`
  - type: string or None
  - MaxLength: 20

- compound_submission_pii <a name="compounds_compound_submission_pii"></a>
  - Description: The PubMed Central ID number for the associated publications. Some articles have this. Applies to all data for THIS COMPOUND. (For now) will never differ from `submission_pmid`.
  - Example: `32856641`
  - type: int or None
  - MaxLength: 20

  ### peak_lists <a name="compounds_peak_lists"></a>
  type: list

    - peak_list_uuid <a name="compounds_peak_lists_peak_list_uuid"></a>
      - Description: uuid value unique to the provided peak list. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
      - Example: `SD0z84d9Ds-D0nP9`
      - type: string
      - MaxLength: 16

    - peak_list_submission_doi <a name="compounds_peak_lists_peak_list_submission_doi"></a>
        - Description: the Digital Object Identifier for the associated publication. Most articles have this. Applies to all data for THIS PEAK LIST. (For now) will never differ from `submission_doi`.
        - Example: `10.9999/npmr.99999999`
        - type: string or None
        - MaxLength: 1000

    - peak_list_submission_pii <a name="compounds_peak_lists_peak_list_submission_pii"></a>
        - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house.  Applies to all data for THIS PEAK LIST. (For now) will never differ from `submission_pii`.
        - Example: `null`
        - type: string or None
        - MaxLength: 20

    - peak_list_submission_pmid <a name="compounds_peak_lists_peak_list_submission_pmid"></a>
        - Description: The PubMed Central ID number for the associated publications. Some articles have this. Applies to all data for THIS PEAK LIST. (For now) will never differ from `submission_pmid`.
        - Example: `32856641`
        - type: int or None
        - MaxLength: 20

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

  - spectrum_submission_doi <a name="compounds_nmr_metadata_spectrum_submission_doi"></a>
      - Description: the Digital Object Identifier for the associated publication. Most articles have this. Applies to all data for THIS SPECTRUM. (For now) will never differ from `submission_doi`.
      - Example: `10.9999/npmr.99999999`
      - type: string or None
      - MaxLength: 1000

  - spectrum_submission_pii <a name="compounds_nmr_metadata_spectrum_spectrum_submission_pii"></a>
      - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house.  Applies to all data for THIS SPECTRUM. (For now) will never differ from `submission_pii`.
      - Example: `null`
      - type: string or None
      - MaxLength: 20

  - spectrum_submission_pmid <a name="compounds_nmr_metadata_spectrum_spectrum_submission_pmid"></a>
      - Description: The PubMed Central ID number for the associated publications. Some articles have this. Applies to all data for THIS SPECTRUM. (For now) will never differ from `submission_pmid`.
      - Example: `32856641`
      - type: int or None
      - MaxLength: 20