# Joint CDS WG3 (Customer Data) Meeting 2025-11-13

Recording: https://zoom.us/rec/share/jydcdwMXmh5thclsRpqjmIXeHYi09eH_9hjDcCp_rPQmjQddpbs3tByJAES0Kgyd.FIBwyHj8Ce-ybxmJ

## Agenda
* Welcome
* Discuss new "Scenarios" section of draft spec.
    * https://github.com/daniel-roesler/CDS-Customer-Data/blob/issue_24_customer_data_partial/specifications/cds-wg3-01.md#scenarios
* Discuss new strategy for selecting which fields are included.

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Jordan Hughes (Apple)

## Minutes
* Welcome
* Pull request review
    * Use Cases (#30)
    * Customer Data Spec (#31)
* Scopes and Auth details fields

```
/oauth/authorize
?response_type=code
&client_id=aaabbb123
&scope=cds_current_rate_plan
&authorization_details=[
    {
        "type": "cds_current_rate_plan",

        # All-or-nothing
        "allow_scope_modifications": false,

        # Service Contract preselection
        "account_numbers": ["A000000000-1", ...],
        "contract_numbers": ["S000000000-1", ...],
        "meter_numbers": ["M000000000-1", ...],
        "addresses": ["123 main st...", ...],
        "service_types": ["electric", ...],
        "error_if_no_preselections": true,

        # Scope-specific
        "sync_date_end": "infinite|now|P7D|2025-12-31T00:00:00Z",
        "include_account_numbers": true,
        "include_contract_numbers": true,
        "include_service_types": true,
        "include_contract_address": true,
    },
]
```
* [Jordan] If client leaves off "include_contract_numbers", is the field still included?
    * [Daniel] In Client Registration spec (CDS-WG1-02) the auth details fields metadata can provide this info

```
# auth server metadata
{
    ...
    "cds_scope_descriptions": {
        ...
        "cds_current_rate_plan": {
            "id": "cds_current_rate_plan",
            "name": "...",
            "description": "...",
            "documentation": "...",
            "registration_requirements": [],
            "response_types_supported": ["code"],
            "grant_types_supported": ["authorization_code", "refresh_token"],
            "token_endpoint_auth_methods_supported": ["client_secret_basic"],
            "code_challenge_methods_supported": ["S256"],
            "coverages_supported": ["..."],
            "authorization_details_types_supported": [
                "cds_current_rate_plan",
            ],
            "authorization_details_fields_supported": [
                {
                    "id": "sync_date_end",
                    "name": "Sync Date End",
                    "description": "...",
                    "documentation": "...",
                    "for_types": ["cds_current_rate_plan"],
                    "format": "relative_or_absolute_datetime",
                    "is_required": false,
                    "default": "infinite",
                    "maxiumum": "infinite",
                    "minimum": "P0D",
                },
                {
                    "id": "include_contract_numbers",
                    "name": "...",
                    "description": "...",
                    "documentation": "...",
                    "for_types": ["cds_current_rate_plan"],
                    "format": "boolean",
                    "is_required": false,
                    "default": true,
                },
                ...
            ]
        },
        ...
    },
    ...
}


/oauth/authorize
?response_type=code
&client_id=aaabbb123
&scope=cds_current_rate_plan
&authorization_details=[
    {
        "type": "cds_current_rate_plan",
        //"sync_date_end: "infinite", // the default
        //"include_contract_numbers: true, // the default
    },
]
```

* Scenarios section
    * New section that defines common "groups" of requirements for Servers


## Closing Discussion
* Consensus to commit this to repo? Yes

