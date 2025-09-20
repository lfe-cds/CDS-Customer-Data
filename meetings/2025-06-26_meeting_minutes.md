# CDS WG3 (Customer Data) Meeting 2025-06-26

Recording: https://zoom.us/rec/share/3ltJIplrK7CUKZVu5GSQ0-a6k-VrPyAKpBoUAWWK_w7nzb9d_Ayz0zuHnatQWym4.q-d1O_MtsUxcoB-7

## Agenda
* Welcome
* Client-initiated data request scenarios to playtest:
    * Customers requesting their own data (as a Third Party Client)
    * Third Party Clients requesting specific accounts
    * Third Party Clients searching for customer accounts, then requesting data for found accounts
    * Third Party Clients searching for aggregations, then requesting aggregated data

## Attendees
* Daniel Roesler (UtilityAPI) (Maintainer)
* Don Coffin (GBA)
* Jordan Hughes (Apple)
* Eloi Ferrer (Flexidao)
* Soazig Kaam (Google)

## Minutes
* Intro and welcome
* Goals of WG for Q3 2025:
    * Sept 10-11, LF Energy Summit EU
    * Main goal: merge in maintainer's draft before summit
* Client-initiated data request scenarios
    * Situations where the utility approves data access, not the customer (or customer consent is obtained out-of-band)

* Scenario 1:
    * Company (large enterprise customer) wants their own energy data
    * Utility (Server) is working directly with directly with Company
    * API Steps:
```
# Company (or company's vendor) registers as a "Client"
POST /oauth/registration
{
    "client_id": "aaaaaaa",
    ...
}

# Utility sees this Client(s) in their backend and knows it's Company

# Utility manually creates a Grant for the Company's Accounts + Customer Data

# Client sees this Grant in the Grants API

# Company uses `grant_admin` token to get access_token for Grant

# Company downloads data from Customer Data APIs using access_token

```
* [Eloi] how to check if already existing client_id for Customer of Flexidao's?
    * If Customer knows their previously registered Client:
        * Option 1: Client creates a Credential for Flexidao and shares that secret with Flexidao
            * Gives Flexidao access to all the Grants that the Client has (i.e. their own data access)
            * Pro: no additional registration required; Con: Flexidao is effectively the same level of access as Customer
        * Option 2: Flexidao registers a seperate client_id, and the Customer messages (via Messages API) the Utility asking for an additional Grant to be issued to Flexidao's client_id.
            * Pro: can only Grant access to the data that's needed; Con: requires Utility to do something new (issue the new Grant to Flexidao's)
        * Option 3: Flexidao already has a Client registered with Utility, so the Customer messages (via Messages API) the Utility asks for a Grant to be issued to Flexidao's client_id.
            * Pro: consolidated Grants on Flexidao's client_id; Con: same as Option 2
    * If Customer doesn't know their previously registered Client:
        * Option 1: Customer registers a new Client
            * Pro: no need to find the old Client; Con: now multiple Clients for a Customer in the Server
        * Option 2: Flexidao creates (or already has) a client_id for them (not the Customer), and the Customer messages the Utility asks for a Grant to be issued to Flexidao's client_id.
            * Pro: Customer doesn't have to register a new Client; Con: Messaging the Utility cannot be done via Messages API

* We need a way to have one Client (i.e. Customer's Client) be able to request a Grant for another Client (i.e. Customer's vendor's Client):
    * Currently, in the spec, the Messages API is unstructured
        * so for Customer to ask Utility to create a Grant: send unstructured message ("Please share my data with client_id=flexidao")
    * What is a better API for one Client to submit a new Grant request for another Client?
```
# since Grants are for self-Client, not ideal for this scenario
POST /api/grants

# maybe use Messages API?
POST /api/messages
{
    "type": "private_message|support_request|client_submission",
    ...
}
# maybe add new type? "grant_request"
e.g. POST /api/messages
{
    "type": "grant_request",
    "name": "Please create a Grant...",
    "description": "I have a new vendor...",
    "related_uri": "http://example.com/api/grants",
    "related_type": "grant_list",
    "updates_requested": [
        {
            "client_id": "flexidao",
            "field": "scope",
            "new_value": "bill_data",
        },
    ],
}
e.g. POST /api/messages
{
    "type": "grant_request",
    "name": "Please create a Grant...",
    "description": "I have a new vendor...",
    "related_uri": "http://example.com/api/clients/flexidao",
    "related_type": "client",
    "grants_requested": [
        {
            "scope": "bill_data",
            "authorization_details": [...],
        },
    ],
}
```
* [Soazig] Heavy on the customer, especially if not IT resources



## Closing Discussion
* Consensus to commit this to repo? Yes
