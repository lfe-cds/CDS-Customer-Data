# Joint CDSC WG1 & WG3 (Connectivity and Customer Data) Meeting 2024-04-10

Recording: https://zoom.us/rec/share/RREJTQ03-YHrz5jpMKJMSPemAJeXaK7RPkVruO5sWMWlTvq2fHTSH6gQ2DGaLV0n.alnbLy7UzYiNQz_u

## Agenda
* Welcome
* Continue discussion of Customer Data scopes

## Attendees
* Daniel Roesler (UtilityAPI) (Maintainer)
* Don Coffin (GBA)
* Eloi Ferrer (Flexidao)
* Soazig Kaam (Google)

## Minutes
* Welcome
* Name change proposed by LF Energy
    * Carbon Data Specification (CDS) --> Connectivity Data Specification (CDS)
    * Same acronym, so few changes to the specs.
    * Things to discuss:
        * Working Group 1 (Connectivity) may change names (WG1 Connectivity --> WG1 Interoperability)
        * Website domain (connectivity.carbondataspec.org / customerdata.carbondataspec.org)
            * Probably change to a new domain (will have redirects from old to new)
        * Github repo (github.com/carbon-data-specification)
            * Change org name (e.g. github.com/cds-specs???)
        * Working group website changes:
            * Scope (will follow whatever language LFESS determines)
            * Use Cases
                * Connectivity: https://github.com/carbon-data-specification/Connectivity/pull/6
                    * likely don't need to update
                * Customer Data: https://daniel-utilityapi.github.io/Customer-Data/use-cases
                    * maybe add use cases based on expanded LFESS scope
                    * [Eloi] how would we expand to cover all use cases
                        * [Daniel] New working groups for new non-customer data use cases
                        * So prefer to keep focused on customer data for WG3
        * Meetings
            * Likely splitting Connectivity and Customer Data WG meetins into separate meetings
                * Alternate meetings WG1 / WG3 every other week (so 1 WG meeting per month)
        * Mailing lists
            * Please join both mailing lists!

* Customer Data scope fields (for granular auths)
```
Example authorization_details:
[
    {
        "type": "cds_current_accounts_details",
        "sync_date_end": "2026-01-01T00:00:00Z",
        "fields": [
            "account_number",
            "account_name",
            ...
        ],
    },
    {
        "type": "cds_bill_statements",
        "statement_date_start": "2021-01-01",
        "statement_date_end": "2026-01-01",
        "flavor": "usage_only",
    },
    {
        "type": "cds_usage",
        "segment_start": "2021-01-01T00:00:00Z",
        "segment_end": "2026-01-01T00:00:00Z",
        "aggregation_type": "monthly_aggregates",
    },
]
```

## Closing Discussion
* Consensus to commit this to repo? Yes

