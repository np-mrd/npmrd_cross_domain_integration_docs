---
layout: default
title: Home
nav_enabled: true
nav_order: 1
has_children: true
---
# Index

## JSON Breakdowns
- [Exchange Breakdown](./exchange_breakdown.md)

## Deposition Exchange

- [Exchange Report Breakdown](./deposition_exchange/exchange_report_breakdown.md): The JSON returned from the database site to the deposition platform following ingestion of compounds included in the data exchange json
- [Embargo Update Breakdown](./deposition_exchange/embargo_update_breakdown.md): A json sent from the deposition platform to the database site to trigger previously embargoed data to be released.
- [Identifier Update Breakdown](./deposition_exchange/identifier_update_breakdown.md): A json sent from the deposition platform to the database site to add an identifier (doi or pii) to a presubmission that previously lacked one.
- [Update Compound in Deposition DB Breakdown](./deposition_exchange/update_compound_in_deposition_db_breakdown.md): A json sent from the database site to the deposition platform to update any compound names or structures that have changed.
- [How to Test](./deposition_exchange/how_to_test.md)
