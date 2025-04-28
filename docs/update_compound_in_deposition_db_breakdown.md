## Update Compound In Deposition Db

Json used to take name, species, genus, or structure updates that has been performed on the database website and send them back to the deposition website.

The provided json will cause any fields changed in the database website (from when they were when originally sent from the deposition system) to be updated in the deposition system.

Should be called whenever a compound with a deposition system compound_uuid is updated on the database website. This allows both the database site and the deposition system to keep up to date with one another.


- [submission_uuid](#submission_uuid)
- [compounds](#compounds)
  - [compound_name](#compounds_compound_name)
  - [compound_uuid](#compounds_compound_uuid)
  - [compound_smiles](#compounds_compound_smiles)
  - [compound_inchikey](#compounds_compound_inchikey)
  - [npmrd_id](#compounds_npmrd_id)
  - [genus](#compounds_genus)
  - [species](#compounds_species)

# JSON
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
        [{
            'compound_name': 'TEST COMP 2',
            'compound_uuid': 'GrJXd6HDqY',
            'compound_smiles': CCC,
            'compound_inchikey': VQOIHQFCIVFBEC-IQPAJRPASA-N,
            'genus': 'TEST GENUS',
            'species': 'TEST SPECIES'
        }]
    ```

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