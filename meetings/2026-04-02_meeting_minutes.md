# Joint CDS WG3 (Customer Data) Meeting 2026-04-02

Recording: https://zoom.us/rec/share/NagSmUV0fFunDSO-kyKubaNrl78qc9YveS-BCZj3wWbm2ZRjP5FLsA3WjMN3npU.WFMUWMupy2sbO-7o

## Agenda
* Welcome
* Overview of changes since last meeting
* Review CDS-WG3-01 spec (Customer Data) progress (authorization details fields, customer authorization form outline)
    * Pull request: https://github.com/lfe-cds/CDS-Customer-Data/pull/31
    * Spec draft: https://daniel-roesler.github.io/CDS-Customer-Data/specs/cds-wg3-01/

## Attendees
* Daniel Roesler (Maintainer)
* Don Coffin (GBA)

## Minutes
* Welcome
* Overview of merged Pull Request #33 (cleaning up old CDSC spec placeholders)
* Overview of latest changes to Customer Data spec
    * https://github.com/lfe-cds/CDS-Customer-Data/pull/31
    * Authorization Details Fields content


* Preselection Fields
    * [Don] How does Authorization Server know if those resources exist?
        * [Daniel] TODO: add section specifying that if `error_if_no_preselections` is included as a auth details field, Authorization Server must be able to know what resources are available on the Resource server for the Preselection Fields that are options.
```
# grant_type=authorization_code (i.e. customer consent)
GET /oauth/authorize
?client_id=111111
&response_type=code
&scope=cds_accounts
&authorization_details=[
    {
        "type": "cds_accounts",
        "account_numbers": ["110033-4"],
        "error_if_no_preselections": true
    }
]
# (on the authorization form, the Account #110033-4 is preselected for authorization, and other accounts are not selected.)


# grant_type=client_credentials (i.e. self-access or internal vendor access)
POST /oauth/token
?grant_type=client_credentials
&scope=cds_query_accounts
&authorization_details=[
    {
        "type": "cds_query_accounts",
        "account_numbers": ["110033-4"]
        "error_if_no_preselections": true
    }
]
# (access token is limited to Account #110033-4, and not all Accounts available to that Client)
```

* Included Data Fields
```
GET /oauth/authorize
?client_id=111111
&response_type=code
&scope=cds_accounts
&authorization_details=[
    {
        "type": "cds_accounts",
        "include_service_contracts": true,   # (won't get contract_numbers by default)
        "include_service_contract_numbers": true,   # (will now include contract_numbers in Service Contract objects)
    }
]
# Account API returns Accounts, as defined in the cds_accounts scope,
# AND the Service Contracts API would return Service Contracts related to those Accounts (which isn't normally included in the scope, but is included since the include_service_contracts field is true)
```

* Duration Fields
```
GET /oauth/authorize
?client_id=111111
&response_type=code
&scope=cds_accounts
&authorization_details=[
    {
        "type": "cds_accounts",
        "sync_util": "P30"
    }
]
# Accounts API is kept updated until the sync_until expires
# Account objects have cds_synced datetime for when the Account's data was last validated against the source of truth
```

* Other Fields
    * Selection Type

* Authorization Form Layout - how the auth form structure is required

```
[Scope Segment] - single scope or combined set of scopes that have th
    [Data Component] - what data is being requested for authorization
    [Duration Component] - time ranges for the authorization
    [Selection Component] - what resources (Accounts, etc.)
        [Account|Service Contract|Service Point|Meter Device|Aggregation Selection] - the UI list type that the Customer selects from
[Scope Segment]
[Scope Segment]
...
[Authorize Button][Decline Button]
```
* [Don] will include examples of what auth form will look like?
    * [Daniel] I was thinking of including some diagrams/wireframes, but not a fully rendered example
    * TODO Reference: EMV spec (for online credit card purchases) for UI examples



## Closing Discussion
* Consensus to commit this to repo? Yes

