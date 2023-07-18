## Requirments By Origin
- [Required Fields for Deposition](#required_fields_for_deposition)

## Required Fields for Deposition <a name="required_fields_for_deposition"></a>
There are 6 fields that must be filled to use the new exchange format. Other fields should always be in the JSON but simply left null if they are not needed.

- **smiles**
- **inchikey**
- **submission**
  - **source**: (where the JSON was generated)
  - **submission_date**: (when)
  - **embargo_status**: (always be explicit about whether there is or isn’t an embargo)
- **citation**
  - None
- **origin**
  - None
- **depositor_info**
  - Email. This is so we know who created the data. Preferable to account_id for now since historical depositions do not have an account_id in the deposition database
- **nmr_data**
  - None
