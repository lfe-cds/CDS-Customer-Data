# CDS-WG3-01 - Customer Data

## Abstract <a id="abstract" href="#abstract" class="permalink">🔗</a>

This specification defines how utilities and other central entities ("Servers") may allow vendors, customers, and other external organizations ("Clients") to securely access account holder data ("Customer Data"), including in situations where Customer consent where needed or data is aggregated.
This specification extends [[CDS-WG1-02](#ref-cds-wg1-02)] ("Client Registration") to add authorization [Scopes](#scopes) for Customer Data access, specifies common [Scenarios](#scenarios), and defines Application Programming Interfaces (APIs) and data formats for Customer Data.

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
    * [3.3. Meter Data](#scenario-meter-data)  
    * [3.4. Rate Plan](#scenario-rate-plan)  
    * [3.5. Program Participation](#scenario-program-participation)  
    * [3.6. Program Analysis](#scenario-program-analysis)  
    * [3.7. Bill Amount Due](#scenario-bill-amount-due)  
    * [3.8. Bill Statements](#scenario-bill-statements)  
    * [3.9. Service Charges](#scenario-service-charges)  
    * [3.10. Whole-Building Data](#scenario-building-data)  
    * [3.11. Aggregated Data](#scenario-aggregated-data)  
    * [3.12. EAC Data](#scenario-eac-data)  
    * [3.13. Internal Access](#scenario-internal-access)  
* [4. CDS-WG1-02 Extension](#cds-wg1-02-extension)  
* [5. Server Metadata](#server-metadata)  
* [6. Scopes Supported](#scopes)  
    * [6.1. Rate Plan](#scope-rate-plan)  
    * [6.2. Program Participation](#scope-program-participation)  
    * [6.3. Account List](#scope-account-list)  
    * [6.4. Service List](#scope-service-list)  
    * [6.5. Meter List](#scope-meter-list)  
    * [6.6. Bill Statements](#scope-bill-statements)  
    * [6.7. Service Bills](#scope-service-bills)  
    * [6.8. Meter Usage](#scope-meter-usage)  
    * [6.9. Aggregation Inclusion](#scope-aggregation-inclusion)  
    * [6.10. Accounts Query](#scope-accounts-query)  
    * [6.11. Service Contracts Query](#scope-service-contracts-query)  
    * [6.12. Service Points Query](#scope-service-points-query)  
    * [6.13. Meters Devices Query](#scope-meter-devices-query)  
    * [6.14. Bill Statements Query](#scope-bill-statements-query)  
    * [6.15. Bill Sections Query](#scope-bill-sections-query)  
    * [6.16. Aggregations Query](#scope-aggregations-query)  
    * [6.17. Usage Query](#scope-usage-query)  
* [7. Authorization Details Fields](#auth-details-fields)  
* [8. Client Registration Requirements](#client-registration-requirements)  
* [9. Customer Authorizations](#customer-authorizations)  
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
These entities can include, but are not limited to, carbon tracking applications, electric vehicle companies, clean energy technology providers, commercial utility customers, grid management applications, and energy efficiency organizations.

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
In contrast, another example is for a building management company to need access to their property owner client's (i.e. the utility Customer's) bill statements.

This section defines typical Scenarios as sets of [Scopes](#scopes) and configuration requirements for Scope Descriptions that are intended to meet the Scenario's use cases.
Because Scenarios are defined sets of Scopes and configuration requirements, they don't actually appear in any API responses by the Server.
Rather, Scenarios are intended to aide regulators, governing authorities, and [Extensions](#extensions) in choosing which Scopes, data fields, and functionality they require Servers to implement.

Regulators, governing authorities, and extensions MAY choose to require implementation of any or all Scenarios defined in this specification or choose to define new Scenarios.
When defining new Scenarios, it is RECOMMENDED that those Scenarios are defined either as a modification to one or more Scenarios from this specification or by defining the new Scenario in the same format as the Scenarios defined in this specification.

New Scenarios MAY include requirements to extend various object formats or numerated values to include additional fields.
For example, a regulator could define a new Scenario that extends the "Meter Data" Scenario by requiring an additional field be included in the [Service Contract](#service-contract-format) object, such as `dso_participant_id`.

In addition to implementing requirements for their jurisdiction, Servers MAY implement additional Scopes or configurations that helps provide Customer Data access for relevant internal or external use cases.
For example, a utility could provide program participants' meter data to the utility's contracted vendor that is running the program.

Clients MUST NOT rely on stated Scenario compliance from regulators or Servers.
Instead, Clients MUST parse the [Server Metadata](#server-metadata) to evaluate if a Server's offered Scopes and functionality is adequate for their use case.

### 3.1. Usage Self-Access <a id="scenario-usage-self-access" href="#scenario-usage-self-access" class="permalink">🔗</a>

This section defines the Scenario "Usage Self-Access" in which a utility Customer wants to access their own accounts' meter and usage data.
For example, a commercial customer would like to download their historical meter usage feed for their owned properties, so that their energy management team can evaluate their buildings' energy profiles.
Another example is for a data center owner to want to integrate an automatic nightly update of their previous day's meter usage, so that they can check their operations for discrepancies between their own energy monitoring systems.

To implement the "Customer Self-Access" Scenario, Servers MUST implement the following list of requirements.

* Servers MUST include at least one of each [Accounts Query](#scope-accounts-query), [Service Contracts Query](#scope-service-contracts-query), [Service Points Query](#scope-service-points-query), [Meter Devices Query](#scope-meter-devices-query), and [Usage Query](#scope-usage-query) Scope Descriptions in the Server Metadata `cds_scope_descriptions` object, as well as include those Scope Description `id` values in the Server Metadata `scopes_supported` list.
* Servers MUST include any required steps for verifying the identity of a Client as the Customer in the Scope Description `registration_requirements` list.
  For example, if a Server requires Customers to associate their online web account with Clients they register, the Server would include a Registration Field [[CDS-WG1-02 Section 3.5](#ref-cds-wg1-02-registration-fields)] with a `type` value of `sso_verification`.
* Once Servers have verified the identity of the Client as the Customer, Servers MUST configure the Customer's Accounts and Service Contracts to be accessible to the Client using the [Accounts Query](#scope-accounts-query) and [Service Contracts Query](#scope-service-contracts-query) scopes.
* Servers MUST configure any Meter Devices, Service Points, and Usage Segments associated with the Customer's Accounts and Service Contracts to be accessible to the Client using the [Service Points Query](#scope-service-points-query), [Meter Devices Query](#scope-meter-devices-query), and [Usage Query](#scope-usage-query) scopes.
* Servers MUST keep the list of Accounts, Service Contracts, Service Points, Meter Devices, and Usage Segments that are associated with the Client updated to reflect any changes to the Customer.
  For example, if a Customer builds a new warehouse on their property and installs a new electric service for the warehouse, the Client should see any new Accounts, Service Contracts, Service Points, Meter Devices, and Usage Segments for the new warehouse included in API objects returned when data is next synced from the Server.
* The [Usage Query](#scope-usage-query) Scope Description's `segment_start` authorization details field MUST allow the Client to access for each connected Service Contract's usage at least 1 year of historical usage data (i.e. `minimum` value set to at least `P1Y`).

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
* The [Bill Sections Query](#scope-bill-sections-query) Scope Description's `start_date` authorization details field MUST allow the Client to access for each connected Service Contract at least 1 year of historical bill sections (i.e. `minimum` value set to at least `P1Y`).

### 3.3. Meter Data <a id="scenario-meter-data" href="#scenario-meter-data" class="permalink">🔗</a>

This section defines the Scenario "Meter Data" in which a Client requests access to a Customer's historical and/or ongoing usage data.
For example, an energy efficiency auditor is working with building owner, and needs to download their previous year's usage.

<span style="background-color:yellow">TODO</span>

### 3.4. Rate Plan <a id="scenario-rate-plan" href="#scenario-rate-plan" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.5. Program Participation <a id="scenario-program-participation" href="#scenario-program-participation" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.6. Program Analysis <a id="scenario-program-analysis" href="#scenario-program-analysis" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.7. Bill Amount Due <a id="scenario-bill-amount-due" href="#scenario-bill-amount-due" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.8. Bill Statements <a id="scenario-bill-statements" href="#scenario-bill-statements" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.9. Service Charges <a id="scenario-service-charges" href="#scenario-service-charges" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.10. Whole-Building Data <a id="scenario-building-data" href="#scenario-building-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.11. Aggregated Data <a id="scenario-aggregated-data" href="#scenario-aggregated-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.12. EAC Data <a id="scenario-eac-data" href="#scenario-eac-data" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 3.13. Internal Access <a id="scenario-internal-access" href="#scenario-internal-access" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 4. CDS-WG1-02 Extension <a id="cds-wg1-02-extension" href="#CDS-WG1-02-extension" class="permalink">🔗</a>

As the framework for providing Client registration, onboarding, communication, management, and grant authorizations, Servers MUST implement CDS's Client Registration specification [[CDS-WG1-02](#ref-cds-wg1-02)].
This specification in subsequent sections extends the CDS's Client Registration framework by defining additional fields, values, and APIs that are relevant to Customer Data access, so that Servers implementing this specification can satisfy Customer Data use cases.

## 5. Server Metadata <a id="server-metadata" href="#server-metadata" class="permalink">🔗</a>

This specification extends CDS's Authorization Server Metadata object [[CDS-WG1-02 Section 3.2](#ref-cds-wg1-02-metadata)] to include the following named values:

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

<span style="background-color:yellow">TODO</span>

### 6.1. Rate Plan <a id="scope-rate-plan" href="#scope-rate-plan" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.2. Program Participation <a id="scope-program-participation" href="#scope-program-participation" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.3. Account List <a id="scope-account-list" href="#scope-account-list" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.4. Service List <a id="scope-service-list" href="#scope-service-list" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.5. Meter List <a id="scope-meter-list" href="#scope-meter-list" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.6. Bill Statements <a id="scope-bill-statements" href="#scope-bill-statements" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.7. Service Bills <a id="scope-service-bills" href="#scope-service-bills" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.8. Meter Usage <a id="scope-meter-usage" href="#scope-meter-usage" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.9. Aggregation Inclusion <a id="scope-aggregation-inclusion" href="#scope-aggregation-inclusion" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.10. Accounts Query <a id="scope-accounts-query" href="#scope-accounts-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.11. Service Contracts Query <a id="scope-service-contracts-query" href="#scope-service-contracts-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.12. Service Points Query <a id="scope-service-points-query" href="#scope-service-points-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.13. Meter Devices Query <a id="scope-meter-devices-query" href="#scope-meter-devices-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.14. Bill Statements Query <a id="scope-bill-statements-query" href="#scope-bill-statements-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.15. Bill Sections Query <a id="scope-bill-sections-query" href="#scope-bill-sections-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.16. Aggregations Query <a id="scope-aggregations-query" href="#scope-aggregations-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

### 6.17. Usage Query <a id="scope-usage-query" href="#scope-usage-query" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 7. Authorization Details Fields <a id="auth-details-fields" href="#auth-details-fields" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 8. Client Registration Requirements <a id="client-registration-requirements" href="#client-registration-requirements" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 9. Customer Authorizations <a id="customer-authorizations" href="#customer-authorizations" class="permalink">🔗</a>

<span style="background-color:yellow">TODO</span>

## 10. Accounts API <a id="accounts-api" href="#accounts-api" class="permalink">🔗</a>

This specification requires Servers provide a set of Application Programming Interfaces (APIs) allowing Clients to retrieve a Customer's Account details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Accounts API.

### 10.1. Account Object Format <a id="account-format" href="#account-format" class="permalink">🔗</a>

Account objects are formatted as JSON objects and contain the following named values:

* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Account object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Account object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Account object.
* `cds_account_parent` - _[string](#string) or `null`_ - (REQUIRED) The `cds_account_id` of a parent Account object of which this Account is grouped under.
  If the Account is not a part of a parent grouping, this value is `null`.
* `customer_number` - _[string](#string) or `null`_ - (REQUIRED) The identifier that a Customer has access to that identifies a collection of Accounts as being for the same Customer.
  If a Server only stores Accounts as the top-level identifier for Customers, or does not have a Customer-accessible identifier set for this Account, this value is `null`.
* `account_number` - _[string](#string)_ - (REQUIRED) The number that a Customer sees on their bill and online user interface as the identifier for this Account.
* `account_name` - _[string](#string)_ - (OPTIONAL) The name that a Customer sees on their bill and online user interface as the name for this Account, if available.
* `account_address` - _[string](#string)_ - (OPTIONAL) The address that a Customer sees on their bill and online user interface as the address for this Account, if available.
  This string MAY have linebreaks, such as when the address is multiple lines.
* `account_type` - _[AccountType](#account-types)_ - (REQUIRED) What type of Account this is.
* `account_contacts` - _Array[[AccountContact](#account-contact-format)]_ - (REQUIRED) A list of Account Contacts for the Account.

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
* `q` - A search term for which the Servers MUST filter the Accounts following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_account_id`
    * `customer_number`
    * `account_number`
    * `account_address`
    * `account_name`

Listings of Account objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Account MUST be first in each listing.

## 11. Service Contracts API <a id="service-contracts-api" href="#service-contracts-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's Service Contract details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Service Contracts API.

### 11.1. Service Contract Object Format <a id="service-contract-format" href="#service-contract-format" class="permalink">🔗</a>

Service Contract objects are formatted as JSON objects and contain the following named values:

* `cds_servicecontract_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Service Contract object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Service Contract object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Service Contract object.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account to which this Service Contract belongs.
* `account_number` - _[string](#string)_ - (REQUIRED) The Account's `account_number` duplicated to this Service Contract.
* `contract_number` - _[string](#string) or `null`_ - (REQUIRED) The identifier that a Customer sees on their bill or online user interface as the identifier for this Service Contract.
  If a Server does not have a Customer-facing identifier for a Service Contract and the Client is not authorized to see the Server's internal identifier for this Service Contract, this value is `null`.
* `contract_address` - _[string](#string)_ - (OPTIONAL) The address that a Customer sees on their bill or online user interface as the address for this Service Contract, if available.
  This string MAY have linebreaks, such as when the address is multiple lines.
* `contract_status` - _[ContractStatus](#contract-statuses)_ - (REQUIRED) The current status of the Service Contract.
* `contract_type` - _[ContractType](#contract-types)_ - (REQUIRED) The type of agreement that this Service Contract represents.
* `contract_entity` - _[string](#string)_ - (REQUIRED) With which entity the Customer has agreement for this Service Contract.
* `contract_start` - _[date](#date)_ - (OPTIONAL) When the agreement that this Service Contract represents started.
* `contract_end` - _[date](#date) or `null`_ - (OPTIONAL) When the agreement that this Service Contract represents ended.
  If the agreement is still ongoing, this value is `null`.
* `service_type` - _[ServiceType](#service-types)_ - (REQUIRED) The type of utility or other service that this Service Contract provides.
* `service_class` - _[ServiceClass](#service-classes)_ - (REQUIRED) The class of service for which this Service Contract is categorized.
* `rateplan_code` - _[string](#string)_ - (REQUIRED) A unique code for the current rate plan or tariff on which costs for this Service Contract are calculated.
* `rateplan_name` - _[string](#string)_ - (REQUIRED) The name that a Customer sees on their bill or online user interface as the rate plan or tariff that applies to this Service Contract.

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
* `service_types` - A space-separated list of `service_type` values for which the Servers MUST filter the Service Contracts.
* `q` - A search term for which the Servers MUST filter the Service Contracts following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_servicecontract_id`
    * `account_number`
    * `contract_number`
    * `contract_address`
    * `rateplan_code`
    * `rateplan_name`

Listings of Service Contract objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Service Contract MUST be first in each listing.

## 12. Service Points API <a id="service-points-api" href="#service-points-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Service Point details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Service Points API.

### 12.1. Service Point Object Format <a id="service-point-format" href="#service-point-format" class="permalink">🔗</a>

Service Point objects are formatted as JSON objects and contain the following named values:

* `cds_servicepoint_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Service Point object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Service Point object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Service Point object.
* `servicepoint_number` - _[string](#string) or `null`_ - (REQUIRED) The identifier that a Customer sees on their bill or online user interface as the identifier for this Service Point.
  If a Server does not have a Customer-facing identifier for a Service Point and the Client is not authorized to see the Server's internal identifier for this Service Point, this value is `null`.
* `servicepoint_type` - _[ServicePointType](#service-point-types)_ - (REQUIRED) The type of service point that this Service Point represents.
* `servicepoint_address` - _[string](#string) or `null`_ - (REQUIRED) The address that a Customer sees on their bill or online user interface as the address for this Service Point, if available.
  Strings MAY have linebreaks, such as when the address is multiple lines.
  If a Server does not have a Customer-facing address for a Service Point and the Client is not authorized to see the Server's internal address for this Service Point, this value is `null`.
* `latitude` - _[decimal](#decimal)_ - (REQUIRED) The latitude that a Customer sees on their bill or online user interface as the latitude for this Service Point.
  If a Server does not have a Customer-facing latitude for a Service Point and the Client is not authorized to see the Server's internal latitude for this Service Point, or the Server does not have latitude stored for this Server Point, this value is `null`.
* `longitude` - _[decimal](#decimal)_ - (REQUIRED) The longitude that a Customer sees on their bill or online user interface as the longitude for this Service Point.
  If a Server does not have a Customer-facing longitude for a Service Point and the Client is not authorized to see the Server's internal longitude for this Service Point, or the Server does not have longitude stored for this Server Point, this value is `null`.
* `current_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts that are currently providing a service to a Customer via this Service Point.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `previous_servicecontracts` - _Array[[string](#string)]_ - (REQUIRED) The list of `cds_servicecontract_id` values that identify Service Contracts that have previously provided a service to a Customer via this Service Point.
  This list MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.
* `premise_number` - _[string](#string) or `null`_ - (REQUIRED) The premise identifier that a Customer sees on their bill or online user interface as the premise identifier for this Service Point, if available.
  If a Server does not have a Customer-facing premise identifier for a Service Point and the Client is not authorized to see the Server's internal premise identifier for this Service Point, or the Server does not have premise identifiers stored for this Server Point, this value is `null`.
* `premise_type` - _[PremiseType](#premise-types) or `null`_ - (REQUIRED) The premise type that a Customer sees on their bill or online user interface as the premise type for this Service Point, if available.
  If a Server does not have a Customer-facing premise type for a Service Point and the Client is not authorized to see the Server's internal premise type for this Service Point, or the Server does not have premise type stored for this Server Point, this value is `null`.

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
* `servicepoint_number` - A space-separated list of `servicepoint_number` values for which the Servers MUST filter the Service Points.
* `current_servicecontracts` - A space-separated list of `current_servicecontracts` values for which the Servers MUST filter the Meter Devices.
* `previous_servicecontracts` - A space-separated list of `previous_servicecontracts` values for which the Servers MUST filter the Meter Devices.
* `q` - A search term for which the Servers MUST filter the Service Points following fields for values that case-insensitive contains the search term, if the field is accessible based on the Client's `access_token` scope.
    * `cds_servicepoint_id`
    * `servicepoint_address`
    * `servicepoint_number`
    * `premise_number`
    * `property_number`
    * `parcel_number`

Listings of Service Point objects MUST be ordered in reverse chronological order by `cds_modified` timestamp, where the most recently updated relevant Service Point MUST be first in each listing.

## 13. Meter Devices API <a id="meter-devices-api" href="#meter-devices-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Meter Device details for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Meter Devices API.

### 13.1. Meter Device Object Format <a id="meter-device-format" href="#meter-device-format" class="permalink">🔗</a>

Meter Device objects are formatted as JSON objects and contain the following named values:

* `cds_meterdevice_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Meter Device object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Meter Device object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Meter Device object.
* `meter_number` - _[string](#string) or `null`_ - (REQUIRED) The identifier that a Customer sees on their bill or online user interface or on the face of their physical meter as the identifier for this Meter Device.
  If a Server does not have a Customer-facing identifier for a Meter Device and the Client is not authorized to see the Server's internal identifier for this Meter Device, this value is `null`.
* `meter_type` - _[MeterType](#meter-types)_ - (REQUIRED) The type of metering device that this Meter Device represents.
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
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Bill Statements API.

### 14.1. Bill Statement Object Format <a id="bill-statement-format" href="#bill-statement-format" class="permalink">🔗</a>

Bill Statement objects are formatted as JSON objects and contain the following named values:

* `cds_billstatement_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Bill Statement object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Bill Statement object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Bill Statement object.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account for which this Bill Statement was issued.
* `account_number` - _[string](#string)_ - (REQUIRED) The Account's `account_number` at the time that this Bill Statement was issued.
* `statement_date` - _[date](#date)_ - (REQUIRED) The statement date that appears on the Customer-facing bill.
* `currency` - _[string](#string)_ - (REQUIRED) The currency for this Bill Statement's charge values as an [[ISO 4217](#ref-iso4217)] currency code.
* `file_uri` - _[URL](#url)_ - (OPTIONAL) A link to the bill statement file that was provided to the Customer, if available.
  If this Bill Statement was mailed to the Customer, this value is a link to the digital PDF copy of the mailed bill.
  Authentication for the linked file MUST be the same authentication as required for accessing this Bill Statement.
* `file_mimetype` - _[string](#string)_ - (OPTIONAL) The MIME type for the `file_uri`.
  This field is required if the `file_uri` field is populated.
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
* `program_participations` - _Array[[ProgramParticipation](#program-participation-format)]_ - (REQUIRED) A list of Account-level programs in which the Customer is participating.
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
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Bill Sections API.

### 15.1. Bill Section Object Format <a id="bill-section-format" href="#bill-section-format" class="permalink">🔗</a>

Bill Section objects are formatted as JSON objects and contain the following named values:

* `cds_billsection_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Bill Section object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Bill Section object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Bill Section object.
* `cds_billstatement_id` - _[string](#string) or `null`_ - (REQUIRED) The Bill Statement for which this Bill Section was issued.
  If the Client is not authorized to see the related Bill Statement, this value is `null`.
* `cds_account_id` - _[string](#string)_ - (REQUIRED) The Account for which this Bill Section was issued.
* `account_number` - _[string](#string)_ - (REQUIRED) The Account's `account_number` at the time that this Bill Section was issued.
* `section_type` - _[BillSectionType](#bill-section-types)_ - (REQUIRED) The type of bill section that this Bill Section represents.
* `start_date` - _[date](#date)_ - (REQUIRED) The start date of services provided for which this Bill Section covers.
* `end_date` - _[date](#date)_ - (REQUIRED) The end date of services provided for which this Bill Section covers.
* `currency` - _[string](#string)_ - (REQUIRED) The currency for this Bill Section's charge values as an [[ISO 4217](#ref-iso4217)] currency code.
* `distribution_entity` - _[DistributionEntity](#distribution-entity-format) or `null`_ - (REQUIRED) The details for the utility or other entity providing distribution for the services that this Bill Section covers.
  If the services provided that are covered by this Bill Section do not have distribution, this value is `null`.
* `load_serving_entity` - _[LoadServingEntity](#load-serving-entity-format) or `null`_ - (REQUIRED) The details for the utility or other entity providing distribution for the services that this Bill Section covers.
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
* `line_items` - _Array[[BillSectionLineItem](#bill-section-line-item-format)]_ - (REQUIRED) A list of the Bill Section Line Item objects that denote the individual charges listed on the Customer-facing bill statement section represented by this Bill Section.
* `energy_summary` - _Array[[BillSectionEnergySummary](#bill-section-line-item-format)]_ - (REQUIRED) A list of the Bill Section Energy Summary objects that detail the service's energy usage and other values on the Customer-facing bill statement section represented by this Bill Section.

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
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Aggregations API.

### 16.1. Aggregation Object Format <a id="aggregation-format" href="#aggregation-format" class="permalink">🔗</a>

Aggregation objects are formatted as JSON objects and contain the following named values:

* `cds_aggregation_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Aggregation object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Aggregation object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Aggregation object.
* `aggregation_type` - _[AggregationType](#aggregation-types)_ - (REQUIRED) The type of aggregation that this Aggregation represents.
* `aggregation_number` - _[string](#string) or `null`_ - (REQUIRED) The aggregation identifier that a Client sees in Server documentation or other online interfaces as the aggregation identifier for this Aggregation, if available.
  If a Server does not have a Client-facing aggregation identifier for an Aggregation and the Client is not authorized to see the Server's internal aggregation identifier for this Aggregation, or the Server does not have aggregation identifiers stored for this Aggregation, this value is `null`.
* `addresses` - _Array[[string](#string)]_ - (OPTIONAL) A list of addresses that are associated with this Aggregation, if applicable for the `aggregation_type`.
  Strings MAY have linebreaks, such as when the address is multiple lines.
* `regions` - _Array[[AggregationRegion](#aggregation-region-format)]_ - (OPTIONAL) A list of Aggregation Regions that are associated with this Aggregation, if applicable for the `aggregation_type`.
* `consents_required` - _Array[[ConsentRequirement](#consent-requirement-format)]_ - (OPTIONAL) A list of Consent Requirements that are required for this Aggregation, if applicable for the `aggregation_type`.

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
    * `regions`

Listings of Aggregation objects MUST be ordered in reverse chronological order by `modified` timestamp, where the most recently updated relevant Aggregation MUST be first in each listing.

## 17. Usage Segments API <a id="usage-segments-api" href="#usage-segments-api" class="permalink">🔗</a>

This specification requires Servers provide a set of APIs allowing Clients to retrieve a Customer's and Server's Usage Segment objects for which they are authorized to access.
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Usage Segments API.

### 17.1. Usage Segment Object Format <a id="usage-segment-format" href="#usage-segment-format" class="permalink">🔗</a>

Usage Segment objects are formatted as JSON objects and contain the following named values:

* `cds_usagesegment_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this Usage Segment object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this Usage Segment object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this Usage Segment object.
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
* `format` - _Array[[ValueFormat](#usage-segment-value-format)]_ - (REQUIRED) For each entry in `values`, these are the formats of that are included, presented in the order in which the values are presented.
* `values` - _Array[[ValueSet](#usage-segment-value-set-format)]_ - (REQUIRED) A list of Value Sets that represent the entries included in the Usage Segment.

Servers MUST only inlude unique identifiers in `related_aggregations`, `related_accounts`, `related_servicecontracts`, `related_servicepoints`, `related_meterdevices`, and `related_billsections` lists MUST only include identifiers that the Client is authorized to see as scoped by their requesting `access_token`.

### 17.2. Usage Segment Value Formats <a id="usage-segment-value-format" href="#usage-segment-value-format" class="permalink">🔗</a>

The Usage Segment `format` field provides an ordered list of strings that denote the object types that are included in each Usage Segment `values` entry's [Value Set](#usage-segment-value-set-format).
Usage Segments are organized this way so that the `format` listing essentially provides a "schema" of object types to expect when parsing the `values` Value Sets, which removes the need for each object in each Value Set to include repeated fields, such as an object type value.

The following Usage Segment format strings are specified with their corresponding Value Set object types:

* `usage_kwh` - [General Electric Usage in kWh object](#usage-segment-value-usage-kwh)
* `usage_fwd_kwh` - [Forward Electric Usage in kWh object](#usage-segment-value-usage-fwd-kwh)
* `usage_rev_kwh` - [Reverse Electric Usage in kWh object](#usage-segment-value-usage-rev-kwh)
* `usage_net_kwh` - [Net Electric Usage in kWh object](#usage-segment-value-usage-net-kwh)
* `aggregated_kwh` - [Aggregated Electric Usage in kWh object](#usage-segment-value-aggregated-kwh)
* `demand_kw` - [Electric Demand in kW object](#usage-segment-value-demand-kw)
* `water_m3` - [Water Usage in Cubic Meters object](#usage-segment-value-water-m3)
* `water_gal` - [Water Usage in Gallons object](#usage-segment-value-water-gal)
* `water_ft3` - [Water Usage in Cubic Feet object](#usage-segment-value-water-ft3)
* `gas_therm` - [Natural Gas Usage in Therms object](#usage-segment-value-gas-therm)
* `gas_ccf` - [Natural Gas Usage in CCF object](#usage-segment-value-gas-ccf)
* `gas_mcf` - [Natural Gas Usage in MCF object](#usage-segment-value-gas-mcf)
* `gas_mmbtu` - [Natural Gas Usage in mmBTU object](#usage-segment-value-gas-mmbtu)
* `supply_mix` - [Electric Supply Mix object](#usage-segment-value-supply-mix)
* `eacs` - [Energy Attribute Certificates object](#usage-segment-value-eacs)

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
These APIs are authenticated using a Bearer `access_token` [granted](https://cds-registration.lfenergy.org/specs/cds-wg1-02#grants-api) that provisions access for a [scope](#authorization-scopes) that allows access to the Energy Attribute Certificates API.

### 18.1. Energy Attribute Certificate Object Format <a id="eac-format" href="#eac-format" class="permalink">🔗</a>

EAC objects are formatted as JSON objects and contain the following named values:

* `cds_eac_id` - _[string](#string)_ - (REQUIRED) The Server's unique identifier for this EAC object.
* `cds_created` - _[datetime](#datetime)_ - (REQUIRED) When the Server created this EAC object.
* `cds_modified` - _[datetime](#datetime)_ - (REQUIRED) When the Server last modified this EAC object.
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
`CDS-WG1-02 Section 3.4` - "Scopes Descriptions Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scope-descriptions-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#scope-descriptions-format)

<a id="ref-cds-wg1-02-registration-fields" href="#ref-cds-wg1-02-registration-fields" class="permalink">🔗</a>
`CDS-WG1-02 Section 3.5` - "Registration Field Object Format", CDS-WG1-02, LF Energy Standards and Specifications (LFESS),  
[https://cds-registration.lfenergy.org/specs/cds-wg1-02/#registration-field-format](https://cds-registration.lfenergy.org/specs/cds-wg1-02/#registration-field-format)

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

## 23. Acknowledgments <a id="acknowledgments" href="#acknowledgments" class="permalink">🔗</a>

The authors would like to thank the late Shuli Goodman, who was the Executive Director of LFEnergy, for her incredible leadership in initially organizing the CDS.

## 24. Authors' Addresses <a id="authors-addresses" href="#authors-addresses" class="permalink">🔗</a>

Daniel Roesler  
Daniel Roesler LLC  
Email: [daniel@danielroesler.com](mailto:daniel@danielroesler.com)  
URI: [https://danielroesler.com/](https://danielroesler.com/)

