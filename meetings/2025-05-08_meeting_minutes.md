# Joint CDSC WG1 & WG3 (Connectivity and Customer Data) Meeting 2024-05-08

Recording: https://zoom.us/rec/share/9XgLTDKK-e0XnIAfQzIXwuAM7DlQjr3ZgMcdmTNxpt7WAvo32ohkNEa2ItaRErQT.2yv9zJFLgm1ZzE7c

## Agenda
* Welcome
* Continue discussion of Customer Data scopes

## Attendees
* Daniel Roesler (UtilityAPI) (Maintainer)
* Barbra dos Santos (Arcadia)
* Jordan Hughes (Apple)
* Soazig Kaam (Google)
* Don Coffin (GBA)
* Eloi Ferrer (Flexidao)

## Minutes
* Overview of SF Climate Week event
* Name change expectation
* Scope Types:

```
# Authorization Request:
https://example.com/oauth/authorize
?client_id=aaaaaaa
&response_type=code
&redirect_uri=https://mysite.com/redirect

&scope=cds_bill_statements

&authorization_details=[
    {
        "type": "cds_bill_statements",
        "statement_date_start": "2021-01-01",
        "statement_date_end": "2026-01-01",
        "fieldset": "amount_due_only",
    },

    {
        "type": "cds_bill_statements_amount_due_only",
        "statement_date_start": "2021-01-01",
        "statement_date_end": "2026-01-01",
    }
]
```

* simplified scopes, with fieldset parameter:
    * Pros:
        * fewer scopes to define in the spec
        * scopes tend to make more logical sense to users (e.g. "ask for bills", "ask for intervals", etc.)
    * Con:
        * hides specificity into the fieldset, which is important for customer consent disclosure
        * Servers must define a default fieldset/flavor, which may be unintuitive
* verbose scopes (no fieldset/flavor):
    * Pros:
        * scope itself defines which fields are included, so no need for sub-parameter
    * Cons:
        * lots more scopes (N scopes * M fieldsets/flavors)
        * for authorizations, more difficult for a server to "combine" scopes in a coehesive UI
        * for different time range parameters, how will the Server combine overlapping scopes?

```
&authorization_details=[
    {
        "type": "cds_bill_statements_amount_due_only",
        "statement_date_start": "today",
        "statement_date_end": "2026-01-01",
        "jwk": {<public_key>},
        "required": True,
    },
    {
        "type": "cds_bill_statements_balance",
        "statement_date_start": "2021-01-01",
        "statement_date_end": "2026-01-01",
    },
]

&authorization_details=[
    {
        "type": "cds_bill_statements",
        "statement_date_start": "2021-01-01",
        "statement_date_end": "2026-01-01",
        "fieldsets": ["amount_due_only", "balance"],
        "required": True,
    },
]
```




## Closing Discussion
* Consensus to commit this to repo? Yes/No

