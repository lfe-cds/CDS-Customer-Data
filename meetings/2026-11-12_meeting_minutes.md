# Joint CDS WG3 (Customer Data) Meeting 2026-11-12

Recording: https://zoom.us/rec/share/mF232cydJxOCDtAKoLSlnkR2xA5Y9G1JvT4ndMiwH39SVW9B4ihSbDiX0IkiEemS.ewoB8kOm69ttBLul

## Agenda
* Welcome
* Review open issues and pull requests

## Attendees
* Daniel Roesler (Maintainer)
* Dakshil Shah (Apple)
* Brett Kerrigan (Dunsky Energy + Climate)
* Ghazal Kamyabjou (California Energy Commission)

## Minutes
* Welcome and intros
* Discuss Registration #21 (for Entity objects)
    * https://github.com/lfe-cds/CDS-Registration/issues/21
* Scenarios
    * Overview of what Scenarios are
    * [Brett]
        * Benchmarking, Carbon Reporting
        * EU ratings / specs
    * [Daniel] Discussion of Whole-Building Data Scenario


```
GET /api/aggregations?aggregation_numbers=BUILDING_ID_0001

{
    "aggregations": [
        {
            ...
            "aggregation_number": "BUILDING_ID_0001",
            "grouped_addresses": ["123 Main St"],
            "scope": "cds_query_usage",
            "authorization_details: [
                {
                    "type": "cds_query_usage",
                    "aggregation_numbers": ["BUILDING_ID_0001"],
                },
            ],
            "consent_requirements": [
                {
                    "id": "cr111111111111",
                    "name": "CA Tenant Consent Req",
                    "types": ["always"],
                    "documentation": "https://...",
                },
            ]
            ...
        }
    ],
    ...
}


POST /oauth/token
{
    "grant_type": "client_credentials",
    "scope": "cds_query_usage",
    "authorization_details": [
        {
            "type": "cds_query_usage",
            "aggregation_numbers": ["BUILDING_ID_0001"],
        },
    ],
}
Response:
{
    "access_token": "333333333333333aaaaaaaaaaaa",
    "scope": "cds_query_usage",
    "authorization_details": [
        {
            "type": "cds_query_usage",
            "aggregation_numbers": ["BUILDING_ID_0001"],
        },
    ],
    "cds_grant_ids": ["g9999"],
}


GET /api/grants?grant_ids=g9999
{
    "grants": [
        {
            "grant_id": "g9999",
            "scope": "cds_query_usage",
            "authorization_details": [
                {
                    "type": "cds_query_usage",
                    "aggregation_numbers": ["BUILDING_ID_0001"],
                },
            ],
            "status": "needs_sub_grants",  # indicates needs individual tenant auths (see "parent" of other Grants)
            "enabled_scope": "",  # empty because
            ...
        },
        {
            "grant_id": "g9999-sub1",
            "parent": "g9999",
            "scope": "cds_aggregation_consent",
            "authorization_details": [
                {
                    "type": "cds_aggregation_consent",
                    "aggregation_numbers": ["BUILDING_ID_0001"],
                },
            ],
            "status": "needs_authorization",  # indicates still haven't received consent from the customer
            "enabled_scope": "",
            ...
        },
    ],
    ...
}


Authoriztion Request to customer:
{auth_url}/authorize?client_id=...&scope..&cds_grant_id=g9999-sub1

[Customer Authorization Form] # customer completes the auth form


# Query Grants API again
GET /api/grants?grant_ids=g9999
{
    "grants": [
        {
            "grant_id": "g9999",
            "scope": "cds_query_usage",
            "authorization_details": [
                {
                    "type": "cds_query_usage",
                    "aggregation_numbers": ["BUILDING_ID_0001"],
                },
            ],
            "status": "active",
            "enabled_scope": "cds_query_usage",
            ...
        },
        {
            "grant_id": "g9999-sub1",
            "parent": "g9999",
            "scope": "cds_aggregation_consent",
            "authorization_details": [
                {
                    "type": "cds_aggregation_consent",
                    "aggregation_numbers": ["BUILDING_ID_0001"],
                },
            ],
            "status": "active",
            "enabled_scope": "cds_aggregation_consent",
            ...
        },
    ],
    ...
}

# Since Grant g9999 is active, can now download the data
GET /api/usage-segments
{
    "usage_segments": [
        {
            "related_aggregations": ["..."],
            "formats": [...],  # format of the usage data
            "values": [...],   # aggregated interval values (monthly/15min, etc.)
            ...
        }
    ],
    ...
}
```

## Closing Discussion
* Consensus to commit this to repo? Yes

