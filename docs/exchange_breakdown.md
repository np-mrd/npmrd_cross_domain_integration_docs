# Data Exchange JSON

### Updated to Reflect Version: 2.0.6

## Purpose
Json used to send major data from one system to another in the NP-MRD Stack. Can contain all forms of data that the system recognizes including system identifiers (npmrd_id, deposition_uuid, deposition compound_uuid, etc.), NMR Data, Peak Lists, Assignment Data, etc.

Every entry represents a single compound with origin information (i.e. submission information) being represented as fields within. All data associated with a compound (nmr, peaks, assignment) are contained in its entry.

A repo that contains this json schema can be found here: [np-mrd/npmrd_data_exchange](https://github.com/np-mrd/npmrd_data_exchange).

## Usage Flow
### Database System → Deposition System

A cronjob in the database system is setup to push any newly deposited data every hour. Ready to send submissions will be added to the send json until there are 60 or more nmr spectra included, which will then prevent any new entries from being added. This is to give the database system time to ingest provided spectra and avoid overloading it.

Due to an expected ingestion time of several minutes to upwards of an hour a separate API call should be made from the database system to the deposition should be made to report the ingestion status to the database system once ingestion is complete using the Deposition Ingestion Report JSON.

- JSON Type: Array of object entries.
- Source: NP-MRD Database System.
- Target: NP-MRD Deposition System.
- Request Type: POST.
- Response Type: Respond via separate API call to endpoint deposition system after fully ingesting data: 
`[DEPOSITION_SITE_EXCHANGE_API]/report_ingestion_status`.
- Response Format: [Deposition Ingestion Report JSON](../index.md)

### DFT System → Deposition System

[FILL_ME]

- JSON Type: Array of object entries.
- Source: NP-MRD DFT System
- Target: NP-MRD Deposition System
- Response Type: [FILL_ME]
- Response Format: [FILL_ME]

## API Endpoint
### Database System → Deposition System
This json should be sent to the endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_update_compound_in_deposition_db_json`.

Response HTTP Status Code:
- Success: 200
- Error: 400+: a number of 400+ response error codes that are thrown if there are errors updating data in the database

## Testing
### Database System
This json can be generated for any entry in the deposition database at the API endpoint `[DEPOSITION_SITE_EXCHANGE_API]/generate_npmrd_exchange_deposition_json` as a `POST REQUEST`. 

On the dev server ingestion of this json can be tested by running `[DEPOSITION_SITE_EXCHANGE_API]/send_npmrd_exchange_deposition_json_to_npmrd`, which will generate a this json and send it to the database website.

### Example JSON
### Database System → Deposition System
```
[
  {
    "compound_name": "TEST COMP 1",
    "smiles": "CC=CCCCCCCCC=CCCCCCCCC",
    "inchikey": "MXARNSWMBYQYOP-UHFFFAOYSA-N",
    "npmrd_id": "NP9999999",
    "submission": {
      "source": "deposition_system",
      "type": "presubmission_article",
      "submission_uuid": "3d1568f7-064a-4118-834e-8c6e5019e46d",
      "compound_uuid": "fAWvfyRdQA",
      "submission_date": "2025-04-03T00:33:00.000000+00:00",
      "send_record_id": 0,
      "embargo_status": "embargo_until_date",
      "embargo_date": "2025-06-30T00:00:00.000000+00:00",
      "compound_embargo_release_ready": false
    },
    "citation": {
      "doi": null,
      "pmid": null,
      "null": null
    },
    "origin": {
      "species": "TEST SPECIES",
      "genus": "TEST GENUS",
      "private_collection": {
        "compound_source_type": null,
        "purified_in_house": {
          "biological_material_source": null
        },
        "commercial": {
          "supplier": null,
          "cas_number": null,
          "catalogue_number": null
        },
        "compound_library": {
          "library_name": null,
          "library_description": null,
          "library_compound_code": null
        },
        "other": {
          "user_specified_compound_source": null,
          "biological_material_source": null
        }
      }
    },
    "depositor_info": {
      "email": "mpin@sfu.ca",
      "account_id": 526,
      "attribution_name": "Matthew Pin",
      "attribution_organization": "Simon Fraser University",
      "show_email_in_attribution": true,
      "show_name_in_attribution": true,
      "show_organization_in_attribution": true
    },
    "nmr_data": {
      "peak_lists": [
        {
          "nucleus": "C",
          "solvent": "CDCl3",
          "reference": "DSS",
          "values": [
            200,
            200
          ],
          "frequency": 300,
          "frequency_units": "MHz",
          "temperature": 300,
          "temperature_units": "K",
          "peak_list_uuid": "fAWvfyRdQA-LTPYF",
          "peak_list_embargo_release_ready": false
        },
        {
          "nucleus": "H",
          "solvent": "CDCl3",
          "reference": "DSS",
          "values": [
            10,
            10,
            10
          ],
          "frequency": 300,
          "frequency_units": "MHz",
          "temperature": 300,
          "temperature_units": "K",
          "peak_list_uuid": "fAWvfyRdQA-LTPYF",
          "peak_list_embargo_release_ready": false
        }
      ],
      "experimental_data": {
        "nmr_data_download_link": "DOWNLOAD_LINK",
        "nmr_metadata": [
          {
            "vendor": "Varian",
            "filetype": "Varian_native",
            "solvent": "CDCl3",
            "frequency": [
              500.111410129
            ],
            "frequency_units": "MHz",
            "f1_nucleus": "1H",
            "f2_nucleus": null,
            "temperature": 298,
            "temperature_units": "K",
            "experiment_type": "1D",
            "extracted_experiment_folder": "1H_1D",
            "extracted_data_path": "1H_1D/procpar",
            "spectrum_uuid": "fAWvfyRdQA-FzXP5",
            "spectrum_embargo_release_ready": false
          }
        ]
      },
      "assignment_data": {},
      "predicted_data": {
        "prediction_method": null,
        "dft_protocol": {
          "molecular_dynamics": {
            "md_software": null,
            "md_software_version": null,
            "forcefield": null,
            "energy_window": null,
            "downselection": null
          },
          "quantum_mechanics": {
            "qm_software": null,
            "qm_software_version": null,
            "tasks": [],
            "functionals": [],
            "basis_sets": [],
            "solvent": null,
            "conversion_factors": {
              "H": {
                "m": null,
                "b": null
              },
              "C": {
                "m": null,
                "b": null
              }
            }
          }
        },
        "ml_protocol": {
          "ml_model": null,
          "training_set": null,
          "training_parameters": {
            "parameter_1": null,
            "parameter_2": null
          },
          "chemical_shifts": {
            "solvent": null,
            "temperature": null,
            "temperature_units": null,
            "h_nmr": {
              "shift": null,
              "shielding_tensor": null,
              "rdkit_index": null
            },
            "c_nmr": {
              "shift": null,
              "shielding_tensor": null,
              "rdkit_index": null
            }
          }
        }
      }
    }
  },
  {
    "compound_name": "TEST COMP 2",
    "smiles": "CC=CC\\CCCC/CCC=CCCC/CCCCC=CC=CC\\CCCC/CCC=CCCC/CCCCC",
    "inchikey": "NFCIFLYOUDQACS-UHFFFAOYSA-N",
    "npmrd_id": "NP9999999",
    "submission": {
      "source": "deposition_system",
      "type": "presubmission_article",
      "submission_uuid": "3d1568f7-064a-4118-834e-8c6e5019e46d",
      "compound_uuid": "5uVEMDdU6k",
      "submission_date": "2025-04-03T00:33:00.000000+00:00",
      "send_record_id": 0,
      "embargo_status": "embargo_until_date",
      "embargo_date": "2025-06-30T00:00:00.000000+00:00",
      "compound_embargo_release_ready": false
    },
    "citation": {
      "doi": null,
      "pmid": null,
      "null": null
    },
    "origin": {
      "species": "TEST SPECIES",
      "genus": "TEST GENUS",
      "private_collection": {
        "compound_source_type": null,
        "purified_in_house": {
          "biological_material_source": null
        },
        "commercial": {
          "supplier": null,
          "cas_number": null,
          "catalogue_number": null
        },
        "compound_library": {
          "library_name": null,
          "library_description": null,
          "library_compound_code": null
        },
        "other": {
          "user_specified_compound_source": null,
          "biological_material_source": null
        }
      }
    },
    "depositor_info": {
      "email": "mpin@sfu.ca",
      "account_id": 526,
      "attribution_name": "Matthew Pin",
      "attribution_organization": "Simon Fraser University",
      "show_email_in_attribution": true,
      "show_name_in_attribution": true,
      "show_organization_in_attribution": true
    },
    "nmr_data": {
      "peak_lists": [],
      "experimental_data": {
        "nmr_data_download_link": "DOWNLOAD_LINK",
        "nmr_metadata": [
          {
            "vendor": "Varian",
            "filetype": "Varian_native",
            "solvent": "CDCl3",
            "frequency": [
              500.111410129
            ],
            "frequency_units": "MHz",
            "f1_nucleus": "1H",
            "f2_nucleus": null,
            "temperature": 298,
            "temperature_units": "K",
            "experiment_type": "1D",
            "extracted_experiment_folder": "1H_1D",
            "extracted_data_path": "1H_1D/procpar",
            "spectrum_uuid": "5uVEMDdU6k-9ts3u",
            "spectrum_embargo_release_ready": false
          }
        ]
      },
      "assignment_data": {},
      "predicted_data": {
        "prediction_method": null,
        "dft_protocol": {
          "molecular_dynamics": {
            "md_software": null,
            "md_software_version": null,
            "forcefield": null,
            "energy_window": null,
            "downselection": null
          },
          "quantum_mechanics": {
            "qm_software": null,
            "qm_software_version": null,
            "tasks": [],
            "functionals": [],
            "basis_sets": [],
            "solvent": null,
            "conversion_factors": {
              "H": {
                "m": null,
                "b": null
              },
              "C": {
                "m": null,
                "b": null
              }
            }
          }
        },
        "ml_protocol": {
          "ml_model": null,
          "training_set": null,
          "training_parameters": {
            "parameter_1": null,
            "parameter_2": null
          },
          "chemical_shifts": {
            "solvent": null,
            "temperature": null,
            "temperature_units": null,
            "h_nmr": {
              "shift": null,
              "shielding_tensor": null,
              "rdkit_index": null
            },
            "c_nmr": {
              "shift": null,
              "shielding_tensor": null,
              "rdkit_index": null
            }
          }
        }
      }
    }
  }
]
```


## JSON Fields
- [compound_name](#compound_name)
- [smiles](#smiles)
- [inchikey](#inchikey)
- [npmrd_id](#npmrd_id)
- [submission](#submission)
	- [source](#submission_source)
	- [type](#submission_type)
	- [submission_uuid](#submission_uuid)
	- [compound_uuid](#submission_compound_uuid)
	- [submission_date](#submission_submission_date)
	- [embargo_status](#submission_embargo_status)
	- [embargo_date](#submission_embargo_date)
  - [compound_embargo_release_ready](#submission_compound_embargo_release_ready)
- [citation](#citation)
	- [doi](#citation_doi)
	- [pmid](#citation_pmid)
	- [pii](#citation_pii)
- [origin](#origin)
	- [species](#origin_species)
	- [genus](#origin_genus)
	- [private_collection](#origin_private_collection)
		- [compound_source_type](#origin_private_collection_compound_source_type)
		- [purified_in_house](#origin_private_collection_purified_in_house)
			- [biological_material_source](#origin_private_collection_purified_in_house_biological_material_source)
		- [commercial](#origin_private_collection_commercial)
			- [supplier](#origin_private_collection_commercial_supplier)
			- [cas_number](#origin_private_collection_commercial_cas_number)
			- [catalogue_number](#origin_private_collection_commercial_catalogue_number)
		- [compound_library](#origin_private_collection_compound_library)
			- [library_name](#origin_private_collection_compound_library_library_name)
			- [library_description](#origin_private_collection_compound_library_library_description)
			- [library_compound_code](#origin_private_collection_compound_library_library_compound_code)
		- [other](#origin_private_collection_other)
			- [user_specified_compound_source](#origin_private_collection_other_user_specified_compound_source)
			- [biological_material_source](#origin_private_collection_other_biological_material_source)
- [depositor_info](#depositor_info)
	- [email](#depositor_info_email)
	- [account_id](#depositor_info_account_id)
	- [attribution_name](#depositor_info_attribution_name)
	- [attribution_organization](#depositor_info_attribution_organization)
	- [show_email_in_attribution](#depositor_info_show_email_in_attribution)
	- [show_name_in_attribution](#depositor_info_show_name_in_attribution)
	- [show_organization_in_attribution](#depositor_info_show_organization_in_attribution)
- [nmr_data](#nmr_data)
  - [peak_lists](#nmr_data_peak_lists)
    - [nucleus](#nmr_data_peak_lists_nucleus)
    - [solvent](#nmr_data_peak_lists_solvent)
    - [reference](#nmr_data_peak_lists_reference)
    - [values](#nmr_data_peak_list_values)
    - [frequency](#nmr_data_peak_lists_frequency)
    - [frequency_units](#nmr_data_peak_lists_frequency_units)
    - [temperature](#nmr_data_peak_lists_temperature)
    - [temperature_units](#nmr_data_peak_lists_temperature_units)
    - [peak_list_uuid](#nmr_data_peak_lists_peak_list_uuid)
    - [linked_peak_list_uuids](#nmr_data_peak_lists_linked_peak_list_uuids)
    - [peak_list_embargo_release_ready](#nmr_data_peak_lists_peak_list_embargo_release_ready)
  - [experimental_data](#nmr_data_experimental_data)
    - [nmr_data_download_link](#nmr_data_experimental_data_nmr_data_download_link)
    - [nmr_metadata](#nmr_data_experimental_data_nmr_metadata)
      - [vendor](#nmr_data_experimental_data_nmr_metadata_vendor)
      - [filetype](#nmr_data_experimental_data_nmr_metadata_filetype)
      - [solvent](#nmr_data_experimental_data_nmr_metadata_solvent)
      - [frequency](#nmr_data_experimental_data_nmr_metadata_frequency)
      - [frequency_units](#nmr_data_experimental_data_nmr_metadata_frequency_units)
      - [f1_nucleus](#nmr_data_experimental_data_nmr_metadata_f1_nucleus)
      - [f2_nucleus](#nmr_data_experimental_data_nmr_metadata_f2_nucleus)
      - [temperature](#nmr_data_experimental_data_nmr_metadata_temperature)
      - [temperature_units](#nmr_data_experimental_data_nmr_metadata_temperature_units)
      - [experiment_type](#nmr_data_experimental_data_nmr_metadata_experiment_type)
      - [extracted_experiment_folder](#nmr_data_experimental_data_nmr_metadata_extracted_experiment_folder)
      - [extracted_data_path](#nmr_data_experimental_data_nmr_metadata_extracted_data_path)
      - [spectrum_uuid](#nmr_data_experimental_data_nmr_metadata_spectrum_uuid)
      - [spectrum_embargo_release_ready](#nmr_data_experimental_data_nmr_metadata_spectrum_embargo_release_ready)
	- [assignment_data](#nmr_data_assignment_data)
        - [assignment_uuid](#nmr_data_assignment_data_assignment_uuid)
        - [curator_email_address](#nmr_data_assignment_data_curator_email_address)
        - [rdkit_version](#nmr_data_assignment_data_curator_rdkit_version)
        - [nucleus](#nmr_data_assignment_data_nucleus)
        - [solvent](#nmr_data_assignment_data_solvent)
        - [temperature](#nmr_data_assignment_data_temperature)
        - [temperature_units](#nmr_data_assignment_data_temperature_units)
        - [reference](#nmr_data_assignment_data_reference)
        - [frequency](#nmr_data_assignment_data_frequency)
        - [frequency_units](#nmr_data_assignment_data_frequency_units)
        - [assignment_data_embargo_release_ready](#nmr_data_assignment_data_release_ready)
        - [spectrum](#nmr_data_assignment_data_spectrum)
            - [shift](#nmr_data_assignment_data_spectrum_shift)
            - [integration](#nmr_data_assignment_data_spectrum_integration)
            - [multiplicity](#nmr_data_assignment_data_spectrum_multiplicity)
            - [coupling](#nmr_data_assignment_data_spectrum_spectrum_coupling)
            - [atom_index](#nmr_data_assignment_data_spectrum_atom_index)
            - [rdkit_index](#nmr_data_assignment_data_spectrum_rdkit_index)
            - [interchangeable_index](#nmr_data_assignment_data_spectrum_interchangeable_index)
	- [predicted_data](#nmr_data_predicted_data)
		- [prediction_method](#nmr_data_predicted_data_prediction_method)
		- [dft_protocol](#nmr_data_predicted_data_dft_protocol)
			- [molecular_dynamics](#nmr_data_predicted_data_dft_protocol_molecular_dynamics)
				- [md_software](#nmr_data_predicted_data_dft_protocol_molecular_dynamics_md_software)
				- [md_software_version](#nmr_data_predicted_data_dft_protocol_molecular_dynamics_md_software_version)
				- [forcefield](#nmr_data_predicted_data_dft_protocol_molecular_dynamics_forcefield)
				- [energy_window](#nmr_data_predicted_data_dft_protocol_molecular_dynamics_energy_window)
				- [downselection](#nmr_data_predicted_data_dft_protocol_molecular_dynamics_downselection)
			- [quantum_mechanics](#nmr_data_predicted_data_dft_protocol_quantum_mechanics)
				- [qm_software](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_qm_software)
				- [qm_software_version](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_qm_software_version)
				- [tasks](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_tasks)
				- [functionals](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_functionals)
				- [basis_sets](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_basis_sets)
				- [solvent](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_solvent)
				- [conversion_factors](#nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors)
		- [ml_protocol](#nmr_data_predicted_data_ml_protocol)
			- [ml_model](#nmr_data_predicted_data_ml_protocol_ml_model)
			- [training_set](#nmr_data_predicted_data_ml_protocol_training_set)
			- [training_parameters](#nmr_data_predicted_data_ml_protocol_training_parameters)
			- [chemical_shifts](#nmr_data_predicted_data_ml_protocol_chemical_shifts)
				- [solvent](#nmr_data_predicted_data_ml_protocol_chemical_shifts_solvent)
				- [temperature](#nmr_data_predicted_data_ml_protocol_chemical_shifts_temperature)
				- [temperature_units](#nmr_data_predicted_data_ml_protocol_chemical_shifts_temperature_units)
				- [h_nmr](#nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr)
					- [shift](#nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_shift)
					- [shielding_tensor](#nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_shielding_tensor)
					- [rdkit_index](#nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_rdkit_index)
				- [c_nmr](#nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr)
					- [shift](#nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_shift)
					- [shielding_tensor](#nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_shielding_tensor)
					- [rdkit_index](#nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_rdkit_index)

## compound_name <a name="compound_name"></a>
- Description: Common name for natural product. Can be a common name (as example), or an alpha-numeric code, or sometimes an IUPAC name. If compounds are not named in the paper they should be listed as 'Not named' to differentiate entries that are known to have no name from entries where the name is unknown (which should be Null)
- Example: `Exampleamide A`
- type: string
- MaxLength: 1000

## smiles <a name="smiles"></a>
- Description: Isomeric SMILES representation of compound structure
- Example: `CCC=CCCC`
- type: string
- MaxLength: 10000

## inchikey <a name="inchikey"></a>
- Description: Standard structure InChIkey for structure
- Example: `VQOIHQFCIVFBEC-IQPAJRPASA-N`
- type: string
- MinLegnth: 27
- MaxLength: 27

## npmrd_id <a name="npmrd_id"></a>
- Description: The primary key for the NP-MRD database. Format is 'NP' followed by 7 digits. Leading zeros must be included
- Example: `NP0000001`
- type: string
- MinLegnth: 9
- MaxLength: 9

## submission <a name="submission"></a>
- source <a name="submission_source"></a>
  - Description: Indicates which team generated the JSON.
  - One of: `deposition_system`, `npmrd_curator`, `dft_team`, or `ml_team`. 
  - Example: `deposition_system`
  - type: string
  - MaxLength: 30
- type <a name="submission_type"></a>
  - Description: Type of data submission.
  - One of: `published_article`, `presubmission_article`, or `private_deposition`
    - `published_article` published papers that have dois
    - `presubmission_article` papers under review that do not yet have dois
    - `private_deposition` datasets not associated with academic publications
  - Example: `published_article`
  - type: string
  - MaxLength: 30
- submission_uuid <a name="submission_uuid"></a>
  - Description: Internal reference ID for the deposition system. This is the primary key for submission-based data storage. Fixed length 36 character uuid string.
  - Example: `97d6db8a-631d-43bf-8afd-3208f79ec8d3`
  - type: string
  - MaxLength: 36
  - MinLength: 36
- compound_uuid <a name="submission_compound_uuid"></a>
  - Description: This uuid value is a short uuid value (10 characters) used as a secondary identifier to identify a specific compound in a deposition entry. This is a new addition added so that we do not need to rely on inchikey or name for this purpose. Required to address issues like atropisomers with the same InChIkey
  - Example: `SD0z84d9Ds`
  - type: string
  - MaxLength: 10
  - MinLength: 10
- submission_date <a name="submission_submission_date"></a>
  - Description: Date on which submission was made
  - Example: `2023-04-28T13:45:00.000Z`
- embargo_status <a name="submission_embargo_status"></a>
  - Description: User-specified field for embargo status. Allows users to set release condition for their data. 
  - One of: `release_immediately`, `do_not_release`, `embargo_until_date`, or `embargo_until_publication`
    - `publish` indicates that a user wishes to release this data immediately. This is the default value in the deposition system.
    - `embargo_until_date` indicates that the <i>embargo_date</i> field must be checked for the release date to make a submission public. It should be withheld from public access until then.
    - `embargo_until_publication` indicates to withhold the data from public access until the article is confirmed to be published OR a user manually releases the data. We will know an article has been published when a DOI is identified and attached to the specific compound/deposition by the deposition system.
  - Example: `embargo_until_publication`
  - type: string
  - MaxLength: 30
- embargo_date <a name="submission_embargo_date"></a>
  - Description: Embargo date for public release of the data
  - Example: `2023-07-22`
  - type: string
  - MaxLength: 10
- compound_embargo_release_ready <a name="submission_compound_embargo_release_ready"></a>
  - Description: Whether or not THIS COMPOUND is ready to be released according any embargoes on the submission. If false this compound's NP-Card should be put into an embargoed (not publicly available) state. If true the compound's NP-Card should be made publicly available. If the provided compound already exists in the main database FROM A DIFFERENT SOURCE this bool should be ignored. If it already exists FROM THE SAME SOURCE the value should be overwritten.
  - Example: true
  - type: bool


## citation <a name="citation"></a>
- doi <a name="citation_doi"></a>
  - Description: the Digital Object Identifier for the associated publication. Most articles have this
  - Example: `10.9999/npmr.99999999`
  - type: string
  - MaxLength: 1000
- pmid <a name="citation_pmid"></a>
  - Description: The PubMed Central ID number for the associated publications. Some articles have this.
  - Example: `32856641`
  - type: int
  - MaxLength: 20
- pii <a name="citation_pii"></a>
  - Description: Publisher Item Identifier. Used exclusively by the Elsevier publishing house. 
  - Example: `null`
  - type: string
  - MaxLength: 20

## origin <a name="origin"></a>
- species <a name="origin_species"></a>
  - Description: The species name for the producing organism. Not capitalized
  - Example: `coelicolor`
- genus <a name="origin_genus"></a>
  - Description: The genus name for the producing organism. Capitalized
  - Example: `Streptomyces`
- private_collection <a name="origin_private_collection"></a>
  - Description: This dictionary contains all necessary compound origin information if a user has completed a private deposition
  - compound_source_type <a name="origin_private_collection_compound_source_type"></a>
    - Description: For each compound a user adds to a private deposition they choose from one of four different source types. This field indicates which option was chosen for the given compound and therefore which field to extract origin information from. If the deposition is not a private deposition this field will be set to null
    - One of: `purified_in_house`, `commercial`, `compound_library`, or `other`.
      - If `commercial` is selected then <b>species</b> and <b>genus will NOT be submitted</b>. `supplier`, `cas_number`, and `catalogue_number` in this object will be filled.
      - If `purified_in_house` is selected then a <b>species</b>, <b>genus</b>, and biological_material_source <b>will be submitted</b>. Species and Genus are in the standard locations and the `biological_material_source` will be found in this object.
      - If `compound_library` is selected then species and genus will NOT be submitted.  `library_name`, `library_description`, and `library_compound_code` in this object will be filled.
      - If `other` is selected then <b>species and genus WILL be submitted</b>. `user_specified_compound_source`, and `biological_material_source_code` in this object will be filled.
    - Example: `purified_in_house`
    - type: string
    - MaxLength: 20
  - purified_in_house <a name="origin_private_collection_purified_in_house"></a>
    - biological_material_source <a name="origin_private_collection_purified_in_house_biological_material_source"></a>
      - Description: Origin of biological material. Did the raw material come from a collection trip, commercial supplier etc.
      - Example: `Natural herbs inc.`
      - type: string
      - MaxLength: 30
  - commercial <a name="origin_private_collection_commercial"></a>
    - supplier <a name="origin_private_collection_commercial_supplier"></a>
      - Description: Name of commercial supplier
      - Example: `Chromadex`
      - type: string
      - MaxLength: 50
    - cas_number <a name="origin_private_collection_commercial_cas_number"></a>
      - Description: Chemical Abstracts Service (CAS) number for compound. Almost all commercial materials have this. Digits and dashes only.
      - Example: `7184-60-3`
      - type: string
      - MaxLength: 50
    - catalogue_number <a name="origin_private_collection_commercial_catalogue_number"></a>
      - Description: Catalogue number from commercial suppliers. Optional field.
      - Example: `B3061`
      - type: string
      - MaxLength: 50
  - compound_library <a name="origin_private_collection_compound_library"></a>
    - library_name <a name="origin_private_collection_compound_library_library_name"></a>
      - Description: Description of compound library from which compound data derives
      - Example: `Linington lab pure compound library`
      - type: string
      - MaxLength: 256
    - library_description <a name="origin_private_collection_compound_library_library_description"></a>
      - Description: Description of library
      - Example: `Marine microbial natural products library. Isolated at SFU`
      - type: string
      - MaxLength: 2000
    - library_compound_code <a name="origin_private_collection_compound_library_library_compound_code"></a>
      - Description: In-house compound code
      - Example: `123456-123`
      - type: string
      - MaxLength: 200
  - other <a name="origin_private_collection_other"></a>
    - user_specified_compound_source <a name="origin_private_collection_other_user_specified_compound_source"></a>
      - Description: Description of source of data. Freeform field. 
      - Example: `Compounds donated by Professor X upon their retirement`
      - type: string
      - MaxLength: 2000
    - biological_material_source <a name="origin_private_collection_other_biological_material_source"></a>
      - Description: Origin of biological material. Did the raw material come from a collection trip, commercial supplier etc.
      - Example: `Natural herbs inc.`
      - type: string
      - MaxLength: 20

## depositor_info <a name="depositor_info"></a>
- Description: Data identifying the researcher who deposited the data to the system
- email <a name="depositor_info_email"></a>
  - Description: email address of depositor or lab point of contact
  - Example: `mpin@sfu.ca`
  - type: string
  - MaxLength: 200
- account_id <a name="depositor_info_account_id"></a>
  - Description: NP-MRD internal account ID number for depositor account
  - Example: `526`
  - type: int
  - MaxLength: 20
- attribution_name <a name="depositor_info_attribution_name"></a>
  - Description: Name of depositor to be displayed with data submission
  - Example: `Matthew Pin`
  - type: string
  - MaxLength: 200
- attribution_organization <a name="depositor_info_attribution_organization"></a>
  - Description: Name of organization to be displayed with submission
  - Example: `Simon Fraser University`
  - type: string
  - MaxLength: 200
- show_email_in_attribution <a name="depositor_info_show_email_in_attribution"></a>
  - Description: Boolean indicating whether email should be displayed on website. Note: at least one `show` term must be True
  - Example: `true`
  - type: boolean
- show_name_in_attribution <a name="depositor_info_show_name_in_attribution"></a>
  - Description: Boolean indicating whether name should be displayed on website
  - Example: `true`
  - type: boolean
- show_organization_in_attribution <a name="depositor_info_show_organization_in_attribution"></a>
  - Description: Boolean indicating whether organization should be displayed on website
  - Example: `true`
  - type: boolean

## nmr_data <a name="nmr_data"></a>
- peak_lists <a name="nmr_data_peak_lists"></a>
  - nucleus <a name="nmr_data_peak_lists_nucleus"></a>
    - Description: The atom the assignment data pertains to.
    - One of: `C` or `H`
    - Example: `C`
    - type: string
    - MaxLength: 1
  - solvent <a name="nmr_data_peak_lists_solvent"></a>
    - Description: Solvent in which NMR spectrum was acquired.
    - Example: `CDCl3`
    - type: string
    - MaxLength: 200
  - reference <a name="nmr_data_peak_lists_reference"></a>
    - Description: The referencing method (if known). Can be None. 'residual_solvent' is the most common entry. 
    - Example: `TMS`
    - type: string
    - MaxLength: 20
  - values <a name="nmr_data_peak_list_values"></a>
    - Description: Array of chemical shift values. 
    - Example: `[172.2, 171.6, 152.1, 146.0, 139.7, 137.7, 123.9, 122.2, 116.3, 106.9, 75.3, 62.8, 61.2, 29.0, 25.3, 24.9, 22.6, 21.4, 20.3, 20.3]`
    - type: array (of numbers)
    - MaxLength: null
  - frequency <a name="nmr_data_peak_lists_frequency"></a>
    - Description: Spectrometer frequency for NMR acquisition. Default unit is MHz
    - Example: `120`
    - type: number
    - maximum: 300
    - minimum: 1
  - frequency_units <a name="nmr_data_peak_lists_frequency_units"></a>
    - Description: Frequency units. Default is MHz
    - Example: `MHz`
    - type: string
    - MaxLength: 10
  - temperature <a name="nmr_data_peak_lists_temperature"></a>
    - Description: Sample temperature for data acquisition. Default units are K.
    - Example: `300`
    - type: integer
    - maximum: 500
    - minimum: 1
  - temperature_units <a name="nmr_data_peak_lists_temperature_units"></a>
    - Description: Units for temperature value. Defult is K.
    - Example: `K`
    - type: string
    - MaxLength: 10
  - peak_list_uuid <a name="nmr_data_peak_lists_peak_list_uuid"></a>
    - Description: uuid value unique to the provided peak list. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
    - Example: `SD0z84d9Ds-D0nP9`
    - type: string
    - MaxLength: 16
  - linked_peak_list_uuids <a name="nmr_data_peak_lists_linked_peak_list_uuids"></a>
    - Description: array of UUID values of other peak_list_uuids that are linked to this peak list
    - Example: `[BoS6oVW9Si-9a2rf]`
    - type: array of strings
  - peak_list_embargo_release_ready <a name="nmr_data_peak_lists_peak_list_embargo_release_ready"></a>
    - Description: Whether or not THIS PEAK LIST is ready to be released according any embargoes on the submission. If false this peak list should be put into an embargoed (not publicly available) state. If true the peak list should be made publicly available. If the provided peak_list already exists in the main database this bool should OVERWRITE it.
    - Example: true
    - type: bool
- experimental_data <a name="nmr_data_experimental_data"></a>
  - nmr_data_download_link <a name="nmr_data_experimental_data_nmr_data_download_link"></a>
    - Description: This field will provide a presigned amazon s3 url to download the data in the submission directly from the deposition website’s data warehouse. These links expire after 7 days (the max allowed duration).
    - Example: `[Link](https://article-pipeline-test-bucket.s3.amazonaws.com/serve/2023-01-17_16_32.zip?AWSAccessKeyId=AKIATG5FM4URBBXHRLET&Signature=B7QypMRri9XmWbdKUT%2BuFIqtKN4%3D&Expires=1682986167)`
    - type: string
    - MaxLength: null
  - nmr_metadata <a name="nmr_data_experimental_data_nmr_metadata"></a>
    - vendor <a name="nmr_data_experimental_data_nmr_metadata_vendor"></a>
      - Description: The manufacturer that built the NMR instrument. 
      - One of: `Bruker`, `Varian`, `JEOL`
      - Example: `Bruker`
      - type: string
      - MaxLength: 20
    - filetype <a name="nmr_data_experimental_data_nmr_metadata_filetype"></a>
      - Description: Specifies the format of the NMR data.
      - One of: `Varian_native`, `Bruker_native`, `JEOL_native`, `Jcampdx`, `Mnova`.
      - Example: `Bruker_native`
      - type: string
      - MaxLength: 30
    - solvent <a name="nmr_data_experimental_data_nmr_metadata_solvent"></a>
      - Description: Solvent in which NMR spectrum was acquired.
      - Example: `CDCl3`
      - type: string
      - MaxLength: 100
    - frequency <a name="nmr_data_experimental_data_nmr_metadata_frequency"></a>
      - Description: Array of spectrometer frequencies for NMR acquisition. Default unit is MHz. Can be 1 or 2 frequencies depending on experiment type. Always returned as an array.
      - Example: `[150.99]`
      - type: array (of numbers)
      - MaxLength: null
      - maxItems: 2
    - frequency_units <a name="nmr_data_experimental_data_nmr_metadata_frequency_units"></a>
      - Description: Frequency units. Default is MHz
      - Example: `MHz`, `Hz`
      - type: string
      - MaxLength: 10
    - f1_nucleus <a name="nmr_data_experimental_data_nmr_metadata_f1_nucleus"></a>
      - Description: Name of observed nucleus in the F1 dimension of the spectrum. Only required for 2D NMR experiments
      - Example: `13C`
      - type: string
      - MaxLength: 20
    - f2_nucleus <a name="nmr_data_experimental_data_nmr_metadata_f2_nucleus"></a>
      - Description: Name of observed nucleus in the F2 dimension of the spectrum. Required for all experiments.
      - Example: `1H`
      - type: string
      - MaxLength: 20
    - temperature <a name="nmr_data_experimental_data_nmr_metadata_temperature"></a>
      - Description: Sample temperature for data acquisition. Default units are K.
      - Example: `300`
      - type: integer
      - maximum: 500
      - minimum: 1
    - temperature_units <a name="nmr_data_experimental_data_nmr_metadata_temperature_units"></a>
      - Description: Units for temperature value. Default is K.
      - Example: `K`
      - type: string
      - MaxLength: 10
    - experiment_type <a name="nmr_data_experimental_data_nmr_metadata_experiment_type"></a>
      - Description: Standardized experiment type name. Describes which experiment (1H, COSY etc) is present
      - Example: `1D`
      - type: string
      - MaxLength: 100
    - extracted_experiment_folder <a name="nmr_data_experimental_data_nmr_metadata_extracted_experiment_folder"></a>
      - Description: Name of extracted data directory for this experiment in the extracted data directory for the compound
      - Example: `13C_1D`
      - type: string
      - MaxLength: 10000
    - extracted_data_path <a name="nmr_data_experimental_data_nmr_metadata_extracted_data_path"></a>
      - Description: Relative path to original data in `extracted_experiment_folder`.
      - Example: `13C_1D/acqu`
      - type: string
      - MaxLength: 10000
    - spectrum_uuid <a name="nmr_data_experimental_data_nmr_metadata_spectrum_uuid"></a>
      - Description: uuid value unique to the provided spectrum. Used as an identifier for the spectrum. The first 10 characters are the same as the `compound_uuid` while the last 5 characters are unique.
      - Example: `SD0z84d9Ds-j3f90`
      - type: string
      - MaxLength: 16
    - spectrum_embargo_release_ready <a name="nmr_data_experimental_data_nmr_metadata_spectrum_embargo_release_ready"></a>
      - Description: Whether or not THIS SPECTRUM is ready to be released according any embargoes on the submission. If false THIS SPECTRUM should be put into an embargoed (not publicly available) state. If true the compound's NP-Card should be made publicly available. If it already exists then the value should be overwritten.
      - Example: true
      - type: bool

## assignment_data <a name="nmr_data_assignment_data"></a>
  - assignment_uuid <a name="nmr_data_assignment_data_assignment_uuid"></a>
    - Description: uuid value unique to the provided assignment data. Used as an identifier for this assignment data. 38 characters in length due to including a standard 36 character uuid as well as an additional dash and nucleus character. The first 36 characters are identical to the "sister assignment entry" (between C and H) to make matching the two of them easier.
    - Example: `ff29e8c3-bbcb-4165-9631-a103743dd703-c`
    - type: string
    - MaxLength: 38
  - curator_email_address <a name="nmr_data_assignment_data_curator_email_address"></a>
    - Description: The email address of the individual that performed the curation using internal curation tools.
    - Example: `curatorName@outlook.com`
    - type: string
    - MaxLength: 1000
  - rdkit_version <a name="nmr_data_assignment_data_curator_rdkit_version"></a>
    - Description: The version of rdkit that was used to perform this curation. Included due to changes in index numbering between rdkit versions.
    - Example: `curatorName@outlook.com`
    - type: string
    - MaxLength: 100
  - nucleus <a name="nmr_data_assignment_data_nuclues"></a>
    - Description: The atom the assignment data pertains to.
    - One of: `C` or `H`
    - Example: `C`
    - type: string
    - MaxLength: 1
  - solvent <a name="nmr_data_assignment_data_solvent"></a>
    - Description: Solvent in which NMR spectrum was acquired.
    - Example: `CDCl3`
    - type: string
    - MaxLength: 20
  - temperature <a name="nmr_data_assignment_data_temperature"></a>
    - Description: Sample temperature for data acquisition. Default units are K.
    - Example: `300`
    - type: integer
    - maximum: 500
    - minimum: 1
  - temperature_units <a name="nmr_data_assignment_data_temperature_units"></a>
    - Description: Units for temperature value. Default is K.
    - Example: `K`
    - type: string
    - MaxLength: 10
  - reference <a name="nmr_data_assignment_data_reference"></a>
    - Description: The referencing method (if known). Can be None. 'residual_solvent' is the most common entry.
    - Example: `TMS`
    - type: string
    - MaxLength: 20
  - frequency <a name="nmr_data_assignment_data_frequency"></a>
    - Description: Spectrometer frequency for proton NMR acquisition. Default unit is MHz
    - Example: `300`
    - type: number
    - maximum: 500
    - minimum: 1
  - frequency_units <a name="nmr_data_assignment_data_frequency_units"></a>
    - Description: Frequency units. Default is MHz
    - Example: `MHz`
    - type: string
    - MaxLength: 10
  - assignment_data_embargo_release_ready <a name="nmr_data_assignment_data_release_ready"></a>
    - Description: Whether or not THIS ASSIGNMENT DATA is ready to be released according any embargoes on the submission. If false this data should be put into an embargoed (not publicly available) state. If true this data should be made publicly available. This value should override any previous versions of it that were sent.
    - Example: true
    - type: bool
  - spectrum <a name="nmr_data_assignment_data_spectrum"></a>
    - shift <a name="nmr_data_assignment_data_spectrum_shift"></a>
      - Description: Chemical shift for a given proton in the chemical structure
      - Example: `4.52`
      - type: number
      - maximum: 20
      - minimum: -2
    - integration <a name="nmr_data_assignment_data_spectrum_integration"></a>
      - Description: Relative integral under the curve for each signal. Computed from structure. Integer values
      - Example: `3`
      - type: integer
      - maximum: 1000
      - minimum: 0
    - multiplicity <a name="nmr_data_assignment_data_spectrum_multiplicity"></a>
      - Description: Description of the splitting of the signal. Examples include dd, dt, ddd dq etc.
      - Example: `t`
      - type: string
      - MaxLength: 20
    - coupling <a name="nmr_data_assignment_data_spectrum_spectrum_coupling"></a>
      - Description: Array of scalar coupling constants
      - Example: `[9.6]`
      - type: array (of numbers)
      - MaxLength: null
    - atom_index <a name="nmr_data_assignment_data_spectrum_atom_index"></a>
      - Description: Atom index from original data source (e.g. atom numbering from publication). Can be int or string
      - Example: `4'`
      - type: string
      - MaxLength: 50
    - rdkit_index <a name="nmr_data_assignment_data_spectrum_rdkit_index"></a>
      - Description: Array of atom indices using standard RDkit atom indexing. For example, CH3 will have three entries (one for each H) 
      - Example: `[10]`
      - type: array
      - MaxLength: null
    - interchangeable_index <a name="nmr_data_assignment_data_spectrum_interchangeable_index"></a>
      - Description: Array of RDkit atom indices that are interchangable with this assignment. Examples include diastereotopic methylene protons that are not explicitly defined, or atoms on two different groups (e.g. two CH3 groups) that cannot be unequivocally assigned with the available data
      - Example: `[]`
      - type: array
      - MaxLength: null


## predicted_data <a name="nmr_data_predicted_data"></a>
- prediction_method <a name="nmr_data_predicted_data_prediction_method"></a>
  - Description: NMR data calculation method. Options are `dft_protocol`, `ml_protocol` 
  - Example: `dft_protocol`
- dft_protocol <a name="nmr_data_predicted_data_dft_protocol"></a>
  - molecular_dynamics <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics"></a>
    - md_software <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics_md_software"></a>
      - Description: Name of Molecular Dynamics software
      - Example: `Gaussian`
      - type: string
      - MaxLength: 200
    - md_software_version <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics_md_software_version"></a>
      - Description: Version of MD software
      - Example: `3.1`
      - type: string
      - MaxLength: 200
    - forcefield <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics_forcefield"></a>
      - Description: Name of forcefield used for molecular dynamics calculations
      - Example: `gff`
      - type: string
      - MaxLength: 200
    - energy_window <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics_energy_window"></a>
      - Description: Energy window used for molecular dynamics calculations. Default unit is kcal 
      - Example: `3`
      - type: float
      - maximum: 250
      - minimum: 0
    - downselection <a name="nmr_data_predicted_data_dft_protocol_molecular_dynamics_downselection"></a>
      - Description: 
      - Example: `null`
  - quantum_mechanics <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics"></a>
    - qm_software <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_qm_software"></a>
      - Description: Name of quantum mechanical software
      - Example: `nwchem`
      - type: string
      - MaxLength: 200
    - qm_software_version <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_qm_software_version"></a>
      - Description: Version of QM software
      - Example: `6.8.2`
      - type: string
      - MaxLength: 200
    - tasks <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_tasks"></a>
      - Description: Array of calculation steps performed in QM calculations
      - Example: `[optimized, shielding]`
      - type: array
      - MaxLength: null
    - functionals <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_functionals"></a>
      - Description: Array of functionals used in QM calculations
      - Example: `[b3lyp, mpw91pw91]`
      - type: array
      - MaxLength: null
    - basis_sets <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_basis_sets"></a>
      - Description: Array of basis sets used in calculations
      - Example: `[6-31g*, 6-311+g**]`
      - type: array
      - MaxLength: null
    - solvent <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_solvent"></a>
      - Description: Solvent used in QM calculations
      - Example: `CHCl3`
      - type: array
      - MaxLength: null
    - conversion_factors <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors"></a>
      - H <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_H"></a>
        - m <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_H_m"></a>
             - Description:
             - Example: `1.0`
             - type: float
             - maximum: null
             - minimum: null
        - b <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_H_b"></a>
             - Description:
             - Example: `1.0`
             - type: float
             - maximum: null
             - minimum: null
      - C <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_C"></a>
        - m <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_C_m"></a>
            - Description:
            - Example: `1.0`
            - type: float
            - maximum: null
            - minimum: null
        - b <a name="nmr_data_predicted_data_dft_protocol_quantum_mechanics_conversion_factors_C_b"></a>
            - Description:
            - Example: `1.0`
            - type: float
            - maximum: null
            - minimum: null

- ml_protocol <a name="nmr_data_predicted_data_ml_protocol"></a>
  - ml_model <a name="nmr_data_predicted_data_ml_protocol_ml_model"></a>
    - Description: Name of ML model used for calculations. This should include sufficient detail to find and reuse model
    - model_name
  - training_set <a name="nmr_data_predicted_data_ml_protocol_training_set"></a>
    - Description: Description and location of dataset used for model training. 
    - training_set_name
  - training_parameters <a name="nmr_data_predicted_data_ml_protocol_training_parameters"></a>
    - parameter_1 <a name="nmr_data_predicted_data_ml_protocol_training_parameters_parameter_1"></a>
      - Description: Generic parameter as placeholder in JSON. Will require updating when NP-MRD starts to insert ML data
      - Example: `null`
    - parameter_2 <a name="nmr_data_predicted_data_ml_protocol_training_parameters_parameter_2"></a>
      - Description: Generic parameter as placeholder in JSON. Will require updating when NP-MRD starts to insert ML data
      - Example: `null`
  - chemical_shifts <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts"></a>
    - solvent <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_solvent"></a>
      - Description: Solvent used for ML calculations
      - Example: `CDCl3`
    - temperature <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_temperature"></a>
      - Description: Temperature used for ML calculations. Default unit is K
      - Example: `298`
    - temperature_units <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_temperature_units"></a>
      - Description: Units for ML calculations. Default is K.
      - Example: `K`
    - h_nmr <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr"></a>
      - shift <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_shift"></a>
        - Description: Chemical shift for proton signal at this position in the molecule
        - Example: `4.56`
        - type: number
        - maximum: 20
        - minimum: -2
      - shielding_tensor <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_shielding_tensor"></a>
        - Description: Array of shielding tensor values
        - Example: `3X3 array`
        - type: array
        - MaxLength: null
      - rdkit_index <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_h_nmr_rdkit_index"></a>
        - Description: Atom index using standard RDkit atom indexing
        - Example: `1`
        - type: int
        - maximum: 2000
        - minimum: 1
    - c_nmr <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr"></a>
      - shift <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_shift"></a>
        - Description: Chemical shift for carbon signal at this position in the molecule
        - Example: `71.2`
        - type: number
        - maximum: 250
        - minimum: -10
      - shielding_tensor <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_shielding_tensor"></a>
        - Description: Array of shielding tensor values
        - Example: `3X3 array`
        - type: array
        - MaxLength: null
      - rdkit_index <a name="nmr_data_predicted_data_ml_protocol_chemical_shifts_c_nmr_rdkit_index"></a>
        - Description: Atom index using standard RDkit atom indexing
        - Example: `5`
        - type: int
        - maximum: 2000
        - minimum: 1

