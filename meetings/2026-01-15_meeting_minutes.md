# Joint CDS WG3 (Customer Data) Meeting 2026-01-15

Recording: https://zoom.us/rec/share/Qainn5aD1pBDMUYps2C_z55FiMCiVa6VPXqcxlwhAnW02gMAXE5li9g6-JAur017.q4eDq1ml9C9ddcbL

## Agenda
* Welcome
* Continue discussion of Scopes and Authorization Detail Fields

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)
* Jordan Hughes (Apple)
* Soazig Kaam (Google)

## Minutes
* Welcome and happy new year
* Scope playtesting

Self-access scopes (Accounts Query, Service Contracts Query, ...* Query)
```
POST /token
Authorization: Basic {base64(client_id:client_secret)}

grant_type=client_credentials
&scope=usage_query
&authorization_details=[
    {
        "type": "accounts_query",
        "account_numbers": ["aaaaaa-1", ...],
    },
]
RESPONSE:
{
    "access_token": "aaaaaaaaaa",
    ...
}


GET /accounts
Authorization: Bearer aaaaaaaaaa

{
    "accounts": [
        {
            "cds_account_id": "9494949494",
            ...
            "account_number": "aaaaaa-1",
            ...
        },
    ],
}
```

Third party request scopes (Accounts List, Rate Plan, Meter Usage, etc.)
```
GET /authorize
?client_id=111111111
&response_type=code
&scope=rate_plan
&code_challenge=.......
&code_challenge_method=S256
&authorization_details=[
    {
        "type": "rate_plan",
        "service_types": ["electric"],
        "allow_scope_modifications": false,
        "error_if_no_preselections": true,
        "include_account_numbers": true,
        "include_contract_numbers": true,
        "sync_until": "P0S|P30DT2H|infinite|2026-12-31T00:00:00Z",
    },
]
Response (redirect)
GET /{redirect_uri}
?code=kkkkkkkkkkkkkkkkk
&state=...
POST /token
Authorization: Basic {base64(client_id:client_secret)}

grant_type=authorization_code
&code=kkkkkkkkkkkkkkkkk
&code_verifier=...

RESPONSE:
{
    "access_token": "aaaaaaaaaa",
    ...
}

GET /service-contracts
Authorization: Bearer aaaaaaaaaa

{
    "service_contracts": [
        {
            "cds_servicecontract_id": "9994984949",
            "cds_modified": "2026...",
            ...
            "account_number": "aaaaaaa-1",
            "contract_number": "bbbbbbb-1",
            "rateplan_code": "RES-1",
            "rateplan_name": "Residential Electric Service",
            ...
        },
    ],
}
```

## Closing Discussion
* Consensus to commit this to repo? Yes

