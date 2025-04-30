---
layout: default
title: Exchange Report
parent: Deposition Exchange
nav_order: 1
---

# Deposition Report JSON

### Updated to Reflect Version: 1.01

## Purpose
After a deposition exchange json is sent from the NP-MRD deposition system to the NP-MRD database system, ingestion is expected to take anywhere from several minutes to up to an hour. As a result, an immediate response is not expected. Once ingestion is complete, the database system generates this response json and sends it back to the deposition system to provide a status update on the success of the ingestion process.

## Usage Flow
This API call should be triggered every time a deposition json has finished being processed in the NP-MRD Database website (regardless of if the ingestion was successful or not) and immediately sent to the deposition system. All compounds in a deposition json (which exist on a compound-by-compound basis) are expected to be returned in a single json due to how the deposition system tracks exchange status.

- JSON Type: Array of object entries.
- Source: NP-MRD Database Platform
- Target: NP-MRD Deposition Platform. Is sent to the database site endpoint `[DEPOSITION_SITE_EXCHANGE_API]/report_ingestion_status`
- Request Type: POST.
- Response Type: Success assumed based on HTTP status code returned.
- Response Format: None.

## API Endpoint
This json should be sent to the endpoint `[DEPOSITION_SITE_EXCHANGE_API]/report_ingestion_status`.

Response HTTP Status Code:
- Success: 200
- Error: 400+: a number of 400+ response error codes that are thrown if there are errors updating data in the database


## Testing
This json can be generated for any entry in the deposition database at the API endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_deposition_report_json`.

On the dev server ingestion of this json can be tested by running `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_deposition_json`, which will generate a deposition exchange json and send it to the database website which should then return this json as a response.

## Example json

```
[
  {
    "compound_name": "TEST COMP 2",
    "genus": "TEST GENUS",
    "species": "TEST SPECIES",
    "compound_uuid": "GrJXd6HDqY",
    "submission_uuid": "3d4f4c42-96f6-4c26-b411-9f92bf58da96",
    "send_record_id": 0,
    "smiles": "CC=CCCCCCCCCC=CCCCCCC=CCCCCCCCCC=CCCCCCC",
    "inchikey": "MNBSRDQSGVNSQD-UHFFFAOYSA-N",
    "np_card_status": "created",
    "npmrd_id": "NP9999999",
    "compound_npmrd_db_release_status": "released",
    "npmrd_match_status": "new_entry",
    "ingestion_time": "2025-04-28T21:51:59.788736+00:00",
    "compound_errors": [
      {
        "type": "compound_error",
        "message": "TEST ERROR: Compound could not be processed"
      }
    ],
    "nmr_data": {
      "peak_lists": [],
      "experimental_data": [
        {
          "experiment_type": "1D",
          "extracted_experiment_folder": "1H_1D",
          "vendor": "Varian",
          "filetype": "Varian_native",
          "spectrum_ingestion_status": "ingested",
          "spectrum_uuid": "GrJXd6HDqY-zNCYy",
          "spectrum_npmrd_db_release_status": "released",
          "spectrum_errors": [
            {
              "type": "spectrum_error",
              "message": "TEST ERROR: Spectrum could not be processed"
            }
          ]
        }
      ],
      "assignment_data": {}
    }
  },
  {
    "compound_name": "TEST COMP 1",
    "genus": "TEST GENUS",
    "species": "TEST SPECIES",
    "compound_uuid": "YeybvJF6eu",
    "submission_uuid": "3d4f4c42-96f6-4c26-b411-9f92bf58da96",
    "send_record_id": 0,
    "smiles": "CC=CCCCCCCCCC=CCCCCCC=CCCCCCCCCC=CCCCC",
    "inchikey": "RREZTOXHAUZFQT-UHFFFAOYSA-N",
    "np_card_status": "created",
    "npmrd_id": "NP9999999",
    "compound_npmrd_db_release_status": "released",
    "npmrd_match_status": "new_entry",
    "ingestion_time": "2025-04-28T21:51:59.788736+00:00",
    "compound_errors": [],
    "nmr_data": {
      "peak_lists": [
        {
          "nucleus": "C",
          "peak_list_ingestion_status": "ingested",
          "peak_list_uuid": "YeybvJF6eu-zLqXn",
          "peak_list_npmrd_db_release_status": "released",
          "peak_list_errors": [
            {
              "type": "peak_list_error",
              "message": "TEST ERROR: Peak List could not be processed"
            }
          ]
        },
        {
          "nucleus": "H",
          "peak_list_ingestion_status": "ingested",
          "peak_list_uuid": "YeybvJF6eu-zLqXn",
          "peak_list_npmrd_db_release_status": "released",
          "peak_list_errors": []
        }
      ],
      "experimental_data": [
        {
          "experiment_type": "1D",
          "extracted_experiment_folder": "1H_1D",
          "vendor": "Varian",
          "filetype": "Varian_native",
          "spectrum_ingestion_status": "ingested",
          "spectrum_uuid": "YeybvJF6eu-ibqUf",
          "spectrum_npmrd_db_release_status": "released",
          "spectrum_errors": []
        }
      ],
      "assignment_data": {}
    }
  }
]
```


## Deposition Report JSON Fields

The json is exchanged as an **array** of entries containing the following. Each entry in the array exists on a **compound** basis and contains the information relating to the submission.

- [json_version](#json_version)
- [compound_name](#compound_name)
- [compound_uuid](#compound_uuid)
- [submission_uuid](#submission_uuid)
- [smiles](#smiles)
- [inchikey](#inchikey)
- [np_card_status](#np_card_status)
- [npmrd_id](#npmrd_id)
- [compound_npmrd_db_release_status](#compound_npmrd_db_release_status)
- [ingestion_time](#ingestion_time)
- [compound_errors](#compound_errors)
- [nmr_data](#nmr_data)
	- [peak_lists](#nmr_data_peak_lists)
		- [peak_list_ingestion_status](#peak_list_ingestion_status)
        - [peak_list_uuid](#nmr_data_peak_lists_peak_list_uuid)
        - [nucleus](#nmr_data_peak_lists_nucleus)
        - [peak_list_npmrd_db_release_status](#nmr_data_peak_lists_peak_list_npmrd_db_release_status)
        - [peak_list_errors](#nmr_data_peak_list_errors)
	- [experimental_data](#nmr_data_experimental_data)
		- [experiment_type](#nmr_data_experimental_data_experiment_type)
		- [extracted_experiment_folder](#nmr_data_experimental_data_extracted_experiment_folder)
		- [vendor](#nmr_data_experimental_data_vendor)
		- [filetype](#nmr_data_experimental_data_filetype)
		- [spectrum_ingestion_status](#nmr_data_experimental_data_spectrum_ingestion_status)
        - [spectrum_uuid](#nmr_data_experimental_data_spectrum_uuid)
        - [spectrum_npmrd_db_release_status](#nmr_data_experimental_data_nmr_metadata_spectrum_npmrd_db_release_status)
        - [spectrum_errors](#nmr_data_experimental_data_spectrum_errors)
    - [assignment_data](#nmr_data_assignment_data)
        - [assignment_data_ingestion_status](#nmr_data_assignment_data_ingestion_status)
        - [assignment_uuid](#nmr_data_assignment_data_assignment_uuid)
        - [nucleus](#nmr_data_assignment_data_nucleus)
        - [assignment_data_npmrd_db_release_status](#nmr_data_assignment_data_assignment_data_npmrd_db_release_status)

## json_version <a name="json_version"></a>
  - Description: The version of the json format. Increments whenever the json format is updated
  - Example: `1.01`
  - type: float

## compound_name <a name="compound_name"></a>
- Description: The name of the compound.
- Example: `Talarolide A`
- Type: string
- Can Overwrite Deposition Database: `Yes`

## compound_uuid <a name="compound_uuid"></a>
- Description: The compound-specific UUID generated by the deposition system and included in the exchange JSON.
- Example: `pldss5do2g`
- Type: string
- Can Overwrite Deposition Database: `No`

## submission_uuid <a name="submission_uuid"></a>
- Description: The UUID tied to the submission that the compound came from.
- Example: `DWXDJPQNIPFVLN-BIUSABGFSA-N`
- Type: string
- Can Overwrite Deposition Database: `No`

## smiles <a name="smiles"></a>
- Description: The SMILES string of the compound.
- Example: `OC1=CC=C(C[C@H](N(C([C@H](C)N(C([C@H](NC([C@@H]([C@H](CC)C)NC([C@@H](CC(C)C)N2C)=O)=O)C)=O)C)=O)C)C(N(O)CC(N[C@H](C2=O)C)=O)=O)C=C1`
- Type: string
- Can Overwrite Deposition Database: Currently `No`. Planned to be yes following deposition system backend rework

## inchikey <a name="inchikey"></a>
- Description: The InChIKey of the compound.
- Example: `DWXDJPQNIPFVLN-BIUSABGFSA-N`
- Type: string
- Can Overwrite Deposition Database: `No` (smiles takes priority)

## np_card_status <a name="np_card_status"></a>
- Description: Whether or not the NP Card was succesfully created in the NP-MRD Database, or, indication that the card already existed.
- Example: `created`
- Type: string
- One of: `created`, `not_created`, `already_existed`
- Can Overwrite Deposition Database: `No` (smiles takes priority)

## npmrd_id <a name="npmrd_id"></a>
- Description: The NP-CARD ID that was assigned to the compound.
- Example: `NP9999999`
- Type: string
- Can Overwrite Deposition Database: `Yes`

## compound_npmrd_db_release_status <a name="compound_npmrd_db_release_status"></a>
  - Description: Reports whether or not the provided compound has been released. If the provided data is still embargoed then it will be indicated as such here. `embargoed` means the data is hidden from the public under an embargo, `released` means the data is publically available, and `withdrawn` indicates that the data was once public but has been re-embargoed
  - Example: `embargoed`
  - type: string
  - One of: `embargoed`, `released`, or `withdrawn`
  - Can Overwrite Deposition Database: `Yes`

## ingestion_time <a name="ingestion_time"></a>
- Description: The date and time that the compound ingestion was complete.
- Example: `2023-11-12T18:08:37.675040+00:00`
- Type: string (datetime format)
- Can Overwrite Deposition Database: `Yes`

## compound_errors <a name="compound_errors"></a>
- Description: A object of the errors relating the compound information that were present in the data being sent. This is NOT for errors relating to peak lists or spectra but for things like invalid SMILES or missing required fields.
- Example: 
    - If error: 
    ```
    {
        "structure_error": "invalid smiles",
        "name_error": "compound_name contains invalid characters",
        "required_field_missing": "submission_uuid is empty"
    }
    ```
    - If no Error:
    ```
    {}
    ```
- Type: object
- Error Types:
    - `structure_error`: Something is wrong with the SMILES or InChIKey.
    - `name_error`: Something is wrong with the compound name.
    - `required_field_missing`: A necessary field (i.e. SMILES) was empty in the JSON.
- Can Overwrite Deposition Database: `Yes`

## nmr_data <a name="nmr_data"></a>
- Description: Information related to NMR data for the compound (peak lists and experimental data).
- Type: object
  - peak_lists <a name="nmr_data_peak_lists"></a>
    - Type: array (of objects)
    - Description: Status reporting for peak lists. Array in case there are multiple peak lists for a single compound. Can be left as a blank array if no peaks were sent.
        - peak_list_ingestion_status <a name="peak_list_ingestion_status"></a>
            - Description: Whether or not the ingestion of the provided peak lists was completed successfully or not.
            - Example: `ingested`
            - Type: array
            - One of: `ingested`, `not_ingested`
            - Can Overwrite Deposition Database: `Yes`
        - peak_list_uuid <a name="nmr_data_peak_lists_peak_list_uuid"></a>
          - Description: uuid value unique to the provided peak list. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
          - Example: `SD0z84d9Ds-D0nP9`
          - type: string
          - MaxLength: 16
          - Can Overwrite Deposition Database: `No`
        - nucleus <a name="nmr_data_peak_lists_nucleus"></a>
          - Description: The nucleus associated with this peak list entry.
          - Example: `H`
          - type: string
          - One of: `H`, or `C`
          - Can Overwrite Deposition Database: `No`
        - peak_list_npmrd_db_release_status <a name="nmr_data_peak_lists_peak_list_npmrd_db_release_status"></a>
          - Description: Indicates whether or not this specific peak list has been `embargoed`, `released`, or `withdrawn`. Included to be able to track indiviudal peak lists.
          - Example: `embargoed`
          - type: string
          - One of: `embargoed`, `released`, or `withdrawn`
        - peak_list_errors <a name="nmr_data_peak_list_errors"></a>
            - Example (error):
                ```
                {
                    "c_values": "Too many C values were provided for the given SMILES",
                    "h_values": "invalid character `J` provided in h_values string."
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
            - Can Overwrite Deposition Database: `Yes`
  - experimental_data <a name="nmr_data_experimental_data"></a>
    - Description: Status report of experimental NMR data (NMR Spectra). Each entry represents a different experiment/spectra. Can be left as a blank array if no experimental data was sent.
    - Type: array (of objects)
      - experiment_type <a name="nmr_data_experimental_data_experiment_type"></a>
        - Description: Type of NMR experiment performed.
        - Example: `TOCSY`
        - Type: string
        - Can Overwrite Deposition Database: `No`
      - extracted_experiment_folder <a name="nmr_data_experimental_data_experiment_folder"></a>
        - Description: Name of the folder where the extracted NMR experiment data was stored in.
        - Example: `1H_1H_TOCSY`
        - Type: string
        - Can Overwrite Deposition Database: `No`
      - original_data_path <a name="nmr_data_experimental_data_original_data_path"></a>
        - Description: Path through the original directory to the main experimental file.
        - Example: `TU011_ISP4_KS_FR14_SEMI_07_CRYO/7/acqu2`
        - Type: string
        - Can Overwrite Deposition Database: `No`
      - vendor <a name="nmr_data_experimental_data_vendor"></a>
        - Description: Vendor of the NMR experiment file
        - Example: `Bruker`
        - Type: string
        - Can Overwrite Deposition Database: `No`
      - filetype <a name="nmr_data_experimental_data_filetype"></a>
        - Description: Filetype of the NMR data.
        - Example: `Bruker_native`
        - Type: string
        - Can Overwrite Deposition Database: `No`
      - spectrum_ingestion_status <a name="nmr_data_experimental_data_spectrum_ingestion_status"></a>
        - Description: Status of the spectrum ingestion
        - Example: `ingested`
        - Type: string
        - One of: `ingested`, `not_ingested`
        - Can Overwrite Deposition Database: `Yes`
      - spectrum_uuid <a name="nmr_data_experimental_data_nmr_metadata_spectrum_uuid"></a>
        - Description: uuid value unique to the provided spectrum. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
        - Example: `pldss5do2g-j3f90`
        - type: string
        - MaxLength: 16
        - Can Overwrite Deposition Database: `No`
      - spectrum_npmrd_db_release_status <a name="nmr_data_experimental_data_nmr_metadata_spectrum_npmrd_db_release_status"></a>
          - Description: Indicates whether or not this specific peak list has been `embargoed`, `released`, or `withdrawn`. Included to be able to track indiviudal spectra.
          - Example: `embargoed`
          - type: string
          - One of: `embargoed`, `released`, or `withdrawn`
          - Can Overwrite Deposition Database: `Yes`
      - spectrum_errors <a name="nmr_data_experimental_data_spectrum_errors"></a>
        - Description: Errors related to the NMR spectrum.
        - Type: array
        - Example (error):
            ```
            {
                "download_error": "nmr_data_download_link could not be downloaded.",
                "filetype_error": "File extension `.mnova` currently cannot be housed in the NP-MRD Database.",
                "nmr_error": "nmr file `acqu` is corrupt and cannot be opened." 
            }
            ```
        - Example (no error):
            ```
            {}
            ```
        - Can Overwrite Deposition Database: `Yes`
  - assignment_data <a name="nmr_data_assignment_data"></a>
    - Type: array (of objects)
    - Description: Status reporting for assignment data. Array in case there are multiple assignment data lists were submitted for a single compound. Can be left as a blank array if no assignment data was included.
        - assignment_data_ingestion_status <a name="nmr_data_assignment_data_ingestion_status"></a>
            - Description: Whether or not the ingestion of the provided assignmetn data was completed successfully or not.
            - Example: `ingested`
            - Type: array
            - One of: `ingested`, `not_ingested`
            - Can Overwrite Deposition Database: `Yes`
        - assignment_uuid <a name="nmr_data_assignment_data_assignment_uuid"></a>
            - Description: uuid value unique to the provided assignment data. Used as an identifier for this assignment data. 38 characters in length due to including a standard 36 character uuid as well as an additional dash and nucleus character. The first 36 characters are identical to the "sister assignment entry" (between C and H) to make matching the two of them easier.
            - Example: `ff29e8c3-bbcb-4165-9631-a103743dd703-c`
            - type: string
            - MaxLength: 38
            - Can Overwrite Deposition Database: `No`
        - nucleus <a name="nmr_data_assignment_data_nucleus"></a>
          - Description: The nucleus associated with this assignment data entry.
          - Example: `H`
          - type: string
          - One of: `H`, or `C`
          - Can Overwrite Deposition Database: `No`
        - assignment_data_npmrd_db_release_status <a name="nmr_data_assignment_data_assignment_data_npmrd_db_release_status"></a>
          - Description: Indicates whether or not this specific assignment data entry has been `embargoed`, `released`, or `withdrawn`. Included to be able to track individual assignment data entries.
          - Example: `embargoed`
          - type: string
          - One of: `embargoed`, `released`, or `withdrawn`
        - assignment_data_errors <a name="nmr_data_assignment_data_errors"></a>
            - Example (error):
                ```
                {
                    "shift": "Too many C values were provided for the given SMILES",
                    "coupling": "invalid value 2.94 provided in coupling list field."
                }
                ```
            - Example (no error):
                ```
                {}
                ```
            - Type: object
            - Can Overwrite Deposition Database: `Yes`