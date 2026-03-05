# Joint CDS WG3 (Customer Data) Meeting 2026-02-05

Recording: https://zoom.us/rec/share/0qSWbXgaCuPYS4r87cNRzdWQPW1lvf9F12lHP46IAWZSQ2CKx_AwgM0SwRfaGnTj.1QuuhGm_AdLVM2s4

## Agenda
* Welcome
* Program Participation discussion

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Atakan Esen (Apple)
* Madhuban Kumar (CarbonLaces)
* Soazig Kaam (Google)

## Minutes
* Welcome
* Recapping last month's discussion on Rich Authorization Requests framework
* Program Participation
  * Need to add fields to Account/Service Contract objects

```
GET /.well-known/oauth-authorization-server

{
    ...
    "cds_scope_descriptions": {...},
    "cds_registration_fields": {...},
    ...
    # TODO: add to spec
    "cds_default_language": "en",  # add to CDS-WG1-02?
    "cds_programs": {
        "dram": {
            "program_id": "dram",
            "type": "demand_response",
            "name": "CAISO DRAM participation (2025 edition)", # TODO: specify as "translatable string", so client knows that it could have multiple entries with languages
            "name#es": "CAISO ... (in spanish)",  # see https://www.rfc-editor.org/rfc/rfc7591#section-2.2
        },
        ...
    },

    "cds_scope_descriptions": {
        "program_participation": {
            "authorization_details_fields_supported": {
                "program_eligibility": {
                    "format": "choice",
                    "choices": [
                        "dram",
                    ],
                }
            }
        }
    }
}


GET /authorize
?client_id=111111111
&response_type=code
&scope=program_participation+rate_plan
&code_challenge=.......
&code_challenge_method=S256
&authorization_details=[
    {
        "type": "program_participation",

        # TODO: new filters
        "program_ids": ["dram"],
        # OR
        "program_types": ["demand_response"],
        # OR
        "program_eligibility": ["dram"],

        "include_account_numbers": true,
        "include_contract_numbers": true,
        "sync_until": "P0S|P30DT2H|infinite|2026-12-31T00:00:00Z",
    },
    {
        "type": "rate_plan",
        "include_account_numbers": true,
        "include_contract_numbers": true,
        "sync_until": "P0S|P30DT2H|infinite|2026-12-31T00:00:00Z",
    },
]


GET /service-contracts
Authorization: Bearer aaaaaaaaaa

{
    "service_contracts": [
        {
            "cds_servicecontract_id": "3939393",
            ...
            "rateplan_code": "COMM-1",
            "program_participations": [],
            ...
        },
        {
            "cds_servicecontract_id": "9994984949",
            ...
            "rateplan_code": "COMM-1",
            "program_participations": [
                {
                    "id": "dr_123",
                    "program_id": "dram",
                    "status": "ACTIVE",
                    "start_date": "2022-01-01",
                },
                ...
            ],
            ...
        },
    ],
}
```

* TODO: rateplan riders vs. rateplan code vs. programs

## Closing Discussion
* Consensus to commit this to repo? Yes

