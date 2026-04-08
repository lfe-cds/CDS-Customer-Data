# CDS-WG3-01 - Customer Data

## Abstract <a id="abstract" href="#abstract" class="permalink">🔗</a>

This specification defines how utilities and other central entities ("Servers") may allow vendors, customers, and other external organizations ("Clients") to securely access account holder data ("Customer Data"), including in situations where Customer consent where needed or data is aggregated.
This specification extends [[CDS-WG1-02](#ref-cds-wg1-02)] ("Client Registration") to add new authorization [Scopes](#scopes) and defines new Application Programming Interfaces (APIs) for Customer Data.

## Status <a id="status" href="#status" class="permalink">🔗</a>

<b style="color:red">WARNING: This specification is a DRAFT and has not achieved consensus by the working group.</b>

## Copyright <a id="copyright" href="#copyright" class="permalink">🔗</a>

Copyright Joint Development Foundation Projects, LLC, LF Energy Standards and Specifications Series and its contributors ("LFESS").
All rights reserved.
For more information, visit [https://lfess.energy/](https://lfess.energy/).

## Table of Contents <a id="table-of-contents" href="#table-of-contents" class="permalink">🔗</a>

* [1. Introduction](#introduction)  
* [2. Terminology](#terminology)  
* [3. Scenarios](#scenarios)  
    * [3.1. Usage Self-Access](#scenario-usage-self-access)  
    * [3.2. Billing Self-Access](#scenario-billing-self-access) 
    * [3.3. Usage and Billing Self-Access](#scenario-self-access)   
    * [3.4. Meter Data](#scenario-meter-data)  
    * [3.5. Rate Plan](#scenario-rate-plan)  
    * [3.6. Program Participation](#scenario-program-participation)  
    * [3.7. Program Analysis](#scenario-program-analysis)  
    * [3.8. Bill Amount Due](#scenario-bill-amount-due)  
    * [3.9. Bill Statements](#scenario-bill-statements)  
    * [3.10. Service Charges](#scenario-service-charges)  
    * [3.11. Whole-Building Data](#scenario-building-data)  
    * [3.12. Aggregated Data](#scenario-aggregated-data)  
    * [3.13. EAC Data](#scenario-eac-data)  
    * [3.14. Internal Access](#scenario-internal-access)  
* [4. CDS-WG1-02 Extension](#cds-wg1-02-extension)  
* [5. Server Metadata](#server-metadata)  
* [6. Scopes Supported](#scopes)  
    * [6.1. Customer Consent Scopes](#scopes-customer-consent)  
        * [6.1.1. Rate Plan](#scope-rate-plan)  
        * [6.1.2. Account Program Participation](#scope-account-program-participation)  
        * [6.1.3. Service Program Participation](#scope-service-program-participation)  
        * [6.1.4. Account List](#scope-account-list)  
        * [6.1.5. Service List](#scope-service-list)  
        * [6.1.6. Meter List](#scope-meter-list)  
        * [6.1.7. Bill Statements](#scope-bill-statements)  
        * [6.1.8. Service Bills](#scope-service-bills)  
        * [6.1.9. Meter Usage](#scope-meter-usage)  
        * [6.1.10. Aggregation Inclusion](#scope-aggregation-inclusion)  
    * [6.2. Direct Access Scopes](#scopes-direct-access)  
        * [6.2.1. Accounts Query](#scope-accounts-query)  
        * [6.2.2. Service Contracts Query](#scope-service-contracts-query)  
        * [6.2.3. Service Points Query](#scope-service-points-query)  
        * [6.2.4. Meters Devices Query](#scope-meter-devices-query)  
        * [6.2.5. Bill Statements Query](#scope-bill-statements-query)  
        * [6.2.6. Bill Sections Query](#scope-bill-sections-query)  
        * [6.2.7. Aggregations Query](#scope-aggregations-query)  
        * [6.2.8. Usage Query](#scope-usage-query)  
    * [6.3. Scope Extensions](#scope-extensions)  
* [7. Authorization Details Fields](#auth-details-fields)  
    * [7.1. Preselection Fields](#auth-details-preselection)  
        * [7.1.1. Preselect Account Numbers](#auth-details-account-numbers)  
        * [7.1.2. Preselect Account Programs](#auth-details-account-programs)  
        * [7.1.3. Preselect Contract Numbers](#auth-details-contract-numbers)  
        * [7.1.4. Preselect Service Types](#auth-details-service-types)  
        * [7.1.5. Preselect Service Programs](#auth-details-service-programs)  
        * [7.1.6. Preselect Service Point Numbers](#auth-details-servicepoint-numbers)  
        * [7.1.7. Preselect Service Point Types](#auth-details-servicepoint-types)  
        * [7.1.8. Preselect Premise Numbers](#auth-details-premise-numbers)  
        * [7.1.9. Preselect Meter Numbers](#auth-details-meter-numbers)  
        * [7.1.10. Preselect Meter Device Types](#auth-details-meter-device-types)  
        * [7.1.11. Preselect Aggregation Numbers](#auth-details-aggregation-numbers)  
        * [7.1.12. Preselect Addresses](#auth-details-addresses)  
            * [7.1.12.1. Address Matching](#auth-details-address-matching)  
    * [7.2. Included Data Fields](#auth-details-included-data)  
        * [7.2.1. Include Accounts](#auth-details-include-accounts)  
            * [7.2.1.1. Include Account Numbers](#auth-details-include-account-numbers)  
            * [7.2.1.2. Include Account Details](#auth-details-include-account-details)  
            * [7.2.1.3. Include Account Programs](#auth-details-include-account-programs)  
        * [7.2.2. Include Service Contracts](#auth-details-include-service-contracts)  
            * [7.2.2.1. Include Contract Numbers](#auth-details-include-contract-numbers)  
            * [7.2.2.2. Include Rate Plans](#auth-details-include-rate-plans)  
            * [7.2.2.3. Include Service Contract Details](#auth-details-include-contract-details)  
            * [7.2.2.4. Include Service Programs](#auth-details-include-service-programs) 
        * [7.2.3. Include Service Points](#auth-details-include-service-points)  
            * [7.2.3.1. Include Premises](#auth-details-include-premises)  
            * [7.2.3.2. Include Service Point Addresses](#auth-details-include-service-point-addresses)  
            * [7.2.3.3. Include Coordinates](#auth-details-include-coordinates)  
        * [7.2.4. Include Meter Devices](#auth-details-include-meter-devices)  
            * [7.2.4.1. Include Meter Numbers](#auth-details-include-meter-numbers)  
        * [7.2.5. Include Bill Statements](#auth-details-include-bill-statements)  
            * [7.2.5.1. Include Bill Statement Files](#auth-details-include-bill-statement-files)  
            * [7.2.5.2. Include Bill Statement Charges](#auth-details-include-bill-statement-charges) 
            * [7.2.5.3. Include Bill Statement Programs](#auth-details-include-bill-statement-programs) 
        * [7.2.6. Include Bill Sections](#auth-details-include-bill-sections)  
            * [7.2.6.1. Include Bill Section Line Items](#auth-details-include-bill-section-line-items)  
        * [7.2.7. Include Aggregations](#auth-details-include-aggregations)  
            * [7.2.7.1. Include Aggregation Addresses](#auth-details-include-aggregation-addresses)  
        * [7.2.8. Include Usage Segments](#auth-details-include-usage-segments)  
            * [7.2.8.1. Include Usage Segment Formats](#auth-details-include-usage-segment-formats)  
        * [7.2.9. Include Energy Attribute Certificates](#auth-details-include-eacs)  
    * [7.3. Duration Fields](#auth-details-duration)  
        * [7.3.1. Sync Until](#auth-details-sync-until)  
        * [7.3.2. Bill Statement Date Start](#auth-details-statement-start)  
        * [7.3.3. Bill Statement Date End](#auth-details-statement-end)  
        * [7.3.4. Bill Section Start Date](#auth-details-bill-section-start)  
        * [7.3.5. Bill Section End Date](#auth-details-bill-section-end)  
        * [7.3.6. Usage Segment Start](#auth-details-usage-segment-start)  
        * [7.3.7. Usage Segment End](#auth-details-usage-segment-end)  
        * [7.3.8. Expires](#auth-details-expires)  
    * [7.4. Other Fields](#auth-details-other)  
        * [7.4.1. Selection Type](#auth-details-selection-type)  
        * [7.4.2. Merge Selections](#auth-details-merge-selections)  
        * [7.4.3. Error If No Preselections](#auth-details-error-if-no-preselections)  
        * [7.4.4. Allow Scope Modifications](#auth-details-allow-scope-modifications)  
* [8. Client Registration Requirements](#client-registration-requirements)  
* [9. Customer Authorizations](#customer-authorizations)  
    * [9.1. Authorization Form Layout](#auth-form-layout)  
    * [9.2. Authorization Form Segments](#auth-form-layout)  
        * [9.2.1. Data Component](#data-component)  
        * [9.2.2. Duration Component](#duration-component)  
        * [9.2.3. Selection Component](#selection-component)  
            * [9.2.3.1. Account Selection](#account-selection)  
            * [9.2.3.2. Service Contract Selection](#contract-selection)  
            * [9.2.3.3. Service Point Selection](#servicepoint-selection)  
            * [9.2.3.4. Meter Device Selection](#meter-selection)  
            * [9.2.3.5. Aggregation Selection](#aggregation-selection)  
    * [9.3. Authorization Errors](#auth-errors)  
        * [9.3.1. Preselection Error](#preselection-error)  
* [10. Accounts API](#accounts-api)  
    * [10.1. Account Object Format](#account-format)  
    * [10.2. Listing Accounts](#accounts-list)  
* [11. Service Contracts API](#service-contracts-api)  
    * [11.1. Service Contract Object Format](#service-contract-format)  
    * [11.2. Listing Service Contracts](#service-contract-list)  
* [12. Service Points API](#service-points-api)  
    * [12.1. Service Point Object Format](#service-point-format)  
    * [12.2. Listing Service Points](#service-point-list)  
* [13. Meter Devices API](#meter-devices-api)  
    * [13.1. Meter Device Object Format](#meter-device-format)  
    * [13.2. Listing Meter Device](#meter-device-list)  
* [14. Bill Statements API](#bill-statements-api)  
    * [14.1. Bill Statement Object Format](#bill-statement-format)  
    * [14.2. Listing Bill Statements](#bill-statement-list)  
* [15. Bill Sections API](#bill-sections-api)  
    * [15.1. Bill Section Object Format](#bill-section-format)  
    * [15.2. Listing Bill Sections](#bill-section-list)  
* [16. Aggregations API](#aggregations-api)  
    * [16.1. Aggregation Object Format](#aggregation-format)  
    * [16.2. Listing Aggregations](#aggregation-list)  
* [17. Usage Segments API](#usage-segments-api)  
    * [17.1. Usage Segment Object Format](#usage-segment-format)  
    * [17.2. Usage Segment Value Formats](#usage-segment-value-format)  
    * [17.3. Usage Segment Value Set Format](#usage-segment-value-set-format)  
    * [17.4. Usage Segment Value Object Format](#usage-segment-value-object-format)  
    * [17.5. Listing Usage Segments](#usage-segment-list)  
* [18. Energy Attribute Certificates API](#eac-api)  
    * [18.1. Energy Attribute Certificate Object Format](#eac-format)  
    * [18.2. Beneficiary Types](#eac-beneficiary-types)  
    * [18.3. EAC Data Format Description Object](#eac-data-format-descriptions)  
    * [18.4. Listing Energy Attribute Certificates](#eac-list)  
* [19. Extensions](#extensions)  
* [20. Security Considerations](#security)  
* [21. Examples](#examples)  
* [22. References](#references)  
* [23. Acknowledgments](#acknowledgments)  
* [24. Authors' Addresses](#authors-addresses)  

## 1. Introduction <a id="introduction" href="#introduction" class="permalink">🔗</a>

This specification was developed as part of the global effort to facilitate grid modernization and the energy transition.
Specifically, in order to scalably deploy and economically operate and analyze new energy technologies, external organizations and Customers need an automated means of requesting access to Customer Data from utilities and other central entities.

There are thousands of utilities serving Customers across the world, and each have their own way of providing access to and formatting Customer Data.
This specification defines a way for these utilities and other central entities ("Servers") to provide a standardized, structured process for external users and organizations ("Clients") to access Customer Data, requesting consent from Customers if needed.
By offering a standardized a Customer Data access protocol, utilities and other central entities can offer secure, automated, managed means of access to Customer Data.

## 2. Terminology <a id="terminology" href="#terminology" class="permalink">🔗</a>

<a id="server" href="#server" class="permalink">🔗</a> **"Server"** - The entity hosting the specified endpoints.
A Server can be an energy utility or another type of entity that want to provide access to privileged functionality or data.
These entities can include, but are not limited to, distribution utilities, grid operators, electric retailers, community choice aggregators, government agencies, data warehouses, and private infrastructure providers.

<a id="client" href="#client" class="permalink">🔗</a> **"Client"** - The entity requesting Server's metadata endpoints.
A Client can be any organization or user seeking to access Customer data with a Server for a specific scope of access.
These entities can include, but are not limited to, energy efficiency contractors, utility vendors, distributed energy resource companies, building energy management platforms, and even Customers themselves (to gain automated access their own data).

<a id="customer" href="#customer" class="permalink">🔗</a> **"Customer"** - The entity who's data is being requested from a Server by a Client ("Customer Data").
For grants that require customer authorization, the Customer is the user who authorizes the access.
Customers are typically the account holder, also commonly called the "customer of record", such as homeowners, renters, businesses, and industrial companies.
Customers may also be a representative of the customer of record, such as an energy management company, provided that the reprentative has been duly authorized give consent on the customer of record's behalf according to local regulatory requirements.

<a id="array" href="#array" class="permalink">🔗</a> "Array" - A list of objects or values as defined by `Arrays` in [[RFC 8259 Section 5](#ref-rfc8259-arrays)].

<a id="date" href="#date" class="permalink">🔗</a> "date" - A string representing date in the format of `full-date` as defined by [[RFC 3339 Section 5.6](#ref-rfc3339-datetime)] (e.g. "2024-01-01").

<a id="datetime" href="#datetime" class="permalink">🔗</a> "datetime" - A string representing date and time in the format of `date-time` as defined by [[RFC 3339 Section 5.6](#ref-rfc3339-datetime)] (e.g. "2024-01-01T00:00:00Z").

<a id="decimal" href="#decimal" class="permalink">🔗</a> "decimal" - A decimal value as defined by `number` in [[RFC 8259 Section 6](#ref-rfc8259-numbers)].

<a id="get" href="#get" class="permalink">🔗</a> "GET" - A request method defined in [[RFC 9110 Section 9](#ref-rfc9110-methods)].

<a id="https" href="#https" class="permalink">🔗</a> "HTTPS" - A secure web request protocol defined in [[RFC 9110 Section 4.2.2](#ref-rfc9110-https)].

<a id="json" href="#json" class="permalink">🔗</a> "JSON" - A data format defined in [[RFC 8259](#ref-rfc8259)].

<a id="map" href="#map" class="permalink">🔗</a> "Map" - A JSON object as defined by `Objects` in [[RFC 8259 Section 4](#ref-rfc8259-objects)].

<a id="mime-type" href="#mime-type" class="permalink">🔗</a> "MIME type" - A string representing a document media type as defined in [[RFC 6838](#ref-rfc6838)] (e.g. "image/png").

<a id="null" href="#null" class="permalink">🔗</a> "null" - The `null` value defined in [[RFC 8259 Section 3](#ref-rfc8259-values)].

<a id="status-code" href="#status-code" class="permalink">🔗</a> "Status Code" - A response status code defined in [[RFC 9110 Section 15](#ref-rfc9110-codes)].

<a id="string" href="#string" class="permalink">🔗</a> "string" - A series of unicode characters as defined in [[RFC 8259 Section 7](#ref-rfc8259-strings)].

<a id="url" href="#url" class="permalink">🔗</a> "URL" - A string representing resource as defined in [[RFC 3986 Section 1.1.3](#ref-rfc3986-url)] (e.g. "https://example.com/page1").

<a id="key-words" href="#key-words" class="permalink">🔗</a> Key Words: "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" are defined in accordance with [[BCP 14](#ref-bcp14)].

## 3. Scenarios <a id="scenarios" href="#scenarios" class="permalink">🔗</a>

Customer Data access is necessary for many different use cases, but not all use cases require the same type of access or same fields of Customer Data.
For example, an energy efficiency auditor working with a Customer may need historical usage intervals, but no bill data.
In contrast, another example is for a building management company to need ongoing access to a Customer's bill statements for accounting and reporting.

This section defines "Scenarios" as sets of [Scopes](#scopes) and configuration requirements for Scope Descriptions that are intended to meet the Scenario's use cases.
Because Scenarios are defined sets of Scopes and configuration requirements, they don't actually appear in any API responses by the Server.
Rather, Scenarios are intended to aide regulators, governing authorities, and [Extensions](#extensions) in choosing which Scopes, data fields, and functionality they require Servers to implement.

Regulators, governing authorities, and extensions MAY choose to require implementation of any combination of Scenarios defined in this specification or choose to define new Scenarios.
When defining new Scenarios, it is RECOMMENDED that those Scenarios are defined either as a list of modifications to one or more Scenarios from this specification or, if a completely new Scenario, using the same format as the Scenarios defined in this specification.

New Scenarios MAY include requirements to extend various object formats or numerated values to include additional fields.
For example, a regulator could define a new Scenario that extends the "Meter Data" Scenario by requiring an additional `CAISO_pricing_node` field be included in the [Service Contract](#service-contract-format) object.
It is RECOMMENDED that when requiring new fields for new Scenarios, that defining the new fields follows the [Extensions](#extensions) section, including adding a relevant prefix to the new field names (e.g. `CAISO_*`).

New Scenarios MAY also define new [Scopes](#scopes) that specify a specialized levels of access for Clients of the Scenario's use cases.
When defining new Scopes, it is RECOMMENDED that those Scopes are defined either as a list of modifications to one or more Scopes from this specification or, if a completely new Scope, using the same format as the otherScopes defined in this specification.

In addition to implementing requirements for their jurisdiction, Servers MAY implement any Scope or configuration that helps provide Customer Data access for relevant internal or external use cases.
For example, a utility could enable Scopes that streamline how they transfer program participant meter data to their contracted program administrator vendor for internal program analysis and reporting.

Clients MUST NOT rely on stated Scenario compliance from regulators or Servers.
Instead, Clients MUST parse the [Server Metadata](#server-metadata) to evaluate if a Server's offered Scopes and functionality is adequate for their use case.

### 3.1. Usage Self-Access <a id="scenario-usage-self-access" href="#scenario-usage-self-access" class="permalink">🔗</a>

This section defines the Scenario "Usage Self-Access" in which a utility Customer wants to access their own accounts' meter and usage data.
For example, a commercial customer would like to download their historical meter usage feed for their owned properties, so that their energy management team can evaluate their buildings' energy profiles.
Another example is for a data center owner to want to integrate an automatic nightly update of their previous day's meter usage, so that they can check their operations for discrepancies between their own energy monitoring systems.

To implement the "Usage Self-Access" Scenario, Servers MUST implement the following list of requirements.

* Servers MUST include at least one of each [Accounts Query](#scope-accounts-query), [Service Contracts Query](#scope-service-contracts-query), [Service Points Query](#scope-service-points-query), [Meter Devices Query](#scope-meter-devices-query), and [Usage Query](#scope-usage-query) Scope Descriptions in the Server Metadata `cds_scope_descriptions` object, as well as include those Scope Description `id` values in the Server Metadata `scopes_supported` list.
* Servers MUST include any required steps for verifying the identity of a Client as the Customer in the Scope Description `registration_requirements` list.
  For example, if a Server requires Customers to associate their online web account with Clients they register, the Server would include a Registration Field [[CDS-WG1-02 Section 3.5](#ref-cds-wg1-02-registration-fields)] with a `type` value of `sso_verification`.
* Once Servers have verified the identity of the Client as the Customer, Servers MUST configure the Customer's Accounts and Service Contracts to be accessible to the Client using the [Accounts Query](#scope-accounts-query) and [Service Contracts Query](#scope-service-contracts-query) scopes.
* Servers MUST configure any Meter Devices, Service Points, and Usage Segments associated with the Customer's Accounts and Service Contracts to be accessible to the Client using the [Service Points Query](#scope-service-points-query), [Meter Devices Query](#scope-meter-devices-query), and [Usage Query](#scope-usage-query) scopes.
* Servers MUST keep the list of Accounts, Service Contracts, Service Points, Meter Devices, and Usage Segments that are associated with the Client updated to reflect any changes to the Customer.
  For example, if a Customer builds a new warehouse on their property and installs a new electric service for the warehouse, the Client should see any new Accounts, Service Contracts, Service Points, Meter Devices, and Usage Segments for the new warehouse included in API objects returned when data is next synced from the Server.
* The [Usage Query](#scope-usage-query) Scope Description's `segment_start` authorization details field MUST allow the Client to access for each connected Service Contract's usage at least 1 year of historical usage data (i.e. `minimum` value set to at least `P1Y`).
* The [Usage Query](#scope-usage-query) Scope Description's `segment_end` authorization details field MUST have the `maximum` field's value set to `infinite`, which means Clients MAY authorize access to.
* <span style="background-color:yellow">TODO: what `include_*` auth details</span>

### 3.2. Billing Self-Access <a id="scenario-billing-self-access" href="#scenario-billing-self-access" class="permalink">🔗</a>

This section defines the Scenario "Billing Self-Access" in which a utility Customer wants to access their own accounts' utility bills.
For example, an enterprise customer would like automate importing their utility bills into their accounting software.

To implement the "Billing Self-Access" Scenario, Servers MUST implement the following list of requirements.

* Servers MUST include at least one of each [Accounts Query](#scope-accounts-query), [Service Contracts Query](#scope-service-contracts-query), [Bill Statements Query](#scope-bill-statements-query), and [Bill Sections Query](#scope-bill-sections-query) Scope Descriptions in the Server Metadata `cds_scope_descriptions` object, as well as include those Scope Description `id` values in the Server Metadata `scopes_supported` list.
* Servers MUST include any required steps for verifying the identity of a Client as the Customer in the Scope Description `registration_requirements` list.
  For example, if a Server requires Customers to associate their online web account with Clients they register, the Server would include a Registration Field [[CDS-WG1-02 Section 3.5](#ref-cds-wg1-02-registration-fields)] with a `type` value of `sso_verification`.
* Once Servers have verified the identity of the Client as the Customer, Servers MUST configure the Customer's Accounts and Service Contracts to be accessible to the Client using the [Accounts Query](#scope-accounts-query) and [Service Contracts Query](#scope-service-contracts-query) scopes.
* Servers MUST also configure any Bill Statements and Bill Sections associated with the Customer's Accounts and Service Contracts to be accessible to the Client using the [Bill Statements Query](#scope-bill-statements-query) and [Bill Sections Query](#scope-bill-sections-query) scopes.
* Servers MUST keep the list of Accounts, Service Contracts, Bill Statements, and Bill Sections that are associated with the Client updated to reflect any changes to the Customer.
  For example, if a Customer buys a property and transfers the utility services for that property to a new account under their profile, the Client should see any new Accounts, Service Contracts, Bill Statements, and Bill Sections for the new account in API objects returned when data is next synced from the Server.
* Bill Statement object `file_uri` and `file_mimetype` fields are REQUIRED to be included, so that Clients MAY download copies of their utility bill statements.
* The [Bill Statements Query](#scope-bill-statements-query) Scope Description's `statement_date_start` authorization details field MUST allow the Client to access for each connected Account at least 1 year of historical bill statements (i.e. `minimum` value set to at least `P1Y`).
* The [Bill Statements Query](#scope-bill-statements-query) Scope Description's `statement_date_end` authorization details field MUST allow the Client to access for each connected Account for any future duration of time (i.e. `maximum` value set to at least `in`).
* The [Bill Sections Query](#scope-bill-sections-query) Scope Description's `start_date` authorization details field MUST allow the Client to access for each connected Service Contract at least 1 year of historical bill sections (i.e. `minimum` value set to at least `P1Y`).
* <span style="background-color:yellow">TODO: what `include_*` auth details</span>

### 3.3. Usage and Billing Self-Access <a id="scenario-self-access" href="#scenario-self-access" class="permalink">🔗</a>

This section defines the Scenario "Usage and Billing Self-Access" in which a utility Customer wants to access their own accounts' usage and bills.
For example, an enterprise customer would like automate a energy billing.

To implement the "Usage and Billing Self-Access" Scenario, Servers MUST implement the following list of requirements.

* Servers MUST implement the [Usage Self-Access](#scenario-usage-self-access) Scenario.
* Servers MUST implement the [Billing Self-Access](#scenario-billing-self-access) Scenario.

### 3.4. Meter Data <a id="scenario-meter-data" href="#scenario-meter-data" class="permalink">🔗</a>

This section defines the Scenario "Meter Data" in which a Client requests access to a Customer's historical and/or ongoing usage data.
For example, an energy efficiency auditor is working with building owner, and needs to download their previous year's usage.

To implement the "Meter Data" Scenario, Servers MUST implement the following list of requirements.

* Servers MUST include at least one of each [Accounts Query](#scope-accounts-query), [Service Contracts Query](#scope-service-contracts-query), [Service Points Query](#scope-service-points-query), [Meter Devices Query](#scope-meter-devices-query), and [Usage Query](#scope-usage-query) Scope Descriptions in the Server Metadata `cds_scope_descriptions` object, as well as include those Scope Description `id` values in the Server Metadata `scopes_supported` list.
* The [Usage Query](#scope-usage-query) Scope Description's `segment_start` authorization details field MUST allow the Client to access for each connected Service Contract's usage at least 1 year of historical usage data (i.e. `minimum` value set to at least `P1Y`).

### 3.5. Rate Plan <a id="scenario-rate-plan" href="#scenario-rate-plan" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.6. Program Participation <a id="scenario-program-participation" href="#scenario-program-participation" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.7. Program Analysis <a id="scenario-program-analysis" href="#scenario-program-analysis" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.8. Bill Amount Due <a id="scenario-bill-amount-due" href="#scenario-bill-amount-due" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.9. Bill Statements <a id="scenario-bill-statements" href="#scenario-bill-statements" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.10. Service Charges <a id="scenario-service-charges" href="#scenario-service-charges" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.11. Whole-Building Data <a id="scenario-building-data" href="#scenario-building-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.12. Aggregated Data <a id="scenario-aggregated-data" href="#scenario-aggregated-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.13. EAC Data <a id="scenario-eac-data" href="#scenario-eac-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.14. Internal Access <a id="scenario-internal-access" href="#scenario-internal-access" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 4. CDS-WG1-02 Extension <a id="cds-wg1-02-extension" href="#CDS-WG1-02-extension" class="permalink">🔗</a>

As the framework for providing Client registration, onboarding, communication, management, and grant authorizations, Servers MUST implement CDS's Client Registration specification [[CDS-WG1-02](#ref-cds-wg1-02)].
This specification in subsequent sections extends the CDS's Client Registration framework by defining additional fields, values, and APIs that are relevant to Customer Data access, so that Servers implementing this specification can satisfy Customer Data use cases.

<span style="background-color:yellow">TODO</span>

## 5. Server Metadata <a id="server-metadata" href="#server-metadata" class="permalink">🔗</a>

This specification extends CDS's Authorization Server Metadata object [[CDS-WG1-02 Section 3.2](#ref-cds-wg1-02-metadata)] to include the following named values:

<span style="background-color:yellow">TODO: check this list</span>

* `cds_accounts_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Accounts API](#accounts-api).
  This is REQUIRED if a [Rate Plan](#scope-rate-plan), [Self-Access](#scope-rate-plan), or [Accounts Query](#scope-accounts-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_servicecontracts_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Service Contracts API](#service-contracts-api).
  This is REQUIRED if a [Rate Plan](#scope-rate-plan) or [Service Contracts Query](#scope-service-contracts-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_servicepoints_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Service Points API](#service-points-api).
  This is REQUIRED if a [Meter List](#scope-meter-list) or [Service Points Query](#scope-service-points-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_meterdevices_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Meter Devices API](#meter-devices-api).
  This is REQUIRED if a [Meter List](#scope-meter-list) or [Meter Devices Query](#scope-meter-devices-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_billstatement_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Bill Statements API](#bill-statements-api).
  This is REQUIRED if a [Bill Statements](#scope-bill-statements) or [Bill Statements Query](#scope-bill-statements-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_billsection_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Bill Sections API](#bill-sections-api).
  This is REQUIRED if a [Service Bills](#scope-service-bills) or [Bill Sections Query](#scope-bill-sections-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_aggregations_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Aggregations API](#aggregations-api).
  This is REQUIRED if an [Aggregations Query](#scope-aggregations-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_usagesegments_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Usage Segments API](#usage-segments-api).
  This is REQUIRED if a [Meter Usage](#scope-meter-usage) or [Usage Query](#scope-usage-query) Scope is included in the Server Metadata object's `scopes_supported` field.
* `cds_eacs_api` - _[URL](#url)_ - (OPTIONAL) The base url for the [Usage Segments API](#usage-segments-api).
  This is REQUIRED if any Scope Descriptions `authorization_details_fields_supported` list has an [Include EACs](#auth-details-field-include-eacs) authorization details field object.
* `cds_eac_formats` - _Map[[EACDataFormatDescription](#eac-data-format-descriptions)]_ - (OPTIONAL) An object providing additional information about each Energy Attribute Certificate (EAC) data format value that may be provided as the [EAC object](#eac-format) `eac_format`, with the [EAC Data Format Description's](#eac-data-format-descriptions) `id` as the keys of the object and values being the [EAC Data Format Description](#eac-data-format-descriptions) object itself.
  This is REQUIRED if the `cds_eacs_api` field is populated in the Server Metadata object.

## 6. Scopes Supported <a id="scopes" href="#scopes" class="permalink">🔗</a>

This specification extends CDS's Client Registration Scopes Supported [[CDS-WG1-02 Section 3.3](#ref-cds-wg1-02-scopes)] to define the following additional Scope Descriptions [[CDS-WG1-02 Section 3.4](#ref-cds-wg1-02-scope-descriptions)] that a Server MAY add to the `scopes_supported` field in the Authorization Server Metadata object [[CDS-WG1-02 Section 3.2](#ref-cds-wg1-02-metadata)].

To support a Scope defined in this section, in addition to that Scope's defined requirements, the Server MUST also implement the following:

* When a Client is granted access, the Server MUST synchronously create a Grant object on the Grants API [[CDS-WG1-02 Section 8.1](#ref-cds-wg1-02-grant-object)] with the Scopes that were authorized or included, except as defined in individual Scope requirements (e.g. not including preselection fields for Scopes that require Customer authorization).
* For Grants that were created from authorization requests (i.e. the Customer authorization form was used), Servers MUST record the authorization's selections using the same type as the [Selection Component](#selection-component) used, so that the data access matches the selected entries by the Customer authorizing the access.
  For example, if the [Service Contract Selection Component](#contract-selection) was used to select Service Contracts in an authorization request for the [Meter List Scope](#scope-meter-list), and the access is granted to sync data for 1 year (`"sync_until": "P1Y"`), and then if there's a meter swap during that period, the Server MUST change which Meter Devices are accessible to the Client so as to match what the Customer authorized (i.e. all the Meter Devices under their selected Service Contracts).
* If the Server asynchronously makes data available to the Client (e.g. the Server has to internally query for the data then load it into Customer Data API cached objects), the Server MUST implement the following:
    * When the Grant object is initially created, the  MUST have the following default values:
        * The `status` value MUST be `"pending"`.
        * The `enabled_scope` value MUST be an empty string (`""`).
        * The `enabled_authorization_details` value MUST be an empty array (`[]`).
        * The `eta` value MUST NOT be `null`.
    * During asynchronous data loading, if some data objects are accessible but others are not (e.g. the Account objects are available, but the Usage Segments are still being loaded), it is RECOMMENDED that the Server update the Grant's `enabled_scope` and `enabled_authorization_details` values to be the appropriate values representing which data is now accessible.
      That way, Client's can start requesting and processing data from the Customer Data APIs while the Server continues to make available the rest of the granted data.
    * During asynchronous data loading, if the estimated time for completing the asynchronous tasks changes (e.g. there is an unexpected delay in the queries for data internally), the Server MUST update the Grant's `eta` value with the Server's new estimate for when its asynchronous tasks will be completed.
    * When the Server completes its asynchronous tasks for making data available, the Server MUST update the `status` value from `pending` to the appropriate status value based on the asynchronous task's results.
      The Server MUST also update the `enabled_scope` and `enabled_authorization_details` values to the appropriate access now available to the Client.

Additionally, the Server MAY include other [Authorization Details Fields](#auth-details-fields) beyond what is required in the `authorization_details_types_supported` array as needed to support the Server's use cases.

### 6.1. Customer Consent Scopes <a id="scopes-customer-consent" href="#scopes-customer-consent" class="permalink">🔗</a>

The Scopes defined in this section proves a means by which a Client can request authorization from a Customer, for situations where Customer consent is required to be able to access data on the Server.
The Customer authorization process uses the Server's OAuth Authorization Code Grant flow [[RFC 6749 Section 4.1](#ref-rfc6749-code-grant)], so these the Scopes in this section require at least the `"code"` response type to be available in the Server's Scope Description object for the supported Scope.

In addition to defining the scopes in this section, this specification also defines [Customer Authorization](#customer-authorizations) interface requirements (authorization form layout, error pages, etc.).

#### 6.1.1. Rate Plan <a id="scope-rate-plan" href="#scope-rate-plan" class="permalink">🔗</a>

For some use cases, a Client only needs to obtain a Customer's rate plans (i.e. the tariffs they are assigned for their utility services).
For example, a demand response app Client may need the rate plan for a Customer in order to determine if the Customer has any services that could join a demand response aggregation program.
In these relevant use cases, Clients only need access to Customers' rate plans, so this Scope provides the ability for Servers to limit access to only rate plan related fields.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_rate_plan"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_types"`](#auth-details-service-types)
    * [`"include_contract_numbers"`](#auth-details-include-contract-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_accounts"`](#auth-details-include-accounts)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"include_meter_devices"`](#auth-details-include-meter-devices)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be [`"service_contract_selection"`](#contract-selection).

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_service_contracts`](#auth-details-include-service-contracts)
    * [`include_rate_plans`](#auth-details-include-rate-plans)
* The Server MUST make available the Service Contract objects that were selected by the Customer during authorization.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.2. Account Program Participation <a id="scope-account-program-participation" href="#scope-account-program-participation" class="permalink">🔗</a>

For some use cases, a Client only needs to obtain a list of which [Account-level programs](#TODO-account-program-format) which apply to a Customer.
For example, a utility rebate program contractor Client may need to know if a Customer is on low-income assistance for their  utility account in order to know if they are qualified for a specific set of utility rebates.
In these relevant use cases, Clients only need access to Customers' relevant account-level programs, so this Scope provides the ability for Servers to limit access to only [Account](#account-format) `account_programs`.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_account_programs"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"account_programs"`](#auth-details-account-programs)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"addresses"`](#auth-details-addresses)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be [`"account_selection"`](#account-selection).

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST reject Client authorization requests with an `invalid_authorization_details` error [[RFC 9396 Section 5](#ref-rfc9396-error-response)] where the `account_programs` authorization details field is set to `null`, meaning that Clients MUST include which specific Account programs to which they are requesting access.
  Servers MAY set the `default` value for the `account_programs` field to a non-`null` value, so that Client requests have a default set of programs for which they are asking the Customer to authorize access.
* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_accounts`](#auth-details-include-accounts)
    * [`include_account_programs`](#auth-details-include-account-programs)
* The Server MUST make available the Account objects that were selected by the Customer during authorization.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST include the `account_programs` field in this scopes authorization details object in the Server response's and Grant object's `authorization_details` arrays.
* Except for the `account_programs` field, the Server MUST NOT include [Preselection Fields](#auth-details-preselection) in this scopes authorization details object in the Server response's and Grant object's `authorization_details` arrays.
* It is RECOMMENDED for the Server to have a Client-specific `cds_server_metadata` URL value for Client objects in the Client API [[CDS-WG1-02 Section 5.1](#ref-cds-wg1-02-grant-object)] where this scope is included in the Client object's `scope` value, rather than a generic public CDS Server Metadata endpoint.
  That way the Server can customize the list of Choice objects in the `choices` list for the `account_programs` Authorization Details Field object in this Scope's `authorization_details_types_supported` field, which lets the Server tailor which Program objects are available for sharing based on which Client is requesting access.

#### 6.1.3. Service Program Participation <a id="scope-service-program-participation" href="#scope-service-program-participation" class="permalink">🔗</a>

For some use cases, a Client only needs to obtain a list of which [Service Contract-level programs](#TODO-service-program-format) which apply to a Customer.
For example, a demand response app Client may need to know if an enterprise Customer is already signed up for a demand response program for their buildings.
In these relevant use cases, Clients only need access to Customers' relevant service-level programs, so this Scope provides the ability for Servers to limit access to only [Service Contract](#service-contract-format) `service_programs`.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_service_programs"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_programs"`](#auth-details-service-programs)
    * [`"service_types"`](#auth-details-service-types)
    * [`"include_contract_numbers"`](#auth-details-include-contract-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_accounts"`](#auth-details-include-accounts)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"include_rate_plans"`](#auth-details-include-rate-plans)
    * [`"include_meter_devices"`](#auth-details-include-meter-devices)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be -[`"service_contract_selection"`](#contract-selection).

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST reject Client authorization requests with an `invalid_authorization_details` error [[RFC 9396 Section 5](#ref-rfc9396-error-response)] where the `service_programs` authorization details field is set to `null`, meaning that Clients MUST include which specific Service Contract programs to which they are requesting access.
  Servers MAY set the `default` value for the `service_programs` field to a non-`null` value, so that Client requests have a default set of programs for which they are asking the Customer to authorize access.
* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_service_contracts`](#auth-details-include-service-contracts)
    * [`include_service_programs`](#auth-details-include-service-programs)
* The Server MUST make available the Service Contract objects that were selected by the Customer during authorization.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST include the `service_programs` field in this scopes authorization details object in the Server response's and Grant object's `authorization_details` arrays.
* Except for the `service_programs` field, the Server MUST NOT include [Preselection Fields](#auth-details-preselection) in this scopes authorization details object in the Server response's and Grant object's `authorization_details` arrays.
* It is RECOMMENDED for the Server to have a Client-specific `cds_server_metadata` URL value for Client objects in the Client API [[CDS-WG1-02 Section 5.1](#ref-cds-wg1-02-grant-object)] where this scope is included in the Client object's `scope` value, rather than a generic public CDS Server Metadata endpoint.
  That way the Server can customize the list of Choice objects in the `choices` list for the `service_programs` Authorization Details Field object in this Scope's `authorization_details_types_supported` field, which lets the Server tailor which Program objects are available for sharing based on which Client is requesting access.

#### 6.1.4. Account List <a id="scope-account-list" href="#scope-account-list" class="permalink">🔗</a>

For some use cases, a Client needs access to the Customer's account list.
For example, a financial auditor Client may need to know the full list of Accounts for an enterprise Customer as part of their accounting audit of the enterprise's organization.
In these relevant use cases, Clients only need access to a Customer's list of Accounts, so this Scope provides the ability for Servers to limit access to only Account objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_accounts"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_account_details"`](#auth-details-include-account-details)
    * [`"include_account_programs"`](#auth-details-include-account-programs)
    * [`"include_aggregations"`](#auth-details-include-aggregations)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be [`"account_selection"`](#account-selection).

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_accounts`](#auth-details-include-accounts)
* The Server MUST make available the Account objects that were selected by the Customer during authorization.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.5. Service List <a id="scope-service-list" href="#scope-service-list" class="permalink">🔗</a>

For some use cases, a Client needs access to the Customer's list of Service Contracts (e.g. their list of electric and gas services from a utility).
For example, an energy consultant Client working with an enterprise Customer may need to review which of the business's buildings have which utility services.
In these relevant use cases, Clients only need access to a Customer's list of Service Contracts, so this Scope provides the ability for Servers to limit access to only Service Contract objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_service_contracts"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_types"`](#auth-details-service-types)
    * [`"include_contract_numbers"`](#auth-details-include-contract-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"include_account_details"`](#auth-details-include-account-details)
    * [`"include_account_programs"`](#auth-details-include-account-programs)
    * [`"include_rate_plans"`](#auth-details-include-rate-plans)
    * [`"include_service_contract_details"`](#auth-details-include-contract-details)
    * [`"include_service_programs"`](#auth-details-include-service-programs)
    * [`"include_meter_devices"`](#auth-details-include-meter-devices)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
    * [`"include_aggregations"`](#auth-details-include-aggregations)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain objects with the following `id` values:
            * [`"service_contract_selection"`](#contract-selection)
        * The `choices` array MAY contain objects with the following `id` values:
            * [`"account_selection"`](#account-selection)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_accounts`](#auth-details-include-accounts)
    * [`include_service_contracts`](#auth-details-include-service-contracts)
* The Server MUST make available the Service Contract objects that were selected by the Customer during authorization.
    * If the Account Selection Component was used to select Accounts for this scope, selected Service Contracts are any with `cds_account_id` values that match the selected Accounts from the Account Selection Component.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.6. Meter List <a id="scope-meter-list" href="#scope-meter-list" class="permalink">🔗</a>

For some use cases, a Client needs access to a Customer's list of Meters Devices.
For example, an energy auditor Client working with an enterprise Customer may need to review which of the meters are at which building for the utility.
In these relevant use cases, Clients only need access to a Customer's list of Service Contracts, so this Scope provides the ability for Servers to limit access to only Service Contract objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_meters"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"meter_types"`](#auth-details-meter-types)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_types"`](#auth-details-service-types)
    * [`"servicepoint_numbers"`](#auth-details-servicepoint-numbers)
    * [`"servicepoint_types"`](#auth-details-servicepoint-types)
    * [`"premise_numbers"`](#auth-details-premise-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_accounts"`](#auth-details-include-accounts)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"include_account_details"`](#auth-details-include-account-details)
    * [`"include_account_programs"`](#auth-details-include-account-programs)
    * [`"include_service_contracts"`](#auth-details-include-service-contracts)
    * [`"include_contract_numbers"`](#auth-details-include-contract-numbers)
    * [`"include_rate_plans"`](#auth-details-include-rate-plans)
    * [`"include_service_contract_details"`](#auth-details-include-contract-details)
    * [`"include_service_programs"`](#auth-details-include-service-programs)
    * [`"include_service_points"`](#auth-details-include-service-points)
    * [`"include_premises"`](#auth-details-include-premises)
    * [`"include_service_point_addresses"`](#auth-details-include-service-point-addresses)
    * [`"include_coordinates"`](#auth-details-include-coordinates)
    * [`"include_aggregations"`](#auth-details-include-aggregations)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain objects with the following `id` values:
            * [`"meter_device_selection"`](#meter-selection)
        * The `choices` array MAY contain objects with the following `id` values:
            * [`"service_contract_selection"`](#contract-selection)
            * [`"account_selection"`](#account-selection)
            * [`"servicepoint_selection"`](#servicepoint-selection)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_meter_devices`](#auth-details-include-meter-devices)
* When rendering the [Service Contract Selection](#contract-selection) Component, the Server MUST include the list of Meter Devices that will be shared under each Service Contract entry that is able to be selected.
  This is to make sure the Customer knows which meters they are sharing for this authorization request.
* The Server MUST make available the Meter Device objects that were selected by the Customer during authorization.
    * If the Account Selection Component was used to select Accounts for this scope, selected Meter Devices are any that have a Service Point listed the Meter Device's `current_servicepoints` and has a Service Contract listed in that Service Point's `current_servicecontracts` that also has a selected Account set as the Service Contract's `cds_account_id`.
    * If the Service Contract Selection Component was used to select Service Contracts for this scope, selected Meter Devices are any that have a Service Point listed the Meter Device's `current_servicepoints` that also has a selected Service Contract listed in the Service Point's `current_servicecontracts`.
    * If the Service Point Selection Component was used to select Service Points for this scope, selected Meter Devices are any that have a selected Service Point listed the Meter Device's `current_servicepoints`.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields.
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.7. Bill Statements <a id="scope-bill-statements" href="#scope-bill-statements" class="permalink">🔗</a>

For some use cases, a Client needs access to a Customer's Bill Statements.
For example, a financial auditor Client may need to review a Customer's utility bills as part of a business's financial audit.
In these relevant use cases, Clients need access to a Customer's set of Bill Statements, so this Scope provides the ability for Servers to offer tailored access to Bill Statement objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_bill_statements"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"statement_date_start"`](#auth-details-statement-start)
    * [`"statement_date_end"`](#auth-details-statement-end)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_bill_statement_files"`](#auth-details-include-bill-statement-files)
    * [`"include_bill_statement_charges"`](#auth-details-include-bill-statement-charges)
    * [`"include_bill_statement_programs"`](#auth-details-include-bill-statement-programs)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be [`"account_selection"`](#account-selection).

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_bill_statements`](#auth-details-include-bill-statements)
    * [`include_accounts`](#auth-details-include-accounts)
    * [`include_account_numbers`](#auth-details-include-account-numbers)
* The Server MUST make available the Bill Statement objects that have a `cds_account_id` value that matches the Accounts selected by the Customer during authorization.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields (e.g. `include_bill_statement_files`) and MUST NOT include any fields or objects that are constrained by the Grant's authorization details fields (e.g. `statement_date_start`).
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.8. Service Bills <a id="scope-service-bills" href="#scope-service-bills" class="permalink">🔗</a>

For some use cases, a Client needs access to a breakdown of bill charges for a Customer's set of Service Contracts.
For example, an energy service company Client that is working with an enterprise Customer may need to analyze the historical energy charges for some of the Customer's utility services for a building.
In these relevant use cases, Clients need access to a Customer's set of Bill Sections, so this Scope provides the ability for Servers to offer tailored access to Bill Section objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_service_bills"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_types"`](#auth-details-service-types)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"start_date"`](#auth-details-bill-section-start)
    * [`"end_date"`](#auth-details-bill-section-end)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"statement_date_start"`](#auth-details-statement-start)
    * [`"statement_date_end"`](#auth-details-statement-end)
    * [`"include_bill_statements"`](#auth-details-include-bill-statements)
    * [`"include_bill_statement_files"`](#auth-details-include-bill-statement-files)
    * [`"include_bill_statement_charges"`](#auth-details-include-bill-statement-charges)
    * [`"include_bill_statement_programs"`](#auth-details-include-bill-statement-programs)
    * [`"include_bill_section_line_items"`](#auth-details-include-bill-section-line-items)
    * [`"include_aggregations"`](#auth-details-include-aggregations)
    * [`"include_service_points"`](#auth-details-include-service-points)
    * [`"include_meter_devices"`](#auth-details-include-meter-devices)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain objects with the following `id` values:
            * [`"service_contract_selection"`](#contract-selection)
            * [`"account_selection"`](#account-selection)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_bill_sections`](#auth-details-include-bill-sections)
    * [`include_accounts`](#auth-details-include-accounts)
    * [`include_account_numbers`](#auth-details-include-account-numbers)
    * [`include_service_contracts`](#auth-details-include-service-contracts)
    * [`include_contract_numbers`](#auth-details-include-contract-numbers)
* If the Customer used the [Account Selection](#account-selection) Component for an authorization Grant, the Server MUST treat the Accounts that were selected as the basis for determining which objects to include.
    * Included Service Contracts are those which have a selected Account as the Service Contract's `cds_account_id` value.
    * Included Bill Sections are those which have a selected Account as the Bill Section's `cds_account_id` value.
* If the Customer used the [Service Contract Selection](#contract-selection) Component for an authorization Grant, the Server MUST treat the Service Contracts that were selected as the basis for determining which objects to include.
    * Included Accounts are those which are referenced by a selected Service Contract's `cds_account_id` value.
    * Included Bill Sections are those which have a selected Service Contract listed in the Bill Section's `related_servicecontracts` array and an included Account referenced in the Bill Section's `cds_account_id`.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields (e.g. `include_bill_section_line_items`) and MUST NOT include any fields or objects that are constrained by the Grant's authorization details fields (e.g. `start_date`).
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.9. Meter Usage <a id="scope-meter-usage" href="#scope-meter-usage" class="permalink">🔗</a>

For some use cases, a Client needs access to the usage data for a Customer's set of Meter Devices.
For example, a building energy management platform Client being used by an enterprise Customer may need to collect and monitor the meter usage for the Customer's properties.
In these relevant use cases, Clients need access to a Customer's Usage Segments, so this Scope provides the ability for Servers to offer tailored access to Usage Segment objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_usage"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"include_usage_segment_formats"`](#auth-details-include-usage-segment-formats)
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"segment_start"`](#auth-details-usage-segment-start)
    * [`"segment_end"`](#auth-details-usage-segment-end)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"merge_selection_with"`](#auth-details-merge-selections)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The `authorization_details_types_supported` array MUST contain any the following values and support the authorization details field when the Server has access to the data that allows for support of that field:
    * [`"account_numbers"`](#auth-details-account-numbers)
    * [`"contract_numbers"`](#auth-details-contract-numbers)
    * [`"service_types"`](#auth-details-service-types)
    * [`"servicepoint_numbers"`](#auth-details-servicepoint-numbers)
    * [`"servicepoint_types"`](#auth-details-servicepoint-types)
    * [`"premise_numbers"`](#auth-details-premise-numbers)
    * [`"meter_numbers"`](#auth-details-meter-numbers)
    * [`"meter_types"`](#auth-details-meter-types)
    * [`"aggregation_numbers"`](#auth-details-aggregation-numbers)
    * [`"addresses"`](#auth-details-addresses)
    * [`"include_accounts"`](#auth-details-include-accounts)
    * [`"include_account_numbers"`](#auth-details-include-account-numbers)
    * [`"include_service_contracts"`](#auth-details-include-service-contracts)
    * [`"include_contract_numbers"`](#auth-details-include-contract-numbers)
    * [`"include_service_points"`](#auth-details-include-service-points)
    * [`"include_premises"`](#auth-details-include-premises)
    * [`"include_service_point_addresses"`](#auth-details-include-service-point-addresses)
    * [`"include_coordinates"`](#auth-details-include-coordinates)
    * [`"include_meter_devices"`](#auth-details-include-meter-devices)
    * [`"include_meter_numbers"`](#auth-details-include-meter-numbers)
    * [`"include_aggregations"`](#auth-details-include-aggregations)
    * [`"include_aggregation_addresses"`](#auth-details-include-aggregation-addresses)
    * [`"include_bill_sections"`](#auth-details-include-bill-sections)
    * [`"include_bill_section_line_items"`](#auth-details-include-bill-section-line-items)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain at least one object and objects MUST have one of following `id` values:
            * [`"account_selection"`](#account-selection)
            * [`"service_contract_selection"`](#contract-selection)
            * [`"servicepoint_selection"`](#servicepoint-selection)
            * [`"meter_device_selection"`](#meter-selection)
            * [`"aggregation_selection"`](#aggregation-selection)
    * If the Server includes `account_selection` as a choice in the `authorization_form_selection_type` authorization details object, the Server MUST include the following values in the `authorization_details_types_supported` array:
        * `"account_numbers"`
        * `"include_accounts"`
        * `"include_account_numbers"`
    * If the Server includes `"service_contract_selection"` as a choice in the `authorization_form_selection_type` authorization details object, the Server MUST include the following values in the `authorization_details_types_supported` array:
        * `"account_numbers"`
        * `"contract_numbers"`
        * `"include_accounts"`
        * `"include_account_numbers"`
        * `"include_service_contracts"`
        * `"include_contract_numbers"`
    * If the Server includes `"servicepoint_selection"` as a choice in the `authorization_form_selection_type` authorization details object, the Server MUST include the following values in the `authorization_details_types_supported` array:
        * `"servicepoint_numbers"`
        * `"include_service_points"`
    * If the Server includes `"meter_device_selection"` as a choice in the `authorization_form_selection_type` authorization details object, the Server MUST include the following values in the `authorization_details_types_supported` array:
        * `"meter_numbers"`
        * `"include_meter_devices"`
        * `"include_meter_numbers"`
    * If the Server includes `"aggregation_selection"` as a choice in the `authorization_form_selection_type` authorization details object, the Server MUST include the following values in the `authorization_details_types_supported` array:
        * `"aggregation_numbers"`
        * `"include_aggregations"`

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_usage_segments`](#auth-details-include-bill-sections)
* If the Customer used the [Account Selection](#account-selection) Component for an authorization Grant, the Server MUST treat the Accounts that were selected as the basis for determining which objects to include.
    * Included Usage Segments are those which have a selected Account listed in the Usage Segment's `related_accounts` array.
* If the Customer used the [Service Contract Selection](#contract-selection) Component for an authorization Grant, the Server MUST treat the Service Contracts that were selected as the basis for determining which objects to include.
    * Included Usage Segments are those which have a selected Service Contract listed in the Usage Segment's `related_servicecontracts` array.
* If the Customer used the [Service Point Selection](#servicepoint-selection) Component for an authorization Grant, the Server MUST treat the Service Points that were selected as the basis for determining which objects to include.
    * Included Usage Segments are those which have a selected Service Points listed in the Usage Segment's `related_servicepoints` array.
* If the Customer used the [Meter Device Selection](#meter-selection) Component for an authorization Grant, the Server MUST treat the Meter Devices that were selected as the basis for determining which objects to include.
    * Included Usage Segments are those which have a selected Meter Devices listed in the Usage Segment's `related_meterdevices` array.
* If the Customer used the [Aggregations Selection](#aggregation-selection) Component for an authorization Grant, the Server MUST treat the Aggregations that were selected as the basis for determining which objects to include.
    * Included Usage Segments are those which have a selected Aggregations listed in the Usage Segment's `related_aggregations` array.
* The Server MUST include any fields or objects as set by the Grant's authorization details fields (e.g. `include_contract_numbers`) and MUST NOT include any fields or objects that are constrained by the Grant's authorization details fields (e.g. `segment_start`).
* The Server MUST NOT include [Preselection Fields](#auth-details-preselection) in the Server response's and Grant object's `authorization_details` arrays.

#### 6.1.10. Aggregation Inclusion <a id="scope-aggregation-inclusion" href="#scope-aggregation-inclusion" class="permalink">🔗</a>

For some use cases, a Client needs a Customer to approve being included in an Aggregation.
For example, a building owner Client needs to get the aggregated energy usage for their property, but the number of tenants in the property falls below a privacy threshold and individual tenant Customer consent is required, so the Server needs the Client to request individual authorizations from Customers.
In these relevant use cases, Clients need to be able to obtain an authorization from a Customer to be included in an Aggregation, so this Scope provides the ability for Servers to perform these authorization requests.

Typically, this scope is used when a Client requests access to Usage Data via the [`cds_query_usage`](#scope-usage-query) scope with an [`aggregation_numbers`](#auth-details-aggregation-numbers) authorization details field value (e.g. request whole-building energy usage for two buildings, which have aggregation numbers `B1111` and `B2222`), but the requested Aggregations require consent from Customers before releasing the Usage Segments.
In these cases, the Server will create a Grant for the `cds_query_usage` scope, set that Grant status to `needs_sub_grants`, create sub-Grants for the individual Customer authorizations required, set those Grant scopes to `cds_aggregation_consent` and statuses of `needs_authorization`.
This allows the Client to detect that individual Customer authorizations are required and use the Grant Authorization Request process [[CDS-WG1-02 Section 8.3](#ref-cds-wg1-02-grant-auth-requests)] to request authorization from relevant Customers.
Then, when all relevant Customers have authorized the sub-Grants (which have the `cds_aggregation_consent` scope), the Server can update the original Grant status to `active` and release the Usage Segments to the Client.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_aggregation_consent"`.
* The `response_types_supported` value must contain at least the value `"code"`.
* The `grant_types_supported` value MUST contain at least the values `"authorization_code"` and `"refresh_token"` and MUST NOT contain the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"aggregation_numbers"`](#auth-details-aggregation-numbers)
    * [`"expires"`](#auth-details-expires)
    * [`"authorization_form_selection_type"`](#auth-details-selection-type)
    * [`"error_if_no_preselections"`](#auth-details-error-if-no-preselections)
    * [`"allow_scope_modifications"`](#auth-details-allow-scope-modifications)
* The following is required for Authorization Details Field Objects in the Scope Description's `authorization_details_fields_supported` field:
    * For the object with an `id` value of `authorization_form_selection_type`, it MUST meet the following requirements:
        * The `choices` array MUST contain only one object, and that object MUST have its `id` value be [`"aggregation_selection"`](#aggregation-selection).
    * For the object with an `id` value of `error_if_no_preselections`, it MUST meet the following requirements:
        * The `default` value MUST be set to `true`.
    * For the object with an `id` value of `allow_scope_modifications`, it MUST meet the following requirements:
        * The `default` value MUST be set to `false`.

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST reject Client authorization requests with an `invalid_authorization_details` error [[RFC 9396 Section 5](#ref-rfc9396-error-response)] where the `aggregation_numbers` authorization details field is set to `null`, meaning that Clients MUST specify which Aggregations for which they are requesting the Customer to authorize inclusion.
* The Server MUST reject Client authorization requests with an `invalid_authorization_details` error [[RFC 9396 Section 5](#ref-rfc9396-error-response)] where the `error_if_no_preselections` authorization details field is set to `false`, meaning that Customer MUST have an applicable Aggregation in the `aggregation_numbers` included in the authorization request's authorization details.
* The Server MUST reject Client authorization requests with an `invalid_authorization_details` error [[RFC 9396 Section 5](#ref-rfc9396-error-response)] where the `allow_scope_modifications` authorization details field is set to `true`, meaning that the Customer MUST NOT be able to edit the authorization request's scope, since the request is specifically for requesting consent to be included in a set of Aggregations.

### 6.2. Direct Access Scopes <a id="scopes-direct-access" href="#scopes-direct-access" class="permalink">🔗</a>

The Scopes defined in this section proves a means by which a Client can directly query the Server for Customer Data, for situations where the Client does not first need Customer consent before accessing the Customer Data.
For the Scopes defined in this section, the Client uses the Server's OAuth Client Credentials Grant flow [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)], so these the Scopes in this section require at least the `"client_credentials"` grant type to be available in the Server's Scope Description object for the supported Scope.

The following are examples of situations where Customer Data access can be provided by a utility without Customer consent:

* Situations where the Customer is themselves registered as a Client, and using that Client to query and access their own Customer Data.
  For example, a large datacenter owner (the Client) wants to automate retrieving their own interval energy usage from the utility (the Server) on a regular basis.
* Situations where the Client is a directly contracted vendor and has met the utility's security requirements, so their data access is considered "internal" to the utility, and thus Customer consent is not required for access.
  For example, a utility vendor (the Client) needs to download a limited set of energy usage data for a subset of accounts in order to analyze as part of their contracted project with the utility.
* Situations where the Client is accessing data that has been generated or aggregated or properly anonymized such that individual Customer consent is not required by local regulatory requirements.
  For example, an academic institution (the Client) may be able to access energy usage that has been aggregated across a large region of the utility's territory (the Server).


For direct access Scopes defined in this section, the Server MUST implement the following:

* If any [Preselection Fields](#auth-details-preselection) are included in a Grant's authorization details, the Server MUST constrain responses to only contain objects matching those preselection fields.

For direct access Scopes defined in this section, the Server MAY impose additional request restrictions for Clients that further limit access beyond what is defined in this section.
The following are some examples of possible additional restrictions:

* If the Server has limited the Client to only be able to access objects for Grants with [Preselection Fields](#auth-details-preselection), the Server MAY reject Client Credentials requests [[RFC 6749 Section 4.4](#ref-rfc6749-client-credentials)] that do not have or have invalid Preselection Field values.
  For example, if a utility vendor Client is only permitted to access [Accounts](#account-api) for which they have obtained the Account number (e.g. as part of a rebate program verification contract), the utility Server can limit the vendor's Client by requiring they always include the `account_numbers` preselection field in Client Credentials requests, so if the vendor Client requests to create an access token that is unbounded (i.e. no `account_numbers` included), the Server is able to reject the request.
* If the Server wants to limit a Client's ability to enumerate all objects on an API, the Server MAY return an error response for API requests that do not have granular enough request parameters.
  For example, if a building owner Client has been given the ability to search a Server's list of building identifiers (stored by the server as [Aggregation](#aggregation-format) objects with the building ID as the `aggregation_number`) for their own building, the Server can limit the [Aggregations listing API](#aggregation-list) to only respond successfully to requests that use request parameters (e.g. `q={building_address}`) and return a limited number of results, and return an error response if there are too many results.
  By imposing this limitation, in combination with [Rate Limiting](#TODO-rate-limiting), the Client will not be able to enumerate all building IDs stored on the Server.

#### 6.2.1. Accounts Query <a id="scope-accounts-query" href="#scope-accounts-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Accounts.
For example, an enterprise Customer, acting as the Client, may need to review their list of utility accounts from the utility, acting as the Server.
As another example, a utility vendor Client may need to search for a specific Account in the utility's Customer list.
In these relevant use cases, Clients need access to a set of Account objects directly, so this Scope provides the ability for Servers to offer managed access to Account objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_accounts"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_accounts`](#auth-details-include-accounts)

#### 6.2.2. Service Contracts Query <a id="scope-service-contracts-query" href="#scope-service-contracts-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Service Contracts.
For example, a utility vendor Client working with the utility Server may need to be able to look up a specific set of utility services for analysis as part of their project with the utility.
In these relevant use cases, Clients need access to a set of Service Contract objects directly, so this Scope provides the ability for Servers to offer managed access to Service Contract objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_service_contracts"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_accounts`](#auth-details-include-accounts)
    * [`include_service_contracts`](#auth-details-include-service-contracts)

#### 6.2.3. Service Points Query <a id="scope-service-points-query" href="#scope-service-points-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Service Points.
For example, a utility vendor Client working with the utility Server may need to be able to look up a specific set of service locations in order to create a map as part of their project with the utility.
In these relevant use cases, Clients need access to a set of Service Point objects directly, so this Scope provides the ability for Servers to offer managed access to Service Point objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_service_points"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_service_points`](#auth-details-include-service-points)

#### 6.2.4. Meter Devices Query <a id="scope-meter-devices-query" href="#scope-meter-devices-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Meter Devices.
For example, a utility vendor Client working with the utility Server may need to be able to look up if a meter exists for a given `meter_number` in the utility's system.
In these relevant use cases, Clients need access to a set of Meter Device objects directly, so this Scope provides the ability for Servers to offer managed access to Meter Device objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_meter_devices"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_meter_devices`](#auth-details-include-meter-devices)

#### 6.2.5. Bill Statements Query <a id="scope-bill-statements-query" href="#scope-bill-statements-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Bill Statements.
For example, a technical support contractor Client working with the utility Server may need to look up a Customer's bills while while investigating a customer support ticket.
In these relevant use cases, Clients need access to a set of Bill Statement objects directly, so this Scope provides the ability for Servers to offer managed access to Bill Statement objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_bill_statements"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"statement_date_start"`](#auth-details-statement-start)
    * [`"statement_date_end"`](#auth-details-statement-end)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_bill_statements`](#auth-details-include-bill-statements)

#### 6.2.6. Bill Sections Query <a id="scope-bill-sections-query" href="#scope-bill-sections-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Bill Sections.
For example, a technical support contractor Client working on customer support ticket for the utility Server may need to look up a Customer's charges for a specific service to understand why a Customer was charged a certain amount for their electric service last month.
In these relevant use cases, Clients need access to a set of Bill Section objects directly, so this Scope provides the ability for Servers to offer managed access to Bill Section objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_bill_sections"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"start_date"`](#auth-details-bill-section-start)
    * [`"end_date"`](#auth-details-bill-section-end)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_bill_sections`](#auth-details-include-bill-sections)

#### 6.2.7. Aggregations Query <a id="scope-aggregations-query" href="#scope-aggregations-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Aggregations.
For example, an academic researcher Client that needs to look up aggregated energy usage on a utility Server would need to query the list of available regional aggregations available for download.
Another example is a building owner Client needing to search for their property's building identifier, which the Server is storing as an Aggregation with the `aggregation_number` as the building ID, so that the owner can download the whole-building energy usage for benchmarking reporting.
In these relevant use cases, Clients need access to a set of Aggregation objects directly, so this Scope provides the ability for Servers to offer managed access to Aggregation objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_aggregations"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_aggregations`](#auth-details-include-aggregations)

#### 6.2.8. Usage Query <a id="scope-usage-query" href="#scope-usage-query" class="permalink">🔗</a>

For some use cases, a Client needs to be able to access a list of Usage Segments.
For example, an enterprise Customer, acting as the Client, needs to access their latest month of energy usage from the utility Server so that they can run their monthly energy report for the company's leadership.
In these relevant use cases, Clients need access to a set of Usage Segment objects directly, so this Scope provides the ability for Servers to offer managed access to Usage Segment objects.

To support this Scope, the Scope Description object MUST meet the following requirements:

* The `type` value MUST be `"cds_query_usage"`.
* The `response_types_supported` value must be an empty array (`[]`).
* The `grant_types_supported` value MUST contain at least the value `"client_credentials"`.
* The `authorization_details_types_supported` array MUST contain at least the following values and support those authorization details fields as defined in the [Authorization Details Fields](#auth-details-fields) section:
    * [`"sync_until"`](#auth-details-sync-until)
    * [`"segment_start"`](#auth-details-usage-segment-start)
    * [`"segment_end"`](#auth-details-usage-segment-end)

Additionally, to support this Scope, the Server MUST implement the following requirements:

* The Server MUST treat this Scope as having included `true` values for the following authorization details fields, so that the data defined by those fields is included (this is the default access granted by this Scope):
    * [`include_usage_segments`](#auth-details-include-bill-sections)

### 6.3. Scope Extensions <a id="scope-extensions" href="#scope-extensions" class="permalink">🔗</a>

While this specification defines [Scopes](#scopes) that can meet many use cases, extensions to this specifications are encouraged to modify Scopes or define additional Scopes to meet other use cases.
However, it is RECOMMENDED that if extensions or use cases can define their needs using existing Scopes defined in this specification, combined with authorization details requirements, that they define those authorization details requirements, rather than creating a new Scope with new requirements.
The [Scenarios](#scenarios) section of this specification are some examples of how to defined combined Scope and authorization details requirements.
This is so that Clients who support the Scopes and authorization details defined in this specification will already be compatible with extensions and new use cases.

Extensions that modify a Scope defined in this specification MUST define those modifications as a new Scope with a new Scope Type identifier that references the existing Scope in this specification followed by the description of modifications.
This is so that Clients do not mix up the Scope defined in this specification with the modified Scope defined in an extension.

It is RECOMMENDED that extensions that create new Scopes define them in the same same format structure as those defined in this specification.
This is so that Client developers who are familiar with this specification can easily add support for other Scopes in extensions.
The format for Scope definitions in this specification use the following structure:

* Brief explanation and example of the use cases the Scope is intending to address.
  This allows Clients to understand why this Scope exists.
* Definition of Scope Description [[CDS-WG1-02 Section 3.4](#ref-cds-wg1-02-scope-descriptions)] requirements.
  This allows Clients to understand how to use this Scope in their requests to Servers.
* Definition of Server access and behavior requirements.
  This allows Clients to understand what access and behavior Servers will provide when this Scope is part of a request or Grant.

## 7. Authorization Details Fields <a id="auth-details-fields" href="#auth-details-fields" class="permalink">🔗</a>

This section defines Authorization Details Fields that are available to be included in [Scope](#scopes) definitions.
When supported, these fields can be used in `authorization_details` parameters as part of OAuth's Rich Authorization Requests [[RFC 9396](#ref-rfc9396)] to define tailored authorization experiences and scopes of access that meet specific use case needs.

When a Server supports an Authorization Details Field defined by this section, the Server MUST include an Authorization Details Field Object [[CDS-WG1-02 Section 3.8](#ref-cds-wg1-02-auth-details-object)] for the field in the `authorization_details_fields_supported` array in the Server's relevant Scope Description objects [[CDS-WG1-02 Section 3.4](#ref-cds-wg1-02-scope-descriptions)].
This allows Clients to automate discovery of which Authorization Details Fields are supported when formatting authorization and token request parameters.

For all authorization details fields specified in this section, if the Server defines a default value for the field in the Authorization Details Field Object [[CDS-WG1-02 Section 3.8](#ref-cds-wg1-02-auth-details-object)], the Server MUST treat requests for scopes in which the authorization details field is available, but not included in the request, as if the authorization details field was included and has the default value.
Additionally, when Servers use an authorization details field's default value as part of issuing a Grant, the Server MUST include that value for the authorization field's value in the Token response's `authorization_details` array, so that the Client may see which values are used in that Grant's authorization details.
By including the default values used in each Grant, Servers MAY change the default values in their Scope Descriptions as needed because those default values are only intended for new Client requests and not for prior issued Grants.
Clients MUST use the authorization details values in a Grant to determine that Grant's scope and authorization details and treat default values in the Server's Scope Descriptions as the default values going forward and not representative of past default values.

Extensions of this specification MAY define additional Authorization Details Fields that define additional behavior for [Scope](#scopes) defined in this specification.
When defining additional Authorization Details Fields, it is RECOMMENDED to define them in the same format as those defined in this section.

### 7.1. Preselection Fields <a id="auth-details-preselection" href="#auth-details-preselection" class="permalink">🔗</a>

Use cases for customer data access, especially commercial use cases, frequently require that Clients to be granted limit access to a subset of the Customer's data.
In these situations, it is useful for a Client to "preselect" the specific accounts, contracts, devices, etc. for which the Customer needs to authorize access.

This section defines authorization details fields that allow a Client to set preselections for the Server's authorization form, so that Customer doesn't have to individually select the subset of accounts, contracts, devices, etc. that are relevant for the Client's needs.
Additionally, the preselection authorization details fields defined in this section MAY also be used by Clients to minimize and tailor scopes of access when making access token requests that do not require authorization (e.g. the `client_credentials` grant flow).

For all preselection fields defined in this section, the Server MUST implement the following requirements:

* Any matched objects for preselection MUST be able to be matched by the Client's base set of permissions.
  This means that if the Server has limited a Client to only having permissions to preselect a specific set of objects or fields, using preselection fields will not be able to extend beyond what the Server permits for that Client.
  For example, if a energy contractor Client is set to only have permission to preselect `premise_numbers` but not `account_numbers`, when the Client uses the `account_numbers` preselection field to try to match specific Accounts, the Server will act as if there were not preselection matches for those Accounts, even if the Server does have Accounts with matching `account_number` values stored in its system.
* If multiple preselection authorization details field are used (i.e. multiple values are not `null`), Servers MUST determine which resources are preselected by intersecting the preselected resources matched by each individual preselection authorization details field.
  If the preselected resources from each individual preselection authorization details field do not have any overlap, then the intersection is an empty set, and no resources are preselected.

#### 7.1.1. Preselect Account Numbers <a id="auth-details-account-numbers" href="#auth-details-account-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of Customer accounts.
For example, an energy services company may be working with a property management Customer to analyze the energy use for a specific subset of utility accounts that the Customer has specified.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Accounts](#account-format) or [Service Contract](#service-contract-format) with provided `account_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"account_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Accounts are preselected.
* If the value is an array and `"_include_future"` is included as a value in the array, the Account Selection's Include Future option, if present, is preselected.
* If the value is an array, Accounts are preselected that match their `account_number` to any of the array's values.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Service Contracts are preselected.
* If the value is an array and `"_include_future"` is included as a value in the array, the Service Contract Selection's Include Future option, if present, is preselected.
* If the value is an array, Service Contracts are preselected that match their `account_number` to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Account objects in API responses that do not match their `account_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Accounts that are included:
        * Aggregation `grouped_accounts`
        * Usage Segment `related_accounts`

#### 7.1.2. Preselect Account Programs <a id="auth-details-account-programs" href="#auth-details-account-programs" class="permalink">🔗</a>

For some use cases, a Client may need to request access to Customer account details for accounts related to a set of Account Programs.
For example, an energy rebate contractor may need to verify which, if any, of a Customer's accounts are part of a low-income program.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Accounts](#account-format) that have certain `account_programs` objects.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"account_programs"`.
* The `format` value MUST be `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that contain Account Program objects that have a matching `program_number` value to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Account objects in API responses that do not contain Account Program objects that have a matching `program_number` value to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Accounts that are included:
        * Aggregation `grouped_accounts`
        * Usage Segment `related_accounts`

#### 7.1.3. Preselect Contract Numbers <a id="auth-details-contract-numbers" href="#auth-details-contract-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of Customer service contracts.
For example, an energy auditor may be working with a commercial Customer to produce a report on the energy use for a specific set of electric services on a Customer's property.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Contract](#service-contract-format) with the provided `contract_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"contract_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Accounts are preselected.
* If the value is an array and `"_include_future"` is included as a value in the array, the Account Selection's Include Future option, if present, is preselected.
* If the value is an array, Accounts are preselected that have a Service Contract with a matching `contract_number` to any of the array's values that also has the Account as that Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Service Contracts are preselected.
* If the value is an array and `"_include_future"` is included as a value in the array, the Service Contract Selection's Include Future option, if present, is preselected.
* If the value is an array, Service Contracts are preselected that match their `contract_number` to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Contract objects in API responses that do not match their `contract_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Contracts that are included:
        * Service Point `current_servicecontracts`
        * Service Point `previous_servicecontracts`
        * Bill Section `related_servicecontracts`
        * Aggregation `grouped_servicecontracts`
        * Usage Segment `related_servicecontracts`

#### 7.1.4. Preselect Service Types <a id="auth-details-service-types" href="#auth-details-service-types" class="permalink">🔗</a>

For some use cases, a Client may need to request access to Customer contract details for specific types of services.
For example, an electrician may need details about a Customer's electric services, but not any gas or water services.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Contract](#service-contract-format) with the provided `service_type` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"service_types"`.
* The `format` value MUST be `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Service Contract with a matching `service_type` to any of the array's values that also has the Account as that Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that match their `service_type` to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Contract objects in API responses that do not match their `service_type` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Contracts that are included:
        * Service Point `current_servicecontracts`
        * Service Point `previous_servicecontracts`
        * Bill Section `related_servicecontracts`
        * Aggregation `grouped_servicecontracts`
        * Usage Segment `related_servicecontracts`

#### 7.1.5. Preselect Service Programs <a id="auth-details-service-programs" href="#auth-details-service-programs" class="permalink">🔗</a>

For some use cases, a Client may need to request access to Customer contract details for services related to a set of Service Programs.
For example, a demand response app may need to confirm which, if any, of a Customer's services are participating in a demand response program.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Contract](#service-contract-format) that have certain `service_programs` objects.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"service_programs"`.
* The `format` value MUST be `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that contain Service Program objects that have a matching `program_number` value to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Contract objects in API responses that do not contain Service Program objects that have a matching `program_number` value to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Contracts that are included:
        * Service Point `current_servicecontracts`
        * Service Point `previous_servicecontracts`
        * Bill Section `related_servicecontracts`
        * Aggregation `grouped_servicecontracts`
        * Usage Segment `related_servicecontracts`

#### 7.1.6. Preselect Service Point Numbers <a id="auth-details-servicepoint-numbers" href="#auth-details-servicepoint-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of Service Points.
For example, a utility vendor may need to run an analysis of energy usage over a specific set of Service Points provided by the utility.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Point](#service-point-format) with the provided `servicepoint_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"servicepoint_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Service Point with a matching `servicepoint_number` to any of the array's values and a Service Contract is listed in the Service Point's `current_servicecontracts` array that also has the Account set as the Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have a Service Point with a matching `servicepoint_number` to any of the array's values that also has the Service Contract listed in the Service Point's `current_servicecontracts`.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Service Points are preselected.
* If the value is an array, Service Points are preselected that match their `servicepoint_number` to any of the array's values.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that have Service Points in their `current_servicepoints` which have matching `servicepoint_number` values to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Point objects in API responses that do not match their `servicepoint_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Points that are included:
        * Meter Device `current_servicepoints`
        * Meter Device `previous_servicepoints`
        * Bill Section `related_servicepoints`
        * Aggregation `grouped_servicepoints`
        * Usage Segment `related_servicepoints`

#### 7.1.7. Preselect Service Point Types <a id="auth-details-servicepoint-types" href="#auth-details-servicepoint-types" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific types of Service Points.
For example, a utility vendor may need to run an analysis of energy usage over a Customer's electric service points.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Point](#service-point-format) with the provided `servicepoint_type` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"servicepoint_types"`.
* The `format` value MUST be `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Service Point with a matching `servicepoint_type` to any of the array's values and a Service Contract is listed in the Service Point's `current_servicecontracts` array that also has the Account set as the Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have a Service Point with a matching `servicepoint_type` to any of the array's values that also has the Service Contract listed in the Service Point's `current_servicecontracts`.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that match their `servicepoint_type` to any of the array's values.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that have Service Points in their `current_servicepoints` which have matching `servicepoint_type` values to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Point objects in API responses that do not match their `servicepoint_type` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Points that are included:
        * Meter Device `current_servicepoints`
        * Meter Device `previous_servicepoints`
        * Bill Section `related_servicepoints`
        * Aggregation `grouped_servicepoints`
        * Usage Segment `related_servicepoints`

#### 7.1.8. Preselect Premise Numbers <a id="auth-details-premise-numbers" href="#auth-details-premise-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific premises.
For example, an building benchmarking tool may need to run an analysis of energy usage for a specific list of premises.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Service Point](#service-point-format) with the provided `premise_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"premise_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Service Point with a matching `premise_number` to any of the array's values and a Service Contract is listed in the Service Point's `current_servicecontracts` array that also has the Account set as the Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have a Service Point with a matching `premise_number` to any of the array's values that also has the Service Contract listed in the Service Point's `current_servicecontracts`.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that match their `premise_number` to any of the array's values.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that have Service Points in their `current_servicepoints` which have matching `premise_number` values to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Service Point objects in API responses that do not match their `premise_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Service Points that are included:
        * Meter Device `current_servicepoints`
        * Meter Device `previous_servicepoints`
        * Bill Section `related_servicepoints`
        * Aggregation `grouped_servicepoints`
        * Usage Segment `related_servicepoints`

#### 7.1.9. Preselect Meter Numbers <a id="auth-details-meter-numbers" href="#auth-details-meter-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of Meter Devices.
For example, a building energy app may need to run an analysis of energy usage over a specific set of Meter Devices for a Customer.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Meter Devices](#meter-device-format) with the provided `meter_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"meter_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Meter Device Point with a matching `meter_number` to any of the array's values and a Service Point is listed in the Meter Device's `current_servicepoints` array and Service Contract listed in the Service Point's `current_servicecontracts` array that also has the Account set as the Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have a Meter Device with a matching `meter_number` to any of the array's values and a Service Point is listed in the Meter Device's `current_servicepoints` array that also has the Service Contract listed in that Service Point's `current_servicecontracts`.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that have a Meter Device with a matching `meter_number` to any of the array's values and the Service Point is listed in the Meter Device's `current_servicepoints` array.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array and `"_all"` is included as a value in the array, all Meter Devices are preselected.
* If the value is an array, Meter Devices are preselected that match their `meter_number` to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Meter Device objects in API responses that do not match their `meter_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Meter Devices that are included:
        * Bill Section `related_meterdevices`
        * Aggregation `grouped_meterdevices`
        * Usage Segment `related_meterdevices`

#### 7.1.10. Preselect Meter Device Types <a id="auth-details-meter-types" href="#auth-details-meter-types" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific type of Meter Devices.
For example, a demand response provider may need to analyze only meters that are bidirectional for a Customer.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Meter Devices](#meter-device-format) with the provided `meter_type` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"meter_types"`.
* The `format` value MUST be `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have a Meter Device Point with a matching `meter_type` to any of the array's values and a Service Point is listed in the Meter Device's `current_servicepoints` array and Service Contract listed in the Service Point's `current_servicecontracts` array that also has the Account set as the Service Contract's `cds_account_id`.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have a Meter Device with a matching `meter_type` to any of the array's values and a Service Point is listed in the Meter Device's `current_servicepoints` array that also has the Service Contract listed in that Service Point's `current_servicecontracts`.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that match their `meter_type` to any of the array's values.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that have a Meter Device with a matching `meter_type` to any of the array's values and the Service Point is listed in the Meter Device's `current_servicepoints` array.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Meter Device objects in API responses that do not match their `meter_type` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Meter Devices that are included:
        * Bill Section `related_meterdevices`
        * Aggregation `grouped_meterdevices`
        * Usage Segment `related_meterdevices`

#### 7.1.11. Preselect Aggregation Numbers <a id="auth-details-aggregation-numbers" href="#auth-details-premise-numbers" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of Aggregations.
For example, an energy auditor may need request access to a list of whole-building energy usage aggregrations for region to do a benchmarking analysis for the local municipality.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects a set of [Aggregations](#aggregation-format) with the provided `aggregation_number` values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"aggregation_numbers"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that have an Aggregation with a matching `aggregation_number` to any of the array's values and the Account is listed in the Aggregation's `grouped_accounts` array.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that have an Aggregation with a matching `aggregation_number` to any of the array's values and the Service Contract is listed in the Aggregation's `grouped_servicecontracts` array.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that have an Aggregation with a matching `aggregation_number` to any of the array's values and the Service Point is listed in the Aggregation's `grouped_servicepoints` array.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that have an Aggregation with a matching `aggregation_number` to any of the array's values and the Meter Device is listed in the Aggregation's `grouped_meterdevices` array.

When rendering [Aggregation Selections](#aggregation-selection) for this field's scope and this field is included, the Server MUST preselect Aggregations based on the following criteria:

* If the value is an array, Aggregations are preselected that match their `aggregation_number` to any of the array's values.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* If the value is `null`, this field has no effect on the filtering API responses.
* If the value is an array, the following filters MUST be applied for API responses:
    * Aggregation objects in API responses that do not match their `aggregation_number` to any of the array's values MUST NOT be included.
    * The following arrays MUST only include references to Aggregations that are included:
        * Aggregation `grouped_aggregations`
        * Usage Segment `related_aggregations`

#### 7.1.12. Preselect Addresses <a id="auth-details-addresses" href="#auth-details-addresses" class="permalink">🔗</a>

For some use cases, a Client may need to request access to datasets for a specific list of addresses.
For example, an energy benchmarking app may request access to the whole-building energy usage aggregration for a building using the building's address.
In these relevant use cases, Clients benefit from being able to configure an authorization request that preselects resources with the provided address values.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"addresses"`.
* The `format` value MUST be `"string_list_or_null"` or `"choice_list_or_null"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.
* This object MUST NOT be included in a Scope Description where the `response_types_supported` field is an empty array.
  This means that that limiting objects by address using this preselection field is not available on [Direct Access Scopes](#scopes-direct-access).

When rendering [Account Selections](#account-selection) for this field's scope and this field is included, the Server MUST preselect Accounts based on the following criteria:

* If the value is an array, Accounts are preselected that match their `account_address` to any of the array's values.

When rendering [Service Contract Selections](#contract-selection) for this field's scope and this field is included, the Server MUST preselect Service Contracts based on the following criteria:

* If the value is an array, Service Contracts are preselected that match their `contract_address` to any of the array's values.

When rendering [Service Point Selections](#servicepoint-selection) for this field's scope and this field is included, the Server MUST preselect Service Points based on the following criteria:

* If the value is an array, Service Points are preselected that match their `servicepoint_address` to any of the array's values.

When rendering [Meter Device Selections](#meter-selection) for this field's scope and this field is included, the Server MUST preselect Meter Devices based on the following criteria:

* If the value is an array, Meter Devices are preselected that have a Service Point with a matching `servicepoint_address` to any of the array's values and the Service Point is listed in the Meter Devices's `current_servicepoints` array.

When rendering [Aggregation Selections](#aggregation-selection) for this field's scope and this field is included, the Server MUST preselect Aggregations based on the following criteria:

* If the value is an array, Aggregations are preselected that match any of their `addresses` values to any of the array's values.

##### 7.1.12.1. Address Matching <a id="auth-details-address-matching" href="#auth-details-address-matching" class="permalink">🔗</a>

Other preselection authorization details fields that have a `string_list_or_null` format as an option (e.g. `account_numbers`)  are matched by exact string matching. However, address string syntax is often difficult to reliably match when only using exact string matching.

To help mitigate some of the difficulties in Clients being able to effectively use the `addresses` preselection authorization details field, this specification requires that Servers MUST implement the following when including this authorization details field:

* For each address value that could be matched by this authorization details field, Servers MUST categorize the address value as a Typical Address or an Atypical Address.
    * Typical Addresses are defined as address values that Customers would largely consider to be able to be used in common mapping services in the Customers' locales. For example, `"123 Main St #123, Anytown, CA"` would be considered to be a Typical Address in the United States locale.
    * Atypical Addresses is any format of address string that is still valid for the Server's uses, but is not a Typical Address. For example, `"Acme Farm Irrigation, 12 mi North on FM 5555, 150 ft West into the field"` could be a valid Service Point address for a utility's own reference, but would not be largely considered by Customers to be an address able to be used in common mapping servcies for the United States.
* For each Typical Address, Servers MUST convert the address to a Normalized Address value using the following steps:
    * Remove the address name, if any (e.g. `"Acme Co., 123 Main  St. #234\n ..."` to `" 123 Main  St. #234\n ..."`).
    * Convert any postal abbreviations to their full names (e.g. `" 123 Main  St. #234\n ..."` to `" 123 Main  Street Unit 234\n ..."`).
    * Convert all characters to lowercase (e.g. `" 123 Main  Street Unit 234\n ..."` to `" 123 main  street unit 234\n ..."`).
    * Convert address segments breaks, whether line breaks or spaces, to the locale's address segment separator to make the address one line (e.g. `" 123 main  street unit 234\n ..."` to `" 123 main  street unit 234, ..."`).
    * Remove any duplicate whitespace (e.g. `" 123 main  street unit 234, ..."` to `" 123 main street unit 234, ..."`)
    * Remove any surrounding whitespace (e.g. `" 123 main street unit 234, ..."` to `"123 main street unit 234, ..."`)
    * The result is the Normalized Address used in address matching (e.g. `"123 main street unit 234, ..."`).
* For each Atypical Address, Servers MUST convert the address to a Normalized Address value using the following steps:
    * Convert all characters to lowercase (e.g. `" 12 mi  North on FM 5555\n ..."` to `" 12 mi  north on fm 5555\n ..."`).
    * Convert line breaks to single spaces (e.g. `" 12 mi  north on fm 5555\n ..."` to `" 12 mi  north on fm 5555  ..."`).
    * Remove any duplicate whitespace (e.g. `" 12 mi  north on fm 5555  ..."` to `" 12 mi north on fm 5555 ..."`)
    * Remove any surrounding whitespace (e.g. `" 12 mi north on fm 5555 ..."` to `"12 mi north on fm 5555 ..."`)
    * The result is the Normalized Address used in address matching (e.g. `"12 mi north on fm 5555 ..."`).
* For each string value ("Input Address") from the authorization details field array, perform a matching comparison using the following steps:
    * Normalize the Input Address using the same steps as normalizing an Atypical Address.
    * Determine if any of the Server's Normalized Addresses exactly match the normalized Input Address and are available to be matched for this scope and Client.
    * If there are no matches, treat the Input Address as having no matches.
    * If there are two or more matches, the Input Address is insufficiently unique and treat it as having no matches.
    * If there is exactly one match, consider that resource to be matched and include it in the results.
* On the page linked by the `documentation` URL value in the Authorization Details Field Object for any `addresses` field, Servers MUST include the following information for each of the Server's locales, so that Clients can format their authorization details field values appropriately:
    * What postal abbreviations are expanded during Typical Address normalization
    * What is used as the address segment separator and what address segments are included in a normalized Typical Address (e.g. `{street address}, {city}, {state}, {zip code}, {country}`)
    * Examples of Typical Addresses and Atypical Address, along with their Normalized Address strings

While the above normalization and matching requirements for Servers helps mitigate many of the common pitfalls for address matching, Clients are RECOMMENDED to take the following measures before constructing address strings for use in the authorization details fields:

* Reviewing the Server's documentation for the locale specifics for Normalized Addresses.
* For Typical Addresses:
    * Ensuring that postal abbreviations are expanded in line with the Server's documentation (e.g. `St.` to `street`).
    * Ensuring that all address segments are included in line with the Server's documentation (city, state, zip code, etc.).
    * Ensuring that apartment or office building unit numbers are included, when the Server's address for the resource includes a unit number.

### 7.2. Included Data Fields <a id="auth-details-included-data" href="#auth-details-included-data" class="permalink">🔗</a>

Many use cases for customer data access require a specific set of data fields to be populated in order for the Client to perform the requisite tasks with the data.
For example, a building energy manager Client may need access to both the energy usage intervals and bill section line items for a set of the Customer's properties in order to perform the energy cost analyses that they have been hired to do by the Customer.
However, in other use cases, the need for bill section line items may not be needed so a Customer may not wish their bill cost breakdowns be shared with a Client.

To provide the flexibility needed for Servers to cover a broad set of use cases, this specification defines authorization data fields that let the Client define which data fields they are requesting to be be included.
That way, Clients can customize the scopes of their requests to select the data fields they need, and ignore others which are not needed.
Additionally, by defining these authorization data fields, Servers will be able to disclose which data fields they support in their `authorization_details_fields_supported` array in their Server Metadata's Scope Descriptions [[CDS-WG1-02 Section 3.4](#ref-cds-wg1-02-scope-descriptions)].

For all included data fields defined in this section, the Server MUST implement the following requirements:

* Any included objects or object fields MUST be able to be accessed by the Client's base set of permissions.
  This means that if the Server has limited a Client to only having access to a specific set of objects or fields, including other data using these authorization details fields will not be able to extend access beyond what the Server permits for that Client.
  For example, if a utility vendor Client is set to only have access to Meter Devices and Usage Segments for a specific list of meters, when the Client uses the `include_aggregations` field to try to enable access to related Aggregations, the Server will return an empty list of Aggregations on the Aggregations API, because the Client is not permitted to see Aggregations, even if the Server does have related Aggregation objects stored in its system.
* If the field's value is `false`, the field does not change access beyond what is provided by the field's scope and other fields included in the scope's authorization details.

#### 7.2.1. Include Accounts <a id="auth-details-include-accounts" href="#auth-details-include-accounts" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Accounts](#account-format).
For example, a contracted vendor may need catalog and organize the list of accounts for an enterprise Customer's locations.
To enable whether Account objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Account access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_accounts"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Account objects.
  Related Account objects are any Account that are referenced by `cds_account_id`, `grouped_accounts`, or `related_accounts` values in any included [Service Contract](#service-contract-format), [Bill Statement](#bill-statment-format), [Bill Section](#bill-section-format), [Aggregation](#aggregation-format), or [Usage Segment](#usage-segment-format).
  Additionally, parent Accounts that are referenced by any included Accounts' `cds_account_parent` values MUST also be included.
* Servers MUST NOT include OPTIONAL fields for Accounts unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Account object fields.

##### 7.2.1.1. Include Account Numbers <a id="auth-details-include-account-numbers" href="#auth-details-include-account-numbers" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's account number for reference with other Customer materials (e.g. paper copies of the bills they received from the Customer).
For example, an energy service company Client doing and initial feasibility analysis for what energy upgrade projects would be viable at an enterprise Customer's locations may need to understand how the Customer's accounts are organized with the utility.
To address these use cases, this specification defines an authorization details field that controls whether Account `account_number` access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_account_numbers"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in the following objects if they are accessible using this field's scope and other authorization details fields:
    * [Account](#account-format) object's `customer_number`
    * [Account](#account-format) object's `account_number`
    * [Service Contract](#service-contract-format) object's `account_number`
    * [Bill Statement](#bill-statement-format) object's `account_number`
    * [Bill Section](#bill-section-format) object's `account_number`

##### 7.2.1.2. Include Account Details <a id="auth-details-include-account-details" href="#auth-details-include-account-details" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's account details for reference with other Customer materials (e.g. the Customer's provided contact details).
For example, an energy consultant Client working with an enterprise Customer may need to understand which divisions of the business are associated with which utility accounts.
To address these use cases, this specification defines an authorization details field that controls whether account details access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_account_details"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Account objects accessible using this field's scope:
    * `account_name`
    * `account_address`
    * `account_type`
    * `account_status`
    * `account_contacts`

##### 7.2.1.3. Include Account Programs <a id="auth-details-include-account-programs" href="#auth-details-include-account-programs" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's account programs for assessing the Customer's eligibility with various utility programs.
For example, an electric vehicle charging app Client may need to determine if a Customer is eligible for a low-income charging rebate offered by a utility.
To address these use cases, this specification defines an authorization details field that controls whether account programs access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_account_programs"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Account objects accessible using this field's scope:
    * `account_programs`

#### 7.2.2. Include Service Contracts <a id="auth-details-include-service-contracts" href="#auth-details-include-service-contracts" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Service Contracts](#service-contract-format).
For example, a contracted vendor may need get a business Customer's list of services so that they can determine which locations are feasible for a project.
To enable whether Service Contract objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Service Contract access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_service_contracts"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Service Contract objects.
  Related Service Contract objects are any Service Contract that are referenced by `cds_servicecontract_id`, `current_servicecontracts`, `previous_servicecontracts`, `related_servicecontracts`, `grouped_servicecontracts`, or `beneficiaries` values in any included [Service Point](#service-point-format), [Bill Section](#bill-section-format), [Aggregation](#aggregation-format), [Usage Segment](#usage-segment-format), or [Energy Attribute Certificate](#eac-format) object.
  Additionally, Service Contracts that have an `cds_account_id` value that matches any included Accounts MUST also be included.
* If this field's value is `true` and the scope also have the [`include_meter_devices`](#auth-details-include-meter-devices) field value as `true` or the scope includes Meter Device access, the Server MUST treat this scope as also having the [`include_service_points`](#auth-details-include-service-points) field value as `true`.
  This means that Service Point objects connecting Service Contracts and Meter Devices MUST be included when both Service Contracts and Meter Devices are included, so that Clients can know which Service Contracts are related to which Meter Devices.
* Servers MUST NOT include OPTIONAL fields for Service Contracts unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Service Contract object fields.

##### 7.2.2.1. Include Contract Numbers <a id="auth-details-include-contract-numbers" href="#auth-details-include-contract-numbers" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's contract number for reference with other Customer materials (e.g. paper copies of the bills they received from the Customer).
For example, an energy service company Client doing and initial feasibility analysis for what energy upgrade projects would be viable at an enterprise Customer's locations may need to match up the service charges from the paper utility bills to the list service contract numbers in the Customer's online accounts.
To address these use cases, this specification defines an authorization details field that controls whether Service Contract `contract_number` access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_contract_numbers"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Contract objects accessible using this field's scope:
    * `contract_number`

##### 7.2.2.2. Include Rate Plans <a id="auth-details-include-rate-plans" href="#auth-details-include-rate-plans" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's service rate plan details to use in analyses for the Customer.
For example, a demand response app Client working with a residential Customer may want to know if the Customer's home is one a time-of-use rate plan, which could impact whether they would save money by joining a demand response program.
To address these use cases, this specification defines an authorization details field that controls whether service contract details access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_rate_plans"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Contract objects accessible using this field's scope:
    * `rateplan_code`
    * `rateplan_name`

##### 7.2.2.3. Include Service Contract Details <a id="auth-details-include-contract-details" href="#auth-details-include-contract-details" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's service contract details for reference with other Customer materials (e.g. the Customer's service addresses).
For example, an energy consultant Client working with an enterprise Customer may need to understand which locations have how many and which types of utility services.
To address these use cases, this specification defines an authorization details field that controls whether service contract details access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_service_contract_details"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Contract objects accessible using this field's scope:
    * `contract_address`
    * `contract_start`
    * `contract_end`

##### 7.2.2.4. Include Service Programs <a id="auth-details-include-service-programs" href="#auth-details-include-service-programs" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's service programs for assessing the Customer's eligibility with various utility programs.
For example, a demand response aggregation app Client may need to determine if a Customer is already participating in a utility's demand response program.
To address these use cases, this specification defines an authorization details field that controls whether service programs access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_service_programs"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Contract objects accessible using this field's scope:
    * `service_programs`

#### 7.2.3. Include Service Points <a id="auth-details-include-service-points" href="#auth-details-include-service-points" class="permalink">🔗</a>

For some use cases, Clients need to have access to a specific list of [Service Points](#service-point-format).
For example, a utility vendor Client may need access to the list of relevant service point locations for the data analysis project with which they are working with the Server utility.
To enable whether Service Point objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Service Point access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_service_points"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Service Point objects.
  Related Service Point objects are any Service Point that are referenced by `current_servicepoints`, `previous_servicepoints`, `related_servicepoints`, or `grouped_servicepoints` values in any included [Meter Device](#meter-device-format), [Bill Section](#bill-section-format), [Aggregation](#aggregation-format), or [Usage Segment](#usage-segment-format) object.
* Servers MUST NOT include OPTIONAL fields for Service Points unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Service Point object fields.

##### 7.2.3.1. Include Premises <a id="auth-details-include-premises" href="#auth-details-include-premises" class="permalink">🔗</a>

For some use cases, Clients need to have access to the Server's premise identifiers and details for reference with other materials (e.g. the local municipality's building permit records).
For example, a utility vendor Client may need cross reference which of the utility's service points have had construction work performed on that premise's property.
To address these use cases, this specification defines an authorization details field that controls whether Service Point premise information is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_premises"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Point objects accessible using this field's scope:
    * `premises`

##### 7.2.3.2. Include Service Point Addresses <a id="auth-details-include-service-point-addresses" href="#auth-details-include-service-point-addresses" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's service point addresses for reference with other materials (e.g. municipal maps).
For example, a utility vendor Client may need cross reference which of the utility's service point locations are within areas designated as commercial zoning.
To address these use cases, this specification defines an authorization details field that controls whether service point address access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_service_point_addresses"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Point objects accessible using this field's scope:
    * `servicepoint_address`

##### 7.2.3.3. Include Coordinates <a id="auth-details-include-coordinates" href="#auth-details-include-coordinates" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's service point coordinates for reference with other materials (e.g. flood zone maps).
For example, a utility vendor Client may need cross reference which of the utility's rural service points are within a specific local municipal jurisdiction.
To address these use cases, this specification defines an authorization details field that controls whether service point coordinates access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_coordinates"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Service Point objects accessible using this field's scope:
    * `latitude`
    * `longitude`

#### 7.2.4. Include Meter Devices <a id="auth-details-include-meter-devices" href="#auth-details-include-meter-devices" class="permalink">🔗</a>

For some use cases, Clients need to have access to a specific list of [Meter Devices](#meter-device-format).
For example, a utility vendor Client may need access to the list of relevant meters for the data analysis project with which they are working with the Server utility.
To enable whether Meter Device objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Meter Device access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_meter_devices"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Meter Device objects.
  Related Meter Device objects are any Meter Device that are referenced by `related_meterdevices` or `grouped_meterdevices` values in any included [Bill Section](#bill-section-format), [Aggregation](#aggregation-format), or [Usage Segment](#usage-segment-format) object.
* If this field's value is `true` and the scope also have the [`include_service_contracts`](#auth-details-include-service-contracts) field value as `true` or the scope includes Service Contract access, the Server MUST treat this scope as also having the [`include_service_points`](#auth-details-include-service-points) field value as `true`.
  This means that Service Point objects connecting Meter Devices and Service Contracts MUST be included when both Meter Devices and Service Contracts are included, so that Clients can know which Meter Devices are related to which Service Contracts.
* Servers MUST NOT include OPTIONAL fields for Service Points unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Service Point object fields.

##### 7.2.4.1. Include Meter Numbers <a id="auth-details-include-meter-numbers" href="#auth-details-include-meter-numbers" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's meter numbers for reference with other Customer materials (e.g. their notes from an on-site inspection).
For example, an energy service company Client doing and initial feasibility analysis for an enterprise Customer's locations may need to match up the meter numbers written down during their on-site inspection to the list of Meter Devices obtained when requesting usage data from the Server for the Customer.
To address these use cases, this specification defines an authorization details field that controls whether Meter Device `meter_number` access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_meter_numbers"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Meter Device objects accessible using this field's scope:
    * `meter_number`

#### 7.2.5. Include Bill Statements <a id="auth-details-include-bill-statements" href="#auth-details-include-bill-statements" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Statements](#bill-statement-format).
For example, a financial auditor Client may need to review a Customer's utility bills as part of a business's financial audit.
To enable whether Bill Statement objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Bill Statement access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_statements"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Bill Statement objects.
  Related Bill Statement objects are any Bill Statements that have a `cds_account_id` value equal to the Accounts for which the scope is providing access.
* Servers MUST NOT include OPTIONAL fields for Bill Statements unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Bill statement object fields.

##### 7.2.5.1. Include Bill Statement Files <a id="auth-details-include-bill-statement-files" href="#auth-details-include-bill-statement-files" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Statements](#bill-statement-format) raw files, such as the PDF files provided to the Customer as their official from the utility.
For example, an energy efficiency upgrade contractor Client may need a Customer's bill PDFs to include in a loan application for financing a Customer's energy efficiency upgrade.
To address these use cases, this specification defines an authorization details field that controls whether Bill Statement raw file access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_statement_files"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Bill Statement objects accessible using this field's scope:
    * `file_uri`

##### 7.2.5.2. Include Bill Statement Charges <a id="auth-details-include-bill-statement-charges" href="#auth-details-include-bill-statement-charges" class="permalink">🔗</a>

For some use cases, Clients need to have access to a granular breakdown of a Customer's [Bill Statement](#bill-statement-format) charges for a bill.
For example, a commercial billing contractor Client may need the summary charge breakdown for Customer's bills to perform their contracted tasks for logging and paying a business Customer's utility bills.
To address these use cases, this specification defines an authorization details field that controls whether Bill Statement charge breakdown details are included.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_statement_charges"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Bill Statement objects accessible using this field's scope:
    * `previous_balance_amount`
    * `previous_balance_paid`
    * `previous_balance_unpaid`
    * `new_charges_amount`
    * `new_charges`
    * `new_balance_amount`

##### 7.2.5.3. Include Bill Statement Programs <a id="auth-details-include-bill-statement-programs" href="#auth-details-include-bill-statement-programs" class="permalink">🔗</a>

For some use cases, Clients need to have access which Account Programs in which a Customer participated for each [Bill Statement](#bill-statement-format) period.
For example, a rebate review contractor may need to see if a Customer was participating in a low-income program for the previous 6 months in order to qualify for a new rebate.
To address these use cases, this specification defines an authorization details field that controls whether Bill Statement program participation details are included.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_statement_programs"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Bill Statement objects accessible using this field's scope:
    * `program_participations`

#### 7.2.6. Include Bill Sections <a id="auth-details-include-bill-sections" href="#auth-details-include-bill-sections" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Sections](#bill-section-format).
For example, an energy service company Client may need to analyze a Customer's billed energy charges to settle an energy savings contract they have with the Customer.
To enable whether Bill Section objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Bill Section access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_sections"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Bill Section objects.
  Related Bill Section objects are any of the following:
    * Any Bill Section that have both a `cds_account_id` value equal to an included [Account](#account-format) and contains at least one included [Service Contract](#service-contract-format) listed in the Bill Section's `related_servicecontracts` array.
    * Any Bill Section that is referenced by `related_billsections` values in any included [Usage Segment](#usage-segment-format) object.
* Servers MUST NOT include OPTIONAL fields for Bill Statements unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Bill statement object fields.

##### 7.2.6.1. Include Bill Section Line Items <a id="auth-details-include-bill-section-line-items" href="#auth-details-include-bill-section-line-items" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Section](#bill-section-format) detailed line items from their utility bills.
For example, an enterprise's internal accounting team, in this case acting as the Client, may need to analyze the details energy cost breakdown for their enterprise, the Customer, to keep track of where energy costs are being allocated.
To address these use cases, this specification defines an authorization details field that controls whether Bill Section detailed line item access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_bill_section_line_items"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Bill Section objects accessible using this field's scope:
    * `line_items`

#### 7.2.7. Include Aggregations <a id="auth-details-include-aggregations" href="#auth-details-include-aggregations" class="permalink">🔗</a>

For some use cases, Clients need to have access to a specific list of [Aggregations](#aggregation-format).
For example, an academic research Client may need access aggregated usage data as part of their energy grid project with their university.
To enable whether Aggregation objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Aggregation access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_aggregations"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Aggregation objects.
  Related Aggregation objects are any of the following:
    * Any Aggregation objects that are referenced by `grouped_aggregations` or `related_aggregations` values in any included [Aggregation](#aggregation-format) or [Usage Segment](#usage-segment-format) object.
    * Any Aggregation objects that have `grouped_accounts` for any included Account objects.
    * Any Aggregation objects that have `grouped_servicecontracts` for any included Service Contract objects.
    * Any Aggregation objects that have `grouped_meterdevices` for any included Meter Device objects.
* Servers MUST NOT include OPTIONAL fields for Aggregations unless those fields are enabled to be included by the Grant's scope or other authorization details fields.

Additional authorization details fields are defined in subsections to this section to enable the inclusion of OPTIONAL Aggregation object fields.

##### 7.2.7.1. Include Aggregation Addresses <a id="auth-details-include-aggregation-addresses" href="#auth-details-include-aggregation-addresses" class="permalink">🔗</a>

For some use cases, Clients need to have access to an [Aggregation](#aggregation-format)'s grouped set of addresses.
For example, a utility vendor Client may need access to an Aggregation's set of addresses to cross reference them with other municipal details (e.g. the distribution zoning designations for the aggregated data).
To address these use cases, this specification defines an authorization details field that controls whether Aggregation `grouped_addresses` access is enabled.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_aggregation_addresses"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, Servers MUST include the following field values in any Aggregation objects accessible using this field's scope:
    * `grouped_addresses`

#### 7.2.8. Include Usage Segments <a id="auth-details-include-usage-segments" href="#auth-details-include-usage-segments" class="permalink">🔗</a>

For some use cases, Clients need to have access to a set of [Usage Segments](#usage-segment-format).
For example, a utility vendor Client may need to access the electric usage for a set of Customer Accounts to analyze the impact of a rebate program.
To enable whether Usage Segment objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Usage Segment access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_usage_segments"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related Usage Segment objects.
  Related Usage Segment objects are any Usage Segments that have the following relationships with other objects included in this field's scope or other authorization details fields:
    * Usage Segments that have `related_aggregations` values that match the `cds_aggregation_id` for any included Aggregation object.
    * Usage Segments that have `related_accounts` values that match the `cds_account_id` for any included Account object.
    * Usage Segments that have `related_servicecontracts` values that match the `cds_servicecontract_id` for any included Service Contract object.
    * Usage Segments that have `related_servicepoints` values that match the `cds_servicepoint_id` for any included Service Point object.
    * Usage Segments that have `related_meterdevices` values that match the `cds_meterdevice_id` for any included Meter Device object.
    * Usage Segments that have `related_billsections` values that match the `cds_billsection_id` for any included Bill Section object.
* If this field's value is `false`, Servers MUST NOT include Usage Segment objects in the Client's access.

##### 7.2.8.1. Include Usage Segment Formats <a id="auth-details-include-usage-segment-formats" href="#auth-details-include-usage-segment-formats" class="permalink">🔗</a>

For some use cases, Clients need to have access to a specific set of [Usage Segment Value Formats](#usage-segment-value-format) (e.g. both usage kWh and peak kW readings).
For example, a utility vendor Client may need to access the electric usage, demand, and supply mix for a specific set of Meter Devices with which they are contracted in analyzing.
To enable which Usage Segment Value Formats access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional Usage Segment access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_usage_segment_formats"`.
* The `format` value MUST be `"choice_list_or_null"`.
* The `choices` array contains Choice objects who's `id` values are one of the defined [Usage Segment Value Formats](#usage-segment-value-format) in this specification.
  Servers MAY include additional Choice objects that do not represent one of the defined Value Formats defined in this specification in order to accommodate extensions and Server-specific use cases (e.g. cloud coverage readings).
  Servers MUST NOT have custom Choice `id` values that represent an already defined Value Format in this specification, so that Clients can be interoperable with Servers for the set of Value Formats defined in this specification.
  Clients MUST ignore any Choice `id` values that they do not understand, so that Servers are able to include extension and Server-specific `id` values without breaking Client integrations.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If the value is `null`, Usage Segments included as part of this field's scope MUST have empty arrays (`[]`) for their `values` and `format` values.
  This can be useful if the Server is only providing durations (e.g. month cutoff dates) for a Usage Segment, but no values, for example as part of an Aggregation.
* If the value is an array, Usage Segments included as part of this field's scope MUST have `format` values that are an array that contains the same or a subset of Value Formats in this field's array.
  Servers MAY order the `format` values in any order, so it does not have to be in the same order as this field's array values.
  Clients MUST be able to parse Usage Segment [Value Formats](#usage-segment-value-format) in any order and in any subset of this field's array values.

#### 7.2.9. Include Energy Attribute Certificates <a id="auth-details-include-eacs" href="#auth-details-include-eacs" class="permalink">🔗</a>

For some use cases, Clients need to have access to a set of [Energy Attribute Certificates](#eac-format") (EACs).
For example, a emissions auditor Client may need to access the utility's EACs that have been applied to a set of enterprise Customer's Accounts in order to verify the Customer's corporate emissions reporting.
To enable whether EAC objects access is included in addition to the data normally provided for a scope, this specification defines an authorization details field that controls additional EAC access.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"include_eacs"`.
* The `format` value MUST be `"boolean"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* If this field's value is `true`, in addition to the data access defined with this field's scope, the Server MUST provide access to related EAC objects.
  Related EAC objects are any EACs that have the following relationships with other objects included in this field's scope or other authorization details fields:
    * EACs that have a `beneficiary_type` value of `customer` and `beneficiaries` values that match the `customer_number` for any included Account object.
    * EACs that have a `beneficiary_type` value of `account` and `beneficiaries` values that match the `cds_account_id` for any included Account object.
    * EACs that have a `beneficiary_type` value of `servicecontract` and `beneficiaries` values that match the `cds_servicecontract_id` for any included Service Contract object.
    * EACs that have a `beneficiary_type` value of `rateplan` and `beneficiaries` values that match the `rateplan_code` for any included Service Contract object.
    * EACs that have a `beneficiary_type` value of `program` and `beneficiaries` values that match the `program_number` for any included Account object's `account_programs` entries or Service Contract object's `service_programs` entries.
    * EACs that are referenced in [`eacs`](#TODO-usage-segment-value-eacs) formatted [Value Objects](#usage-segment-value-object-format) in any included [Usage Segment](#usage-segment-format) object.
    * Any additional EACs that the Server determines are related to this field's scope or other authorization details fields (e.g. EACs that have a `beneficiary_type` value of `general` and applicable to the Client's use case).
* If this field's value is `false`, Servers MUST NOT include EAC objects in the Client's access.

### 7.3. Duration Fields <a id="auth-details-duration" href="#auth-details-duration" class="permalink">🔗</a>

Many use cases for customer data access have a defined date range of data that should be accessible to the Client.
For example, an energy auditor Client may only need access to the past three years of historical energy usage intervals, but not any ongoing energy usage intervals, because the audit period is a fixed date range (e.g. over the past three years).
However, in other use cases, a Client may also be the Customer and is using the Server's implementation of this specification as a means for self-accessing their own data, in which case an authorization or grant duration is indefinite.

To provide the flexibility needed for Servers to cover a broad set of use cases, this specification defines authorization details fields that let the Client request customized durations of access that appropriately match their scope of work.
Additionally, by defining these authorization details fields, Servers will be able to disclose the maximum date ranges they support in their `authorization_details_fields_supported` array in their Server Metadata's Scope Descriptions [[CDS-WG1-02 Section 3.4](#ref-cds-wg1-02-scope-descriptions)].

For all duration authorization details fields defined in this section, a Server MUST implement the following requirements:

* Any included objects or object fields MUST be able to be accessed by the Client's base set of permissions.
  This means that if the Server has limited a Client to only having access to a specific time range of objects or fields, including other data using these duration fields will not be able to extend access beyond what the Server permits for that Client.
  For example, if a utility vendor Client is set to only have access to one year of data for Usage Segments, when the Client uses the `start_date` and `end_date` fields to try to enable access for two years, the Server will still only return one year of Usage Segment data, even if the Server does have the two years of Usage Segment objects stored in its system.
* If the field is part of an authorization details array that is returned by the Server in a Token Response [[RFC 9396 Section 7](#ref-rfc9396-token-response)] or the Grants API [[CDS-WG1-02 Section 8.1](#ref-cds-wg1-02-grant-object)], the value of the field MUST be a `date` or `datetime` or `"infinite"` string, subject to the field's other constraints (e.g. the `maximum` value), and MUST NOT be a `relative date` or `relative datetime` string.
  This ensures Clients are provided an absolute time for field's value.
* Clients MAY set formats that are any valid relative date or datetime format when including the field in any authorization details array as part of a request to the Server.
  When receiving a relative formatted string (e.g. `"P30DT2H"`) from a Client's request for the field, as part of any Token Response and for any Grant created, the Server MUST convert that relative string into an absolute string (i.e. `absolute date` or `absolute datetime`) that is the Server's Grant creation time plus the duration of the relative string for the authorization details field.
  This ensures that Clients can specify a relative duration during authorization and token requests, then when a Grant is issued, the Client can know the absolute date or datetime for the field that the Server has set.

#### 7.3.1. Sync Until <a id="auth-details-sync-until" href="#auth-details-sync-until" class="permalink">🔗</a>

For some use cases, Clients need to have ongoing access to Customer Data that may change over time.
For example, a demand response app may need continuing access to a Customer's rate plan details for a defined period of time for a program.
To control how long a Client has access to updated data for a Customer, this specification defines an authorization details field that allows configuration of when the Server will stop syncing updates to objects made available to the Client.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"sync_until"`.
* The `format` value MUST be `"relative_or_absolute_datetime"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Data available under the scope for this authorization details field MUST be populated with data that is valid on or before the `sync_until` datetime.
* If the Server provides Customer Data objects when requested by the Client that are populated synchronously from the Server's source of truth (e.g. by internally proxying requests to the utility's internal customer database), the Server MUST NOT populate data that is valid after the `sync_until` datetime.
  This means that a Server MUST freeze the Customer Data as of the `sync_until` time for Client requests made after the end of the `sync_until` datetime so that Clients do not have access to changes to the data after that.
* If the Server populates data objects for the Grant asynchronously after a Grant is created, the Server MUST ensure that the data populated is not from a time after the `sync_until` value.
  This means that if a Client submits this field as part of a request and the value has no duration (e.g. `"P0S"`), the Server's expiration for syncing will be the time of creation of any Grant, and the Server MUST populate the data objects with data synced before or exactly the same as the Grant's creation time.
* In the situation that a Server that asynchronously updates data objects finds that their asynchronous task for a Grant is running after the `sync_until` datetime has expired, the Server MUST continue the task and update any data objects with data that was previously synchronized before the `sync_until` datetime.
  This means that new Grants that have no duration will still have their data objects populated with data, even if the asynchronous task to update data happens after the `sync_until` datetime.
  This also means that it is possible for a `cds_modified` datetime to be after the `sync_until` datetime, if the object's data is updated after the `sync_until` datetime with data that was valid before the `sync_until` datetime.
* If a Server receives updated data from their source of truth (e.g. a utility's internal meter data management system or customer database) that is valid as of a time after a data objects `cds_synced` datetime and before or on the Grant's `sync_until` value, the Server MUST update the object's data, if anything, and update the `cds_synced` value to match the datetime of the source of truth's data validity, even if no changes to the object's data were made.
  This ensures that Clients know when the last confirmation of validity was for a data object.

#### 7.3.2. Bill Statement Date Start <a id="auth-details-statement-start" href="#auth-details-statement-start" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Statements](#bill-statement-format) for a specific period of time.
For example, a financial auditor Client may need to review a Customer's utility bills for the prior calendar year as part of a business's financial audit.
To control how many Bill Statements a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of date ranges of Bill Statements.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"statement_date_start"`.
* The `format` value MUST be `"relative_or_absolute_date"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Bill Statement objects available under the scope for this authorization details field MUST have a `statement_date` value that is equal to or after this field's value.

#### 7.3.3. Bill Statement Date End <a id="auth-details-statement-end" href="#auth-details-statement-end" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Statements](#bill-statement-format) for a specific period of time.
For example, a financial auditor Client may need to review a Customer's utility bills for the prior calendar year as part of a business's financial audit.
To control how many Bill Statements a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of date ranges of Bill Statements.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"statement_date_end"`.
* The `format` value MUST be `"relative_or_absolute_date"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Bill Statement objects available under the scope for this authorization details field MUST have a `statement_date` value that is equal to or before this field's value.

#### 7.3.4. Bill Section Start Date <a id="auth-details-bill-section-start" href="#auth-details-bill-section-start" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Bill Sections](#bill-section-format) for a specific period of time.
For example, an energy service company Client may need to compare a Customer's utility service costs with the contracted savings guarantees they have with the Customer.
To control how many Bill Sections a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of date ranges of Bill Sections.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"start_date"`.
* The `format` value MUST be `"relative_or_absolute_date"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Bill Section objects available under the scope for this authorization details field MUST have a `start_date` value that is equal to or after this field's value.

#### 7.3.5. Bill Section End Date <a id="auth-details-bill-section-end" href="#auth-details-bill-section-end" class="permalink">🔗</a>
For some use cases, Clients need to have access to a Customer's [Bill Sections](#bill-section-format) for a specific period of time.
For example, an energy service company Client may need to compare a Customer's utility service costs with the contracted savings guarantees they have with the Customer.
To control how many Bill Sections a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of date ranges of Bill Sections.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"end_date"`.
* The `format` value MUST be `"relative_or_absolute_date"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Bill Section objects available under the scope for this authorization details field MUST have a `end_date` value that is equal to or before this field's value.

#### 7.3.6. Usage Segment Start <a id="auth-details-usage-segment-start" href="#auth-details-usage-segment-start" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Usage Segments](#usage-segment-format) for a specific period of time.
For example, an electric vehicle charging app Client may need to display a Customer's energy meter interval usage compared to the Customer's electric vehicle charging schedule.
To control how many Usage Segments a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of time ranges of Usage Segments.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"segment_start"`.
* The `format` value MUST be `"relative_or_absolute_datetime"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Usage Segment objects available under the scope for this authorization details field MUST have a `segment_start` value that is equal to or after this field's value.
* If a Usage Segment contains [Value Sets](#usage-segment-value-set-format) `values` that represent intervals that start on or after this field's value, the Server MUST create a new Usage Segment with `values` that contain only Value Sets representing intervals that start on or after this field's value.
  For example, if the Server normally provides 30 day long Usage Segments with daily intervals (i.e. 30 Value Sets per Usage Segment), and the Client sets this field's value to start 10 days into a normal Usage Segment period, the Server MUST not provided access to the normal Usage Segment and instead create a Usage Segment that has the remaining 20 days worth of Value Sets as the Usage Segment's `values` and provide access to that trimmed Usage Segment.
  This means that Clients will still be granted access to authorized interval data even if the normally provided Usage Segment is partially cut off by this field's value.

#### 7.3.7. Usage Segment End <a id="auth-details-usage-segment-end" href="#auth-details-usage-segment-end" class="permalink">🔗</a>

For some use cases, Clients need to have access to a Customer's [Usage Segments](#usage-segment-format) for a specific period of time.
For example, an electric vehicle charging app Client may need to display a Customer's energy meter interval usage compared to the Customer's electric vehicle charging schedule.
To control how many Usage Segments a Client has access to for a Customer, this specification defines an authorization details field that allows configuration of time ranges of Usage Segments.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"segment_end"`.
* The `format` value MUST be `"relative_or_absolute_datetime"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* Usage Segment objects available under the scope for this authorization details field MUST have a `segment_end` value that is equal to or before this field's value.
* If a Usage Segment contains [Value Sets](#usage-segment-value-set-format) `values` that represent intervals that end on or before this field's value, the Server MUST create a new Usage Segment with `values` that contain only Value Sets representing intervals that start on or after this field's value.
  For example, if the Server normally provides 30 day long Usage Segments with daily intervals (i.e. 30 Value Sets per Usage Segment), and the Client sets this field's value to end 10 days from the end of a normal Usage Segment period, the Server MUST not provided access to the normal Usage Segment and instead create a Usage Segment that has the first 20 days worth of Value Sets as the Usage Segment's `values` and provide access to that trimmed Usage Segment.
  This means that Clients will still be granted access to authorized interval data even if the normally provided Usage Segment is partially cut off by this field's value.

#### 7.3.8. Expires <a id="auth-details-expires" href="#auth-details-expires" class="permalink">🔗</a>

For some use cases, Clients and Customers need to be able to set an expiration date for a Grant.
For example, a tenant Customer may be only able to consent to being included in an Aggregation for whole-building data for a 1-year period, and have to reauthorize each subsequent year.
To control how long a Grant lasts, this specification defines an authorization details field that allows configuration of a Grant's `expires` value.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"expires"`.
* The `format` value MUST be `"relative_or_absolute_datetime"`.
* If this object is included in a Scope Description where the `response_types_supported` field is not empty array (e.g. Customer authorization is supported):
    * The `is_required` value MUST be `false`.

Additionally, Servers MUST implement the following behavior to support this authorization details field:

* When a Grant is created with this field as an authorization details field, provided that all scope and authorization details are valid, the Server MUST set to the Grant's `expires` value to this field's value.
* When the Grant expires, the Server MUST remove the Client's access that was provided by the Grant. 

### 7.4. Other Fields <a id="auth-details-other" href="#auth-details-other" class="permalink">🔗</a>

This section defines authorization details fields that do not fall under the preselection, data fields, or duration categories, but are still important user experience or constraining factors that help Clients customize their requests to more seamlessly integrate their applications with Servers.

#### 7.4.1. Selection Type <a id="auth-details-selection-type" href="#auth-details-selection-type" class="permalink">🔗</a>

For some [Scopes](#scopes), the Server has multiple [Selections](#selection-component) as choices to use when rendering the authorization form for a Customer.
While this specification requires that the Server define a default Selection used for a given Scope, in some use cases a Client could need to have the Server render an alternative Selection for the Customer to choose their resources on the authorization form.
To allow this Client customization behavior, this specification defines an authorization detail that is applied to Scopes that have multiple Selection choices, so that Clients opt to have the Server render a non-default Selection on the authorization form.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"authorization_form_selection_type"`.
* The `format` value MUST be `"choice"`.
* The `is_required` value MUST be `false`.

Servers MUST include an object for this authorization field in a Scope Description's `authorization_details_fields_supported` array when the [Scope](#scopes) definition lists multiple Selections as possible ways to render the Customer's resource selection in the authorization form.
Servers MAY support any number of the possible Selection choices for a scope, but Servers MUST always support this authorization details field when supporting that scope.
So even if a Server supports only one of the Selections listed as possibilities in a given scope definition in this specification, the Server MUST include this authorization details field in it's scope Description with one choice available and the default choice defined, so that the Client knows which Selection will be rendered in the authorization form the scope by default.

When this authorization field is included in an authorization request, Servers MUST implement the following requirements:

* The Server MUST render the chosen Selection as the Selection Component for that scope's Authorization Segment.

#### 7.4.2. Merge Selections <a id="auth-details-merge-selections" href="#auth-details-merge-selections" class="permalink">🔗</a>

For some use cases, a Client may send authorization request that contains multiple scopes that are intended to be applied of the same selection of Customer accounts or service contracts.
For example, a demand response provided may need to request access to both interval energy usage intervals (e.g. the `cds_meter_usage` scope) and the Customer's rate plan (e.g. the `cds_rate_plan` scope), and the Client needs the list of Service Contracts for which the Customer authorizes to be the same.

In these relevant use cases, Clients benefit from being able to have the Server "merge" [Selections](#selection-component) in the authorization form into a single Selection Component that constrains multiple scopes to the same set of resources (e.g. Service Contracts).

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"merge_selection_with"`.
* The `format` value MUST be `"string_or_null"`.
* The `is_required` value MUST be `false`.

Servers MUST NOT include an object for this authorization field in a Scope Description's `authorization_details_fields_supported` array when the Scope Description's `response_types_supported` list is empty (i.e. no authorization request method is available).

When this authorization field is included in an authorization request, Servers MUST implement the following requirements:

* When the field value is a string, the Server MUST match the string to another individual scope included in the scope string request parameter.
  Additionally, the Server MUST implement the following behavior:
    * If the string does not match any of these values in the authorization request, the Server MUST reject the authorization request with an `invalid_authorization_details` error as defined in [[RFC 9396 Section 5](#ref-rfc9396-error-response)].
    * If there are multiple matches in the authorization request, the Server MUST choose the first matched scope as the one intended to be grouped with this authorization details' scope.
    * When a match is found for this field's string value, the Server MUST require the matches pass the following validation requirements:
        * Matches MUST have a non-`null` value for that scope's `merge_selection_with` authorization details field.
        * Matches MUST have the same Selection type to be rendered on the authorization form, so that the Selection Component can be unified.
        * For preselection authorization details fields that exist both in both match's Scope Description `authorization_details_fields_supported`, the field's object MUST have the same values, so that the field's value in the request has the same validation process for both scopes.
        * For preselection authorization details fields that only exist in one or the other Scope Descriptions `authorization_details_fields_supported`, the field's value in the request MUST be `null` or `null` by default, so that there is no preselection impact for combining the Scopes into a single Selection Component.
    * If a match does not pass validation, the Server MUST reject the authorization request with an `invalid_authorization_details` error.
    * If a match passes validation, the Server MUST render the Authorization Segments for this field's scope and the matched scope as having a single Selection Component, such that the User only needs to select the resources in that Selection and those resources will be applied to all of the scopes grouped together via this authorization details field.
    * Servers MUST support merging the any number of scopes together that reference each other via this authorization details field, so that Clients can group any number of scopes together into a single Selection Component for an authorization form.
* When the field value is `null`, Servers MUST treat the scope for this authorization details field as an individually rendered Authorization Segment with an individual Selection Component.
  This means that when a scope uses this authorization details field to reference another scope, that other scope MUST also have a non-`null` value for its `merge_selection_with` authorization details field.

#### 7.4.3. Error If No Preselections <a id="auth-details-error-if-no-preselections" href="#auth-details-error-if-no-preselections" class="permalink">🔗</a>

For some use cases, when requesting authorization from a Customer, a Client may want to preselect certain fields on the authorization form.
These preselections are configured by other authorization details fields (e.g. `service_types`).
However, sometimes a Customer does not have any Accounts, Meter Devices, or other relevant objects that meet the preselection criteria.
For example, an energy audit contractor Client may request access to all of a Customer's electric Service Contracts using the [`service_types`](#auth-details-service-types) authorization details field (i.e. `"service_types": ["electric"]`).
If the Customer does not have any electric meters (e.g. they are a gas-only customer), the authorization form will not be able to have any Service Contracts preselected.
In other situations, a Customer may have multiple accounts they control, such as a commercial Customer with multiple locations, and may need to reauthenticate as a different user if they are accidentally logged in as the wrong user for an authorization request with a specific list of preselected [account numbers](#auth-details-account-numbers) (e.g. `"account_numbers": ["1234-5"]`).

In these relevant use cases, Clients benefit from being able to configure the authorization request to automatically render a [Preselection Error](#preselection-error) for an authenticated Customer if they do not have any objects that can be preselected.
That way, Customers are given a chance to re-authenticate as a different user or decline the authorization and be redirected back to the Client with a specific error.
This allows Clients to present relevant messages to Customers that do not have required objects for the Client's use case (e.g. "You don't appear to have any electric meters, so we won't be able to run an energy audit for your account.").

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"error_if_no_preselections"`.
* The `format` value MUST be `"boolean"`.

For scopes where the Scope Description's `response_types_supported` array is not empty (i.e. authorization requests are required) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* When this field value is `true`, after the user is authenticated and prior to rendering the authorization form to the user, the Server MUST determine if the user will need to select any objects relevant to preselection fields included in the authorization details of the request (e.g. `"account_numbers": ["1234-5"]`) didn't match any of the user's Account `account_number` values, so no Accounts will be preselected and the user will be asked to select the Accounts they want to apply to this authorization).
  If the user will need to select any objects relevant to preselection fields included in the authorization details of the request, the Server MUST render and present a [Preselection Error](#preselection-error) to the user, which allows the user to either reauthenticate or cancel the authorization request.
 If the user selects to cancel the authorization request, the Server MUST redirect the user back to the Client's `redirect_uri` with an `error` parameter value of `no_preselections`.
* If the authorization request did not include any preselection fields, defined by Section 7.1, or this field value is `false`, the Server MUST ignore this field and treat the authorization request as if this field was not included.

For scopes where the Scope Description's `response_types_supported` array is empty (i.e. no authorization request method is available) and this field is included in `authorization_details_fields_supported` object, Servers MUST implement the following requirements:

* When this field value is `true`, the Server MUST determine if the preselection fields (e.g. `"account_numbers": ["1234-5"]`) are acceptable for the Client's Access Token request and the Server can filter the scope to the preselected resources (e.g. only grant access to the provided account numbers).
  If the Server determines that the preselection field values are insufficient or invalid (e.g. the Client provided too many account numbers in the request), the Server MUST respond to the Access Token request with the error `invalid_authorization_details` as defined in [[RFC 9396 Section 5](#ref-rfc9396-error-response)].
* If this field value is `false`, the Server MUST ignore this field and treat the token request as if this field was not included.

#### 7.4.4. Allow Scope Modifications <a id="auth-details-allow-scope-modifications" href="#auth-details-allow-scope-modifications" class="permalink">🔗</a>

For some use cases, a Client may have a specific set of data fields that they are required to have to complete their process.
For example, an energy audit contractor could be required to obtain the last 12 months of meter usage data from a Customer in order to complete their energy audit report.

In these relevant use cases, Clients benefit from being able to configure an authorization request that cannot be modified by a Customer during the authorization process, so that the Customer does not inadvertently remove required data fields or ranges.
This authorization data field is intended to allow Clients a way to configure a Server's authorization form as editable or non-editable.

To support this authorization details field, the Authorization Details Field Object MUST meet the following requirements:

* The `id` value MUST be `"allow_scope_modifications"`.
* The `format` value MUST be `"boolean"`.

Servers MUST NOT include an object for this authorization field in a Scope Description's `authorization_details_fields_supported` array when the Scope Description's `response_types_supported` list is empty (i.e. no authorization request method is available).

When this authorization field is included in an authorization request, Servers MUST implement the following requirements:

* When the field value is `true`, Servers MAY render authorization form presented to the user as editable, meaning that the user MAY modify the requested scope or authorization details values within the authorization form and then authorize that modified scope.
* When this field value is `false`, Servers MUST render the authorization from presented to the user as non-editable, meaning that the user MUST be able to only authorize or decline the authorization request as a whole and cannot modify the requested scope.

## 8. Client Registration Requirements <a id="client-registration-requirements" href="#client-registration-requirements" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 9. Customer Authorizations <a id="customer-authorizations" href="#customer-authorizations" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 9.1. Authorization Form Layout <a id="auth-form-layout" href="#auth-form-layout" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 9.2. Authorization Form Segments <a id="auth-form-segments" href="#auth-form-segments" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

#### 9.2.1. Data Component <a id="data-component" href="#data-component" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

#### 9.2.2. Duration Component <a id="duration-component" href="#duration-component" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

#### 9.2.3. Selection Component <a id="selection-component" href="#selection-component" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

##### 9.2.3.1. Account Selection <a id="account-selection" href="#account-selection" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

##### 9.2.3.2. Service Contract Selection <a id="contract-selection" href="#contract-selection" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

##### 9.2.3.3. Service Point Selection <a id="servicepoint-selection" href="#servicepoint-selection" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

##### 9.2.3.4. Meter Device Selection <a id="meter-selection" href="#meter-selection" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

##### 9.2.3.5. Aggregation Selection <a id="aggregation-selection" href="#aggregation-selection" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 9.3. Authorization Errors <a id="auth-errors" href="#auth-errors" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

#### 9.3.1. Preselection Error <a id="preselection-error" href="#preselection-error" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 10. Accounts API <a id="accounts-api" href="#accounts-api" class="permalink">🔗</a>

This specification requires Servers provide a set of Application Programming Interfaces (APIs) allowing Clients to retrieve a Customer's Account details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Accounts API.

### 10.1. Account Object Format <a id="account-format" href="#account-format" class="permalink">🔗</a>

Account objects are formatted as JSON objects and contain the following named values:

* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Account object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Account object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Account object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `cds_account_parent` - _[string](#string) or `null`_ - (REQUIRED) The `cds_account_id` of a parent Account object of which this Account is grouped under.
  If the Account is not a part of a parent grouping, this value is `null`.
* `customer_number` - _[string](#string) or `null`_ - (OPTIONAL) The identifier that a Customer has access to that identifies a collection of Accounts as being for the same Customer.
  If a Server only stores Accounts as the top-level identifier for Customers, or does not have a Customer-accessible identifier set for this Account, this value is `null`.
* `account_number` - _[string](#string)_ - (OPTIONAL) The number that a Customer sees on their bill and online user interface as the identifier for this Account.
* `account_name` - _[string](#string)_ - (OPTIONAL) The name that a Customer sees on their bill and online user interface as the name for this Account, if available.
* `account_address` - _[string](#string)_ - (OPTIONAL) The address that a Customer sees on their bill and online user interface as the address for this Account, if available.
  This string MAY have linebreaks, such as when the address is multiple lines.
* `account_type` - _[AccountType](#TODO-account-types)_ - (OPTIONAL) What type of Account this is.
* `account_status` - _[AccountStatus](#TODO-account-status)_ - (OPTIONAL) What the Account's status is.
* `account_contacts` - _Array[[AccountContact](#TODO-account-contact-format)]_ - (OPTIONAL) A list of Account Contacts for the Account.
* `account_programs` - _Array[[AccountProgram](#TODO-account-program-format)]_ - (OPTIONAL) A list of Account Programs for the Account.
  If the Server does not have this information or the Customer is not participating in any Account-level programs, this value is and empty list (`[]`).

### 10.2. Listing Accounts <a id="accounts-list" href="#accounts-list" class="permalink">🔗</a>

Clients may request to list Account objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Accounts, to the `cds_accounts_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Account listing request responses are formatted as JSON objects and contain the following named values.

* `accounts` - _Array[[Account](#account-format)]_ - (REQUIRED) A list of Accounts to which the requesting `access_token` is scoped to have access.
  If no Accounts are accessible, this value is an empty list (`[]`).
  If more than 100 Accounts are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Accounts.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Accounts.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Accounts.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Accounts to be the intersection of results for each of the URL parameters filters:

* `cds_account_ids` - A space-separated list of `cds_account_id` values for which the Servers MUST filter the Accounts.
* `customer_numbers` - A space-separated list of `customer_number` values for which the Servers MUST filter the Accounts.
  Customer number values of `null` MUST be treated as invalid for this URL parameter filter (i.e. only populated `customer_number` values are available to be filtered using URL parameters).
* `account_numbers` - A space-separated list of `account_number` values for which the Servers MUST filter the Accounts.
* `account_statuses` - A space-separated list of `account_status` values for which the Servers MUST filter the Accounts.
* `q` - A search term for which the Servers MUST filter the Accounts following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_account_id`
    * `customer_number`
    * `account_number`
    * `account_address`
    * `account_name`
    * `contact_name` in any included Account Contact object
    * `contact_email` in any included Account Contact object
    * `program_number` in any included Account Program object

Listings of Account objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Account MUST be first in each listing.

## 11. Service Contracts API <a id="service-contracts-api" href="#service-contracts-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's Service Contract details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Service Contracts API.

### 11.1. Service Contract Object Format <a id="service-contract-format" href="#service-contract-format" class="permalink">🔗</a>

Service Contract objects are formatted as JSON objects and contain the following named values:

* `cds_servicecontract_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Service Contract object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Service Contract object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Service Contract object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account to which this Service Contract belongs.
* `account_number` - _[string](#string)_ - (OPTIONAL) The Account's `account_number` duplicated to this Service Contract.
* `contract_number` - _[string](#string) or `null`_ - (OPTIONAL) The identifier that a Customer sees on their bill or online user interface as the identifier for this Service Contract.
  If a Server does not have a Customer-facing identifier for a Service Contract and the Client is not authorized to see the Server's internal identifier for this Service Contract, this value is `null`.
* `contract_address` - _[string](#string)_ - (OPTIONAL) The address that a Customer sees on their bill or online user interface as the address for this Service Contract, if available.
  This string MAY have linebreaks, such as when the address is multiple lines.
* `contract_status` - _[ContractStatus](#TODO-contract-statuses)_ - (REQUIRED) The current status of the Service Contract.
* `contract_type` - _[ContractType](#TODO-contract-types)_ - (REQUIRED) The type of agreement that this Service Contract represents.
* `contract_entity` - _[string](#string)_ - (REQUIRED) With which entity the Customer has agreement for this Service Contract.
* `contract_start` - _[date](#date)_ - (OPTIONAL) When the agreement that this Service Contract represents started.
* `contract_end` - _[date](#date) or `null`_ - (OPTIONAL) When the agreement that this Service Contract represents ended.
  If the agreement is still ongoing, this value is `null`.
* `service_type` - _[ServiceType](#TODO-service-types)_ - (REQUIRED) The type of utility or other service that this Service Contract provides.
* `service_class` - _[ServiceClass](#TODO-service-classes)_ - (REQUIRED) The class of service for which this Service Contract is categorized.
* `rateplan_code` - _[string](#string)_ - (OPTIONAL) A unique code for the current rate plan or tariff on which costs for this Service Contract are calculated.
* `rateplan_name` - _[string](#string)_ - (OPTIONAL) The name that a Customer sees on their bill or online user interface as the rate plan or tariff that applies to this Service Contract.
* `service_programs` - _Array[[ServiceProgram](#TODO-service-program-format)]_ - (OPTIONAL) A list of Service Programs for the Account.
  If the Server does not have this information or the Customer is not participating in any Service Contract-level programs, this value is and empty list (`[]`).

### 11.2. Listing Service Contracts <a id="service-contract-list" href="#service-contract-list" class="permalink">🔗</a>

Clients may request to list Service Contract objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Service Contracts, to the `cds_servicecontracts_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Service Contract listing request responses are formatted as JSON objects and contain the following named values.

* `service_contracts` - _Array[[ServiceContract](#service-contract-format)]_ - (REQUIRED) A list of Service Contracts to which the requesting `access_token` is scoped to have access.
  If no Service Contracts are accessible, this value is an empty list (`[]`).
  If more than 100 Service Contracts are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Service Contracts.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Service Contracts.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Service Contracts.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Service Contracts to be the intersection of results for each of the URL parameters filters:

* `cds_servicecontract_ids` - A space-separated list of `cds_servicecontract_id` values for which the Servers MUST filter the Service Contracts.
* `account_numbers` - A space-separated list of `account_number` values for which the Servers MUST filter the Service Contracts.
* `contract_numbers` - A space-separated list of `contract_number` values for which the Servers MUST filter the Service Contracts.
* `contract_statuses` - A space-separated list of `contract_status` values for which the Servers MUST filter the Service Contracts.
* `contract_types` - A space-separated list of `contract_type` values for which the Servers MUST filter the Service Contracts.
* `service_types` - A space-separated list of `service_type` values for which the Servers MUST filter the Service Contracts.
* `q` - A search term for which the Servers MUST filter the Service Contracts following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_servicecontract_id`
    * `account_number`
    * `contract_number`
    * `contract_address`
    * `rateplan_code`
    * `rateplan_name`
    * `program_number` in any included Service Program object

Listings of Service Contract objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Service Contract MUST be first in each listing.

## 12. Service Points API <a id="service-points-api" href="#service-points-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Service Point details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Service Points API.

### 12.1. Service Point Object Format <a id="service-point-format" href="#service-point-format" class="permalink">🔗</a>

Service Point objects are formatted as JSON objects and contain the following named values:

* `cds_servicepoint_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Service Point object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Service Point object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Service Point object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `servicepoint_number` - _[string](#string) or `null`_ - (REQUIRED) The identifier that a Customer sees on their bill or online user interface as the identifier for this Service Point.
  If a Server does not have a Customer-facing identifier for a Service Point and the Client is not authorized to see the Server's internal identifier for this Service Point, this value is `null`.
* `servicepoint_type` - _[ServicePointType](#TODO-service-point-types)_ - (REQUIRED) The type of service point that this Service Point represents.
* `servicepoint_address` - _[string](#string) or `null`_ - (OPTIONAL) The address that a Customer sees on their bill or online user interface as the address for this Service Point, if available.
  Strings MAY have linebreaks, such as when the address is multiple lines.
  If a Server does not have a Customer-facing address for a Service Point and the Client is not authorized to see the Server's internal address for this Service Point, this value is `null`.
* `latitude` - _[decimal](#decimal)_ - (OPTIONAL) The latitude that a Customer sees on their bill or online user interface as the latitude for this Service Point.
  If a Server does not have a Customer-facing latitude for a Service Point and the Client is not authorized to see the Server's internal latitude for this Service Point, or the Server does not have latitude stored for this Server Point, this value is `null`.
* `longitude` - _[decimal](#decimal)_ - (OPTIONAL) The longitude that a Customer sees on their bill or online user interface as the longitude for this Service Point.
  If a Server does not have a Customer-facing longitude for a Service Point and the Client is not authorized to see the Server's internal longitude for this Service Point, or the Server does not have longitude stored for this Server Point, this value is `null`.
* `current_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts that are currently providing a service to a Customer via this Service Point.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `previous_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts that have previously provided a service to a Customer via this Service Point.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `premises` - _Array[[Premise](#TODO-premise-format)]_ - (OPTIONAL) A list of related premise identifiers and details that a Customer sees on their bill or online user interface as the premises for this Service Point, if available.
  If a Server does not have any Customer-facing premises for a Service Point or the Client is not authorized to see the Server's internal premise details for this Service Point, or the Server does not have premises stored for this Server Point, this value is an empty list (`[]`).

### 12.2. Listing Service Points <a id="service-point-list" href="#service-point-list" class="permalink">🔗</a>

Clients may request to list Service Point objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Service Points, to the `cds_servicepoints_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Service Point listing request responses are formatted as JSON objects and contain the following named values.

* `service_points` - _Array[[ServicePoint](#service-point-format)]_ - (REQUIRED) A list of Service Points to which the requesting `access_token` is scoped to have access.
  If no Service Points are accessible, this value is an empty list (`[]`).
  If more than 100 Service Points are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Service Points.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Service Points.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Service Points.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Service Points to be the intersection of results for each of the URL parameters filters:

* `cds_servicepoint_ids` - A space-separated list of `cds_servicepoint_id` values for which the Servers MUST filter the Service Points.
* `servicepoint_numbers` - A space-separated list of `servicepoint_number` values for which the Servers MUST filter the Service Points.
* `current_servicecontracts` - A space-separated list of `current_servicecontracts` values for which the Servers MUST filter the Meter Devices.
* `previous_servicecontracts` - A space-separated list of `previous_servicecontracts` values for which the Servers MUST filter the Meter Devices.
* `q` - A search term for which the Servers MUST filter the Service Points following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_servicepoint_id`
    * `servicepoint_address`
    * `servicepoint_number`
    * `premise_number` in any included Premise object

Listings of Service Point objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Service Point MUST be first in each listing.

## 13. Meter Devices API <a id="meter-devices-api" href="#meter-devices-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Meter Device details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Meter Devices API.

### 13.1. Meter Device Object Format <a id="meter-device-format" href="#meter-device-format" class="permalink">🔗</a>

Meter Device objects are formatted as JSON objects and contain the following named values:

* `cds_meterdevice_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Meter Device object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Meter Device object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Meter Device object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `meter_number` - _[string](#string) or `null`_ - (OPTIONAL) The identifier that a Customer sees on their bill or online user interface or on the face of their physical meter as the identifier for this Meter Device.
  If a Server does not have a Customer-facing identifier for a Meter Device and the Client is not authorized to see the Server's internal identifier for this Meter Device, this value is `null`.
* `meter_type` - _[MeterType](#TODO-meter-types)_ - (REQUIRED) The type of metering device that this Meter Device represents.
* `current_servicepoints` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicepoint_id` values that identify Service Points where this Meter Device is currently installed or associated.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `previous_servicepoints` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicepoint_id` values that identify Service Points where this Meter Device was previously installed or associated.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.

### 13.2. Listing Meter Devices <a id="meter-device-list" href="#meter-device-list" class="permalink">🔗</a>

Clients may request to list Meter Device objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Meter Devices, to the `cds_meterdevices_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Meter Device listing request responses are formatted as JSON objects and contain the following named values.

* `meter_devices` - _Array[[MeterDevice](#meter-device-format)]_ - (REQUIRED) A list of Meter Devices to which the requesting `access_token` is scoped to have access.
  If no Meter Devices are accessible, this value is an empty list (`[]`).
  If more than 100 Meter Devices are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Meter Devices.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Meter Devices.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Meter Devices.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Meter Devices to be the intersection of results for each of the URL parameters filters:

* `cds_meterdevice_ids` - A space-separated list of `cds_meterdevice_id` values for which the Servers MUST filter the Meter Devices.
* `meter_number` - A space-separated list of `meter_number` values for which the Servers MUST filter the Meter Devices.
* `current_servicepoints` - A space-separated list of `current_servicepoint` values for which the Servers MUST filter the Meter Devices.
* `previous_servicepoints` - A space-separated list of `previous_servicepoints` values for which the Servers MUST filter the Meter Devices.
* `q` - A search term for which the Servers MUST filter the Meter Devices following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_meterdevice_id`
    * `meter_number`

Listings of Meter Device objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Meter Device MUST be first in each listing.

## 14. Bill Statements API <a id="bill-statements-api" href="#bill-statements-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Bill Statement details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Bill Statements API.

### 14.1. Bill Statement Object Format <a id="bill-statement-format" href="#bill-statement-format" class="permalink">🔗</a>

Bill Statement objects are formatted as JSON objects and contain the following named values:

* `cds_billstatement_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Bill Statement object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Bill Statement object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Bill Statement object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account for which this Bill Statement was issued.
* `account_number` - _[string](#string)_ - (REQUIRED) The Account's `account_number` at the time that this Bill Statement was issued.
* `statement_date` - _[date](#date)_ - (REQUIRED) The statement date that appears on the Customer-facing bill.
* `currency` - _[string](#string)_ - (REQUIRED) The currency for this Bill Statement's charge values as an [[ISO 4217](#ref-iso4217)] currency code.
* `file_uri` - _[URL](#url) or `null`_ - (OPTIONAL) A link to the raw bill statement file (e.g. the PDF bill) that was provided to the Customer, if available.
  If this Bill Statement was mailed to the Customer, this value is a link to the digital PDF copy of the mailed bill.
  Authentication for the linked file MUST be the same authentication as required for accessing this Bill Statement.
  If the Server does not have a raw bill statement file for the Bill Statement and the scope of access for a Client requires including this field, this field is include in the Bill Statement object with a value of `null`.
* `file_mimetype` - _[string](#string)_ - (OPTIONAL) The MIME type for the `file_uri`.
  This field is REQUIRED if the `file_uri` field is populated with a URL.
* `previous_balance_amount` - _[decimal](#decimal)_ - (OPTIONAL) The value shown on the Customer-facing bill statement as the previous balance for the Account, if available.
* `previous_balance_paid` - _[decimal](#decimal)_ - (OPTIONAL) The value shown on the Customer-facing bill statement as the previous balance paid for the Account, if available.
* `previous_balance_unpaid` - _[decimal](#decimal)_ - (OPTIONAL) The value shown on the Customer-facing bill statement as the previous balance unpaid for the Account, if available.
* `new_charges_amount` - _[decimal](#decimal)_ - (OPTIONAL) The value shown on the Customer-facing bill statement as the new credits and charges total, if available.
* `new_charges` - _Array[[NewCharge](#new-charge-format)]_ - (OPTIONAL) A list of new overall charges shown on the Customer-facing bill statement, if available.
* `new_balance_amount` - _[decimal](#decimal)_ - (OPTIONAL) The value shown on the Customer-facing bill statement as the new Account balance, if available.
* `amount_due` - _[decimal](#decimal)_ - (REQUIRED) The value shown on the Customer-facing bill statement as the amount due, if available.
* `amount_due_date` - _[date](#date)_ - (REQUIRED) The value shown on the Customer-facing bill statement as the date that the `amount_due` value is due, if available.
* `shutoff_prevention_minimum_due` - _[decimal](#decimal) or `null`_ - (REQUIRED) The value shown on the Customer-facing bill statement as the minimum amount due to prevent service shutoff, if available.
  If there is no shutoff notice on the Customer-facing bill statement, this value is `null`.
* `shutoff_prevention_date` - _[date](#date) or `null`_ - (REQUIRED) The value shown on the Customer-facing bill statement as the date that the `shutoff_prevention_minimum_due` value is due, if available.
  If `shutoff_prevention_minimum_due` is `null`, this value is also `null`.
* `program_participations` - _Array[[ProgramParticipation](#TODO-program-participation-format)]_ - (OPTIONAL) A list of Account-level programs in which the Customer is participating for the Bill Statement period.
  If the Server does not have this information or the Customer is not participating in any Account-level programs, this value is and empty list (`[]`).

### 14.2. Listing Bill Statements <a id="bill-statement-list" href="#bill-statement-list" class="permalink">🔗</a>

Clients may request to list Bill Statement objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Bill Statements, to the `cds_billstatement_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Bill Statement listing request responses are formatted as JSON objects and contain the following named values.

* `bill_statements` - _Array[[BillStatement](#bill-statement-format)]_ - (REQUIRED) A list of Bill Statements to which the requesting `access_token` is scoped to have access.
  If no Bill Statements are accessible, this value is an empty list (`[]`).
  If more than 100 Bill Statements are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Bill Statements.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Bill Statements.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Bill Statements.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Bill Statements to be the intersection of results for each of the URL parameters filters:

* `cds_billstatement_ids` - A space-separated list of `cds_billstatement_id` values for which the Servers MUST filter the Bill Statements.
* `before` - A [date](#date) for which the Server MUST filter the Bill Statement `statement_date` to values that are on or before this date.
* `after` - A [date](#date) for which the Server MUST filter the Bill Statement `statement_date` to values that are on or after this date.
* `cds_account_ids` - A space-separated list of `cds_account_id` values for which the Servers MUST filter the Bill Statements.
* `account_numbers` - A space-separated list of `account_number` values for which the Servers MUST filter the Bill Statements.

Listings of Bill Statement objects MUST be ordered in reverse chronological order by `statement_date` date, where the most recent dated relevant Bill Statement MUST be first in each listing.
In situations where relevant Bill Statement objects have the same `statement_date`, those Bill Statements with the same `statement_date` are further ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Bill Statement is listed first.

## 15. Bill Sections API <a id="bill-sections-api" href="#bill-sections-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Bill Section details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Bill Sections API.

### 15.1. Bill Section Object Format <a id="bill-section-format" href="#bill-section-format" class="permalink">🔗</a>

Bill Section objects are formatted as JSON objects and contain the following named values:

* `cds_billsection_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Bill Section object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Bill Section object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Bill Section object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `cds_billstatement_id` - _[string](#string) or `null`_ - (REQUIRED) The Bill Statement for which this Bill Section was issued.
  If the Client is not authorized to see the related Bill Statement, this value is `null`.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account for which this Bill Section was issued.
* `account_number` - _[string](#string)_ - (REQUIRED) The Account's `account_number` at the time that this Bill Section was issued.
* `section_type` - _[BillSectionType](#TODO-bill-section-types)_ - (REQUIRED) The type of bill section that this Bill Section represents.
* `start_date` - _[date](#date)_ - (REQUIRED) The start date of services provided for which this Bill Section covers.
* `end_date` - _[date](#date)_ - (REQUIRED) The end date of services provided for which this Bill Section covers.
* `currency` - _[string](#string)_ - (REQUIRED) The currency for this Bill Section's charge values as an [[ISO 4217](#ref-iso4217)] currency code.
* `distribution_entity` - _[DistributionEntity](#TODO-distribution-entity-format) or `null`_ - (REQUIRED) The details for the utility or other entity providing distribution for the services that this Bill Section covers.
  If the services provided that are covered by this Bill Section do not have distribution, this value is `null`.
* `load_serving_entity` - _[LoadServingEntity](#TODO-load-serving-entity-format) or `null`_ - (REQUIRED) The details for the utility or other entity providing distribution for the services that this Bill Section covers.
  If the services provided that are covered by this Bill Section do not have a supply or retailer component, this value is `null`.
  If the load serving entity is the same as the distributor, the Load Serving Entity `type` value MUST be `distributor` and no other fields are populated in the Load Serving Entity object.
* `related_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts to which this Bill Section is applicable.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `related_servicepoints` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicepoint_id` values that identify Service Points to which this Bill Section is applicable.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `related_meterdevices` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_meterdevice_id` values that identify Meter Devices to which this Bill Section is applicable.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `related_billsections` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_billsection_id` that identify other Bill Sections that were issued alongside this Bill Section, where those Bill Sections represent another set of charges covering the same Service Contract.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `usage_summary` - _Array[[BillSectionUsageSummary](#TODO-bill-section-usage-summary-format)]_ - (REQUIRED) A list of the Bill Section Usage Summary objects that detail the service's usage and other values on the Customer-facing bill statement section represented by this Bill Section.
* `cost_summary` - _Array[[BillSectionCostSummary](#TODO-bill-section-cost-summary-format)]_ - (REQUIRED) A list of the Bill Section Cost Summary objects that detail the service's total and subtotal costs on the Customer-facing bill statement section represented by this Bill Section.
* `line_items` - _Array[[BillSectionLineItem](#TODO-bill-section-line-item-format)]_ - (OPTIONAL) A list of the Bill Section Line Item objects that denote the individual charges listed on the Customer-facing bill statement section represented by this Bill Section.

### 15.2. Listing Bill Sections <a id="bill-section-list" href="#bill-section-list" class="permalink">🔗</a>

Clients may request to list Bill Section objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Bill Sections, to the `cds_billsection_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Bill Section listing request responses are formatted as JSON objects and contain the following named values.

* `bill_sections` - _Array[[BillSection](#bill-section-format)]_ - (REQUIRED) A list of Bill Sections to which the requesting `access_token` is scoped to have access.
  If no Bill Sections are accessible, this value is an empty list (`[]`).
  If more than 100 Bill Sections are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Bill Sections.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Bill Sections.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Bill Sections.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Bill Sections to be the intersection of results for each of the URL parameters filters:

* `cds_billsection_ids` - A space-separated list of `cds_billsection_id` values for which the Servers MUST filter the Bill Sections.
* `before` - A [date](#date) for which the Server MUST filter the Bill Section `start_date` to values that are on or before this date.
* `after` - A [date](#date) for which the Server MUST filter the Bill Section `end_date` to values that are on or after this date.
* `cds_billstatement_ids` - A space-separated list of `cds_billstatement_id` values for which the Servers MUST filter the Bill Sections.
* `cds_account_ids` - A space-separated list of `cds_account_id` values for which the Servers MUST filter the Bill Sections.
* `account_numbers` - A space-separated list of `account_number` values for which the Servers MUST filter the Bill Sections.
* `cds_servicecontract_ids` - A space-separated list of `related_servicecontracts` values for which the Servers MUST filter the Bill Sections.
* `contract_numbers` - A space-separated list of `related_servicecontracts` listed Service Contract `contract_number` values for which the Servers MUST filter the Bill Sections.

Listings of Bill Section objects MUST be ordered in alphanumeric order by `account_number`.
In situations where relevant Bill Section objects have the same `account_number`, those Bill Sections with the same `account_number` are further ordered in alphanumeric order by `contract_number`.
In situations where relevant Bill Section objects have the same `account_number` and `contract_number`, those Bill Sections with the same `account_number` and `contract_number` are further ordered in chronological order by `start_date`, where the relevant Bill Section with the earliest date is listed first.
In situations where relevant Bill Section objects have the same `account_number`, `contract_number`, and `start_date`, those Bill Sections with the same `account_number`, `contract_number`, and `start_date` are further ordered in reverse chronological order by `modified`, where the most recently updated relevant Bill Section is listed first.

## 16. Aggregations API <a id="aggregations-api" href="#aggregations-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Server's Aggregation objects for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Aggregations API.

### 16.1. Aggregation Object Format <a id="aggregation-format" href="#aggregation-format" class="permalink">🔗</a>

Aggregation objects are formatted as JSON objects and contain the following named values:

* `cds_aggregation_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Aggregation object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Aggregation object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Aggregation object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `aggregation_type` - _[AggregationType](#TODO-aggregation-types)_ - (REQUIRED) The type of aggregation that this Aggregation represents.
* `aggregation_number` - _[string](#string) or `null`_ - (REQUIRED) The aggregation identifier that a Client sees in Server documentation or other online interfaces as the aggregation identifier for this Aggregation, if available.
  If a Server does not have a Client-facing aggregation identifier for an Aggregation and the Client is not authorized to see the Server's internal aggregation identifier for this Aggregation, or the Server does not have aggregation identifiers stored for this Aggregation, this value is `null`.
* `grouped_accounts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_account_id` values that identify Accounts that are part of this Aggregation.
  For Aggregations that do not have or the Client does not have authorization to access Accounts that are part of the Aggregation, this is an empty array (`[]`).
* `grouped_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts that are part of this Aggregation.
  For Aggregations that do not have or the Client does not have authorization to access Service Contracts that are part of the Aggregation, this is an empty array (`[]`).
* `grouped_servicepoints` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicepoint_id` values that identify Service Points that are part of this Aggregation.
  For Aggregations that do not have or the Client does not have authorization to access Service Points that are part of the Aggregation, this is an empty array (`[]`).
* `grouped_meterdevices` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_meterdevice_id` values that identify Meter Devices that are part of this Aggregation.
  For Aggregations that do not have or the Client does not have authorization to access Meter Devices that are part of the Aggregation, this is an empty array (`[]`).
* `grouped_aggregations` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_aggregation_id` values that identify other Aggregations that are part of this Aggregation.
  For Aggregations that do not have or the Client does not have authorization to access other sub Aggregations that are part of the parent Aggregation, this is an empty array (`[]`).
* `grouped_addresses` - _Array[[string](#string)]_ - (OPTIONAL) A list of addresses that are associated with this Aggregation, if applicable for the `aggregation_type`.
  Strings MAY have line breaks, such as when the address is multiple lines.
  For Aggregations that do not have or the Client does not have authorization to access addresses that are part of the parent Aggregation, this is an empty array (`[]`).
* `grouped_regions` - _Array[[AggregationRegion](#TODO-aggregation-region-format)]_ - (REQUIRED) A list of Aggregation Regions that are associated with this Aggregation, if applicable for the `aggregation_type`.
  For Aggregations that do not have or the Client does not have authorization to access regions that are part of the parent Aggregation, this is an empty array (`[]`).
* `consents_required` - _Array[[ConsentRequirement](#TODO-consent-requirement-format)]_ - (REQUIRED) A list of Consent Requirements that are required for this Aggregation, if applicable for the `aggregation_type`.
  If there are no consent requirements for this Aggregation, this value is an empty array (`[]`).

For Aggregations that have more than 1000 grouped items in any of the Aggregation object's `grouped_*` arrays, so as to not create overly large individual Aggregation objects, the Server MUST create Aggregation objects that group the items in groups of 1000 or fewer items, then include those Aggregations in the original Aggregation's `groupd_aggregations` array.
The intent of this requirement is to enable Servers to use the [Listing Aggregations](#aggregation-list) API as a means for breaking up and paginating very large groupings of Account, Service Contract, Service Point, Meter Devices, or Aggregation objects.

### 16.2. Listing Aggregations <a id="aggregation-list" href="#aggregation-list" class="permalink">🔗</a>

Clients may request to list Aggregation objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Aggregations, to the `cds_aggregations_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Aggregation listing request responses are formatted as JSON objects and contain the following named values.

* `aggregations` - _Array[[Aggregation](#aggregation-format)]_ - (REQUIRED) A list of Aggregations to which the requesting `access_token` is scoped to have access.
  If no Aggregations are accessible, this value is an empty list (`[]`).
  If more than 100 Aggregations are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Aggregations.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Aggregations.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Aggregations.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Aggregations to be the intersection of results for each of the URL parameters filters:

* `cds_aggregation_ids` - A space-separated list of `cds_aggregation_id` values for which the Servers MUST filter the Aggregations.
* `aggregation_numbers` - A space-separated list of `aggregation_number` values for which the Servers MUST filter the Aggregations.
* `aggregation_types` - A space-separated list of `aggregation_type` values for which the Servers MUST filter the Aggregations.
* `q` - A search term for which the Servers MUST filter the Aggregations following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_aggregation_id`
    * `aggregation_number`
    * `addresses`
    * `region_number` in any included Aggregation Region object

Listings of Aggregation objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently updated relevant Aggregation MUST be first in each listing.

## 17. Usage Segments API <a id="usage-segments-api" href="#usage-segments-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Usage Segment objects for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Usage Segments API.

### 17.1. Usage Segment Object Format <a id="usage-segment-format" href="#usage-segment-format" class="permalink">🔗</a>

Usage Segment objects are formatted as JSON objects and contain the following named values:

* `cds_usagesegment_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Usage Segment object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Usage Segment object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Usage Segment object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `related_aggregations` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_aggregation_id` values that identify Aggregations to which this Usage Segment is applicable.
* `related_accounts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_account_id` values that identify Accounts to which this Usage Segment is applicable.
* `related_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts to which this Usage Segment is applicable.
* `related_servicepoints` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicepoint_id` values that identify Service Points to which this Usage Segment is applicable.
* `related_meterdevices` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_meterdevice_id` values that identify Meter Devices to which this Usage Segment is applicable.
* `related_billsections` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_billsection_id` values that identify Bill Sections to which this Usage Segment is applicable.
* `segment_start` - _[datetime](#datetime)_ - (REQUIRED) The starting timestamp for the list of Usage Segment `values`.
* `segment_end` - _[datetime](#datetime)_ - (REQUIRED) The ending timestamp for the list of Usage Segment `values`.
  For values that represent a duration (e.g. 15-min usage interval reading), the `segment_end` MUST represent the timestamp at the end of the duration.
* `interval` - _[decimal](#decimal)_ - (REQUIRED) How long between values, in seconds.
  For values representing a duration interval (e.g. kwh usage intervals), this `interval` represents the duration of each interval, where the next value starts immediately at the end of the prior interval value with no time gap.
  For values representing instantaneous readings (e.g. register read), this `interval` represents the time between the readings provided in the `values`.
  For usage readings that have varying duration, such as monthly usage readings, Servers MUST create individual Usage Point objects that have an `interval` value with the duration of the specific period (e.g. month) and a single Value Set entry in their `values` array.
* `format` - _Array[[ValueFormat](#usage-segment-value-format)]_ - (REQUIRED) For each entry in `values`, these are the formats of that are included, presented in the order in which the values are presented.
* `values` - _Array[[ValueSet](#usage-segment-value-set-format)]_ - (REQUIRED) A list of Value Sets that represent the entries included in the Usage Segment.

Servers MUST only include unique identifiers in `related_aggregations`, `related_accounts`, `related_servicecontracts`, `related_servicepoints`, `related_meterdevices`, and `related_billsections` lists MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.

### 17.2. Usage Segment Value Formats <a id="usage-segment-value-format" href="#usage-segment-value-format" class="permalink">🔗</a>

The Usage Segment `format` field provides an ordered list of strings that denote the object types that are included in each Usage Segment `values` entry's [Value Set](#usage-segment-value-set-format).
Usage Segments are organized this way so that the `format` listing essentially provides a "schema" of object types to expect when parsing the `values` Value Sets, which removes the need for each object in each Value Set to include repeated fields, such as an object type value.

The following Usage Segment format strings are specified with their corresponding Value Set object types:

* `usage_kwh` - [General Electric Usage in kWh object](#TODO-usage-segment-value-usage-kwh)
* `usage_fwd_kwh` - [Forward Electric Usage in kWh object](#TODO-usage-segment-value-usage-fwd-kwh)
* `usage_rev_kwh` - [Reverse Electric Usage in kWh object](#TODO-usage-segment-value-usage-rev-kwh)
* `usage_net_kwh` - [Net Electric Usage in kWh object](#TODO-usage-segment-value-usage-net-kwh)
* `aggregated_kwh` - [Aggregated Electric Usage in kWh object](#TODO-usage-segment-value-aggregated-kwh)
* `demand_kw` - [Electric Demand in kW object](#TODO-usage-segment-value-demand-kw)
* `peak_kw` - [Electric Peak Demand in kW object](#TODO-usage-segment-value-peak-kw)
* `water_m3` - [Water Usage in Cubic Meters object](#TODO-usage-segment-value-water-m3)
* `water_gal` - [Water Usage in Gallons object](#TODO-usage-segment-value-water-gal)
* `water_ft3` - [Water Usage in Cubic Feet object](#TODO-usage-segment-value-water-ft3)
* `gas_therm` - [Natural Gas Usage in Therms object](#TODO-usage-segment-value-gas-therm)
* `gas_ccf` - [Natural Gas Usage in CCF object](#TODO-usage-segment-value-gas-ccf)
* `gas_mcf` - [Natural Gas Usage in MCF object](#TODO-usage-segment-value-gas-mcf)
* `gas_mmbtu` - [Natural Gas Usage in mmBTU object](#TODO-usage-segment-value-gas-mmbtu)
* `supply_mix` - [Electric Supply Mix object](#TODO-usage-segment-value-supply-mix)
* `eacs` - [Energy Attribute Certificates object](#TODO-usage-segment-value-eacs)

Value format strings MAY be repeated in the `format` listing, indicating there are multiple value objects in the Value Set for that format.
This is useful when a Server provides multiple variations of optional fields for the same type of value.

Extensions to this specification MAY further define additional formats other that what is listed above.
Client MUST ignore format values they do not know how to interpret.

### 17.3. Usage Segment Value Set Format <a id="usage-segment-value-set-format" href="#usage-segment-value-set-format" class="permalink">🔗</a>

The Usage Segment `values` field provides an ordered list of Value Set entries.
A Value Set entry is an array that contains an ordered list of [Value objects](#usage-segment-value-object-format), where the type of each Value object which is defined by the order of the `format` listing.

Each Value Set entry in the `values` listing represents an increment of `interval` seconds in the Usage Segment from the `segment_start`.
For example, if a Usage Segment has a `segment_start` of `2025-01-01T00:00:00Z`, an `interval` of `900`, and four Value Set entries in the `values`, the Value Set entries would represent the each 15 minute period of a one hour segment (`2025-01-01T00:00:00Z - 2025-01-01T00:15:00Z`, `2025-01-01T00:15:00Z - 2025-01-01T00:30:00Z`, `2025-01-01T00:30:00Z - 2025-01-01T00:45:00Z`, and `2025-01-01T00:45:00Z - 2025-01-01T01:00:00Z`).

If a Value object is not available for a specific Value Set.
Servers MUST replace the item in the Value Set with `null`.

### 17.4. Usage Segment Value Object Format <a id="usage-segment-value-object-format" href="#usage-segment-value-object-format" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 17.5. Listing Usage Segments <a id="usage-segment-list" href="#usage-segment-list" class="permalink">🔗</a>

Clients may request to list Usage Segment objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of Usage Segments, to the `cds_usagesegments_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The Usage object listing request responses are formatted as JSON objects and contain the following named values.

* `usage_segments` - _Array[[UsageSegment](#usage-segment-format)]_ - (REQUIRED) A list of Usage Segments to which the requesting `access_token` is scoped to have access.
  If no Usage Segments are accessible, this value is an empty list (`[]`).
  If more than 100 Usage Segments are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of Usage Segments.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of Usage Segments.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of Usage Segments.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of Usage Segments to be the intersection of results for each of the URL parameters filters:

* `cds_usagesegment_ids` - A space-separated list of `cds_usagesegment_id` values for which the Servers MUST filter the Usage Segments.
* `before` - A [datetime](#datetime) for which the Server MUST filter the Usage Segment object `segment_start` to values that are on or before this date.
* `after` - A [datetime](#datetime) for which the Server MUST filter the Usage Segment object `segment_end` to values that are on or after this date.
* `cds_account_ids` - A space-separated list of `cds_account_id` values for which the Servers MUST filter the Usage Segments.
* `account_numbers` - A space-separated list of `account_number` values for which the Servers MUST filter the Usage Segments.
* `cds_servicecontract_ids` - A space-separated list of `cds_servicecontract_id` values for which the Servers MUST filter the Usage Segments.
* `contract_numbers` - A space-separated list of `contract_number` values for which the Servers MUST filter the Usage Segments.
* `cds_servicepoint_ids` - A space-separated list of `cds_servicepoint_id` values for which the Servers MUST filter the Usage Segments.
* `servicepoint_numbers` - A space-separated list of `servicepoint_number` values for which the Servers MUST filter the Usage Segments.
* `cds_meterdevice_ids` - A space-separated list of `cds_meterdevice_ids` values for which the Servers MUST filter the Usage Segments.
* `meter_numbers` - A space-separated list of `meter_numbers` values for which the Servers MUST filter the Usage Segments.
* `cds_billsection_ids` - A space-separated list of `cds_billsection_id` values for which the Servers MUST filter the Usage Segments.

Listings of Usage Segments MUST be ordered in alphanumeric order by `aggregation_number`.
In situations where relevant Usage Segments have the same `aggregation_number`, those Usage Segments with the same `aggregation_number` are further ordered in alphanumeric order by `account_number`.
In situations where relevant Usage Segments have the same `account_number`, those Usage Segments with the same `account_number` are further ordered in alphanumeric order by `contract_number`.
In situations where relevant Usage Segments have the same `contract_number`, those Usage Segments with the same `contract_number` are further ordered in chronological order by `segment_start`, where the relevant Usage Segment object with the earliest date is listed first.
In situations where relevant Usage Segments have the same `segment_start`, those Usage Segments with the same `segment_start` are further ordered in reverse chronological order by `cds_modified`, where the most recently updated relevant Usage Segment object is listed first.

## 18. Energy Attribute Certificates API <a id="eac-api" href="#eac-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Energy Attribute Certificate (EAC) objects for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#scopes) that allows access to the Energy Attribute Certificates API.

### 18.1. Energy Attribute Certificate Object Format <a id="eac-format" href="#eac-format" class="permalink">🔗</a>

EAC objects are formatted as JSON objects and contain the following named values:

* `cds_eac_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this EAC object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this EAC object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this EAC object, except for the `cds_synced` value.
* `cds_synced` - _[datetime](#datetime)_ - (REQUIRED) When the data in this object was last confirmed to be valid against the Server's source of truth (e.g. the utility's backend customer database).
  If the Server directly queries the source of truth when returning data for this object, this value is the current time.
  If the Server caches the data for this object and periodically synchronizes it with the Server's source of truth (e.g. nightly batch updates), this value is the last time this object's Customer Data was updated or validated.
* `beneficiary_type` - _[BeneficiaryType](#eac-beneficiary-types)_ - (REQUIRED) The type of beneficiary to which this EAC is applied.
* `beneficiaries` - _Array[[string](#string)]_ - (REQUIRED) The list of identifiers to which the EAC is allocated, based on the `beneficiary_type`.
* `eac_numbers` - _Array[[string](#string)]_ - (REQUIRED) A list of EAC identifier that a Client or Customer sees in Server documentation, Customer documents, Certificate Registries, or other interfaces as the identifier for this EAC, if available.
  If a Server does not have a Client or Customer-facing EAC identifier and the Client is not authorized to see the Server's internal EAC identifier for this EAC, or the Server does not have identifiers stored for this EAC, this value is an emtpy list (`[]`).
* `eac_format` - _[string](#string)_ - (REQUIRED) The format of the EAC data linked in the `ead_data_url`.
  Possible values of this format MUST be included in the [Server Metadata's](#server-metadata) `cds_eac_formats` object.
  Clients MUST ignore `eac_format` values that they do not know how to interpret.
* `eac_data_url` - _[URL](#url)_ - (REQUIRED) A link to an endpoint containing the EAC's data (e.g. asset identifiers, values, locations, etc.).
  This is a URL, rather than a structured object because there are many EAC formats available across regions, registries, and jursidictions, and rather than this specification attempting to accomodate all EAC formats, Servers only need to link to the EAC data in the format they have the data accessible.
  URL endpoints provided in this field MUST be accessible under the same authentication and security requirements as accessing this specification's EAC API endpoints.
  URLs may link to an individual formatted file, compressed archive files, the base URL to a set of APIs, or any other type of data location, as long as the data is formatted in the manner described by the `eac_format` field.
  In situations where the URL is the base URL to another set of APIs that contain the EAC data, Servers MUST scope the access to that API to only the relevant EAC data for the specific Grant that the `access_token` provides.
* `period_start` - _[datetime](#datetime)_ - (REQUIRED) When the EAC coverage time period started.
* `period_end` - _[datetime](#datetime)_ - (REQUIRED) When the EAC coverage time period ended or will end.

### 18.2. Beneficiary Types <a id="eac-beneficiary-types" href="#eac-beneficiary-types" class="permalink">🔗</a>

EAC object `beneficiary_type` values MUST be one of the following:

* `customer` - Within the EAC's destination, the EAC is further applied to an individual Power Purchase Agreement (PPA) for a set of Customers.
  When the `beneficiary_type` is `customer`, the `beneficiaries` MUST have values of the relevant `customer_number` values.
  NOTE: listed `customer_number` values MUST be authorized to be visible to the Client, so while the destination MAY allocate beneficiaries of the EAC to multiple customers, only the customer numbers that are available to be seen as part of the Client's authorization MUST be listed.
* `account` - Within the EAC's destination, the EAC is further applied to an individual Power Purchase Agreement (PPA) for a set of Customer accounts.
  When the `beneficiary_type` is `account`, the `beneficiaries` MUST have values of the relevant `cds_account_id` values.
  NOTE: listed `cds_account_id` values MUST be authorized to be visible to the Client, so while the destination MAY allocate beneficiaries of the EAC to multiple accounts, only the accounts that are available to be seen as part of the Client's authorization MUST be listed.
* `servicecontract` - Within the EAC's destination, the EAC is further applied to an individual Power Purchase Agreement (PPA) for a set of Customer service contracts.
  When the `beneficiary_type` is `servicecontract`, the `beneficiaries` MUST have values of the relevant `cds_servicecontract_id` values.
  NOTE: listed `cds_servicecontract_id` values MUST be authorized to be visible to the Client, so while the destination MAY allocate beneficiaries of the EAC to multiple contracts, only the service contracts that are available to be seen as part of the Client's authorization MUST be listed.
* `rateplan` - Within the EAC's destination, the EAC is further applied to a rate plan or tariff for the customers with service contracts on that rate plan.
  When the `beneficiary_type` is `rateplan`, the `beneficiaries` MUST have the value of the relevant `rateplan_code` values.
  NOTE: listed `rateplan_code` values MUST be authorized to be visible to the Client, so while the destination MAY allocate beneficiaries of the EAC to multiple rate plans or tariffs, only the rate plans that are available to be seen as part of the Client's authorization MUST be listed.
* `program` - Within the EAC's destination, the EAC is further applied to a special program in which customers are participating.
  When the `beneficiary_type` is `program`, the `beneficiaries` MUST have the value of the relevant `program_id` values.
  NOTE: listed `program_id` values MUST be authorized to be visible to the Client, so while the destination MAY allocate beneficiaries of the EAC to multiple special programs, only the rate plans that are available to be seen as part of the Client's authorization MUST be listed.
* `general` - Within the EAC's destination, the EAC is applied generally to the base energy usage profile for all customers.
  When the `beneficiary_type` is `general`, the `beneficiaries` MUST be an empty list (`[]`).

### 18.3. EAC Data Format Description Object <a id="eac-data-format-descriptions" href="#eac-data-format-descriptions" class="permalink">🔗</a>

EAC Data Format Descriptions objects are formatted as JSON objects and contain the following named values:

* `id` - _[string](#string)_ - (REQUIRED) The identifier of the EAC data format that is set as the [EAC object](#eac-format) `eac_format` value when the EAC object links to EAC data in the `eac_data_url` that is structured in this format.
* `name` - _[string](#string)_ - (REQUIRED) A human-readable name of the EAC data format (e.g. "Energy Tag API").
* `description` - _[string](#string)_ - (REQUIRED) A human-readable description of the EAC data format.
* `documentation` - _[URL](#url)_ - (REQUIRED) A link to developer documentation on the EAC data format.

### 18.4. Listing Energy Attribute Certificates <a id="eac-list" href="#eac-list" class="permalink">🔗</a>

Clients may request to list EAC objects that they have access to by making an HTTPS `GET` request, authenticated with a valid Bearer `access_token` that is scoped to provide access to a set of EACs, to the `cds_eacs_api` URL included in the [Client Registration Response](https://cds-registration.lfenergy.org/specs/cds-wg1-02#registration-response) or [Clients API](https://cds-registration.lfenergy.org/specs/cds-wg1-02#client-format).
The EAC object listing request responses are formatted as JSON objects and contain the following named values.

* `eacs` - _Array[[EnergyAttributeCredit](#eac-format)]_ - (REQUIRED) A list of EACs to which the requesting `access_token` is scoped to have access.
  If no EACs are accessible, this value is an empty list (`[]`).
  If more than 100 EACs are available to be listed, Servers MAY truncate the list and use the `next` value to link to the next segment of the list of EACs.
* `next` - _[URL](#url)_ or `null` - Where to request the next segment of the list of EACs.
  If no next segment exists (i.e. the requester is at the end of the list), this value is `null`.
* `previous` - _[URL](#url)_ or `null` - Where to request the previous segment of the list of EACs.
  If no previous segment exists (i.e. the requester is at the front of the list), this value is `null`.

Servers MUST support Clients adding any of the following URL parameters to the `GET` request, which will filter the list of EACs to be the intersection of results for each of the URL parameters filters:

* `cds_eac_ids` - A space-separated list of `cds_eac_id` values for which the Servers MUST filter the EACs.
* `eac_numbers` - A space-separated list of `eac_numbers` values for which the Servers MUST filter the EACs.
* `beneficiaries` - A space-separated list of `beneficiaries` values for which the Servers MUST filter the EACs.
  Since `beneficiaries` contains values based on the `beneficiary_type`, different types of beneficiaries may have the same value (e.g. `customer_number` and `account_number` may be the same).
  For listings returned using this filter parameter, Clients SHOULD check the `beneficiary_type` for each returned result to filter out any results that may have been inadvertent inclusions due to value collision on the Server.
* `before` - A [datetime](#datetime) for which the Server MUST filter the EAC object `period_start` to values that are on or before this datetime.
* `after` - A [datetime](#datetime) for which the Server MUST filter the EAC object `period_end` to values that are on or after this datetime.

Listings of EACs MUST be ordered in reverse chronological order by `period_start`, where the most recently started EAC is first.
In situations where relevant EACs have the same `period_start`, those EACs with the same `period_start` are further ordered in reverse chronological order by `cds_created`, where the most recently created EAC is first.
In situations where relevant EACs have the same `period_start` and `cds_created`, those EACs with the same `period_start` and `cds_created` are further ordered in alphanumeric order by `cds_eac_id`.

## 19. Extensions <a id="extensions" href="#extensions" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 20. Security Considerations <a id="security" href="#security" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 21. Examples <a id="examples" href="#examples" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 22. References <a id="references" href="#references" class="permalink">🔗</a>

<a id="ref-bcp14" href="#ref-bcp14" class="permalink">🔗</a>
`BCP 14` - "Best Current Practice 14", Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/info/bcp14](https://www.rfc-editor.org/info/bcp14)

<a id="ref-cds-wg1-02" href="#ref-cds-wg1-02" class="permalink">🔗</a>
`CDS-WG1-02` - "Client Registration", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/](https://cds-registration.lfenergy.org/specs/cds-wg1-02/)

<a id="ref-cds-wg1-02-metadata" href="#ref-cds-wg1-02-metadata" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.2` - "Metadata Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#auth-server-metadata-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#auth-server-metadata-format)

<a id="ref-cds-wg1-02-scopes" href="#ref-cds-wg1-02-scopes" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.3` - "Scopes Supported", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scopes](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scopes)

<a id="ref-cds-wg1-02-scope-descriptions" href="#ref-cds-wg1-02-scope-descriptions" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.4` - "Scope Descriptions Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scope-descriptions-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scope-descriptions-format)

<a id="ref-cds-wg1-02-registration-fields" href="#ref-cds-wg1-02-registration-fields" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.5` - "Registration Field Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#registration-field-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#registration-field-format)

<a id="ref-cds-wg1-02-auth-details-object" href="#ref-cds-wg1-02-auth-details-object" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.8` - "Authorization Details Field Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#auth-details-field-formats](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#auth-details-fields-format)

<a id="ref-cds-wg1-02-client-object" href="#ref-cds-wg1-02-client-object" class="permalink">🔗</a>
`CDS-WG1-02 Section 5.1` - "Client Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#client-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#client-format)

<a id="ref-cds-wg1-02-grant-object" href="#ref-cds-wg1-02-grant-object" class="permalink">🔗</a>
`CDS-WG1-02 Section 8.1` - "Grant Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#grant-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#grant-format)

<a id="ref-cds-wg1-02-grant-auth-requests" href="#ref-cds-wg1-02-grant-auth-requests" class="permalink">🔗</a>
`CDS-WG1-02 Section 8.3` - "Grant Authorization Requests", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#grant-authorization-requests](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#grant-authorization-requests)

<a id="ref-iso4217" href="#ref-iso4217" class="permalink">🔗</a>
`ISO 4217` - "Currency Codes", ISO 4217, International Organization for Standardization (ISO),  
[https://www.iso.org/iso-4217-currency-codes.html](https://www.iso.org/iso-4217-currency-codes.html)

<a id="ref-rfc3339-datetime" href="#ref-rfc3339-datetime" class="permalink">🔗</a>
`RFC 3339 Section 5.6` - Section 5.6. Internet Date/Time Format, "Date and Time on the Internet: Timestamps", RFC 3339, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc3339#section-5.6](https://www.rfc-editor.org/rfc/rfc3339#section-5.6)

<a id="ref-rfc3986-url" href="#ref-rfc3986-url" class="permalink">🔗</a>
`RFC 3986 Section 1.1.3` - Section 1.1.3. URI, URL, and URN, "Uniform Resource Identifier (URI): Generic Syntax", RFC 3986, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc3986#section-1.1.3](https://www.rfc-editor.org/rfc/rfc3986#section-1.1.3)

<a id="ref-rfc6749-code-grant" href="#ref-rfc6749-code-grant" class="permalink">🔗</a>
`RFC 6749 Section 4.1` - Section 4.1. Authorization Code Grant, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.1](https://www.rfc-editor.org/rfc/rfc6749#section-4.1)

<a id="ref-rfc6749-client-credentials" href="#ref-rfc6749-client-credentials" class="permalink">🔗</a>
`RFC 6749 Section 4.4` - Section 4.4. Client Credentials Grant, "The OAuth 2.0 Authorization Framework", RFC 6749, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6749#section-4.4](https://www.rfc-editor.org/rfc/rfc6749#section-4.4)

<a id="ref-rfc6838" href="#ref-rfc6838" class="permalink">🔗</a>
`RFC 6838` - "Media Type Specifications and Registration Procedures", RFC 6838, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc6838](https://www.rfc-editor.org/rfc/rfc6838)

<a id="ref-rfc8259" href="#ref-rfc8259" class="permalink">🔗</a>
`RFC 8259` - "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259](https://www.rfc-editor.org/rfc/rfc8259)

<a id="ref-rfc8259-values" href="#ref-rfc8259-values" class="permalink">🔗</a>
`RFC 8259 Section 3` - Section 3. Values, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-3](https://www.rfc-editor.org/rfc/rfc8259#section-3)

<a id="ref-rfc8259-objects" href="#ref-rfc8259-objects" class="permalink">🔗</a>
`RFC 8259 Section 4` - Section 4. Objects, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-4](https://www.rfc-editor.org/rfc/rfc8259#section-4)

<a id="ref-rfc8259-arrays" href="#ref-rfc8259-arrays" class="permalink">🔗</a>
`RFC 8259 Section 5` - Section 5. Arrays, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-5](https://www.rfc-editor.org/rfc/rfc8259#section-5)

<a id="ref-rfc8259-numbers" href="#ref-rfc8259-numbers" class="permalink">🔗</a>
`RFC 8259 Section 6` - Section 6. Numbers, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-6](https://www.rfc-editor.org/rfc/rfc8259#section-6)

<a id="ref-rfc8259-strings" href="#ref-rfc8259-strings" class="permalink">🔗</a>
`RFC 8259 Section 7` - Section 7. Strings, "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 8259, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc8259#section-7](https://www.rfc-editor.org/rfc/rfc8259#section-7)

<a id="ref-rfc9110-https" href="#ref-rfc9110-https" class="permalink">🔗</a>
`RFC 9110 Section 4.2.2` - Section 4.2.2. https URI Scheme, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-4.2.2](https://www.rfc-editor.org/rfc/rfc9110#section-4.2.2)

<a id="ref-rfc9110-methods" href="#ref-rfc9110-methods" class="permalink">🔗</a>
`RFC 9110 Section 9` - Section 9. Methods, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-9](https://www.rfc-editor.org/rfc/rfc9110#section-9)

<a id="ref-rfc9110-codes" href="#ref-rfc9110-codes" class="permalink">🔗</a>
`RFC 9110 Section 15` - Section 15. Status Codes, "HTTP Semantics", RFC 9110, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9110#section-15](https://www.rfc-editor.org/rfc/rfc9110#section-15)

<a id="ref-rfc9396" href="#ref-rfc9396" class="permalink">🔗</a>
`RFC 9396` - "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396](https://www.rfc-editor.org/rfc/rfc9396)

<a id="ref-rfc9396-error-response" href="#ref-rfc9396-error-response" class="permalink">🔗</a>
`RFC 9396 Section 5` - Section 5. Authorization Error Response, "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396#section-5](https://www.rfc-editor.org/rfc/rfc9396#section-5)

<a id="ref-rfc9396-token-response" href="#ref-rfc9396-token-response" class="permalink">🔗</a>
`RFC 9396 Section 7` - Section 7. Token Response, "OAuth 2.0 Rich Authorization Requests", RFC 9396, Internet Engineering Task Force (IETF),  
[https://www.rfc-editor.org/rfc/rfc9396#section-7](https://www.rfc-editor.org/rfc/rfc9396#section-7)


## 23. Acknowledgments <a id="acknowledgments" href="#acknowledgments" class="permalink">🔗</a>

The authors would like to thank the late Shuli Goodman, who was the Executive Director of LFEnergy, for her incredible leadership in initially organizing the CDS.

## 24. Authors' Addresses <a id="authors-addresses" href="#authors-addresses" class="permalink">🔗</a>

Daniel Roesler  
Daniel Roesler LLC  
Email: [daniel@danielroesler.com](mailto:daniel@danielroesler.com)  
URI: [https://danielroesler.com/](https://danielroesler.com/)

