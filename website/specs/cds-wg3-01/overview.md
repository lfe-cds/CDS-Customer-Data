---
layout: base
nav: specs
title: CDS-WG3 - 01 Customer Data - Overview
meta_description: Linux Foundation Energy - Connected Data Specification (CDS) - Customer Dat aWorking Group (WG3) - Specifications - cds-wg3-01 - Customer Data - Overview
---
[Home]({{ "/" | relative_url }}) / [Specifications]({{ "/specs" | relative_url }}) / [`cds-wg3-01` - Customer Data]({{ "/specs/cds-wg3-01" | relative_url }}) / Overview

# Customer Data - Overview

This is a summary overview of the [`cds-wg3-01`]({{ "/specs/cds-wg3-01" | relative_url }}) ("Customer Data") specification, which describes how utilities and other central grid entities ("Servers") can provide secure, authorized access to customer account, usage, and bill data to external entities ("Clients").
External entities could be anyone, including customers themselves, their vendors or energy managers, approved academic institutions, government regulators, and more.
The goal of this specification is to define a standardized way for utilities and other central grid entities to offer secure, authorized, streamlined, and automated data access that meets many of their customer data exchange requirements.

This specification extends the CDS Registration Working Group (CDS-WG1) [specifications](https://cds-registration.lfenergy.org/specs/) by defining new "scopes" (i.e. permission levels) that Servers can offer for granular customer data access.
Servers can customize what level of access they offer, what customer datasets are available, and which external entities are approved to access the data.
Servers can also configure "self-service" authorizations (i.e. OAuth) for customers, which allows customers to manage their own data access for approved external entities ("Clients").
This allows utilities and other central entities to deploy Servers that are "right-sized" for their use cases, big or small.

This specification also defines Application Programming Interfaces (APIs) that organize customer data into documented API endpoints and machine-readable data formats.
These APIs can be deployed in tandem other data access protocols for seamless, parallel operation and smooth transitions from prior protocols to the CDS APIs.
By standardizing an open, freely available set of customer data APIs and data schemas, Servers and Clients can automate their connections and improve interoperability at a global scale.

Overall, this specification's combination of defined scopes of access and documented APIs and data formats gives utilities and other central entities a free, secure, scalable, and standardized protocol to meet their customer data access needs.

## Background <a id="background" href="#background" class="permalink">🔗</a>

Utilities and other central grid entities are increasingly needing to digitally exchange customer data with their own customers, vendors, or external entities who are approved to receive it.
Unfortunately, solutions to this problem are held back by manual processes, proprietary protocols, non-free standards, and high deployment costs.

The CDS Customer Data working group was formed to write new, freely available specifications that can be implemented at low cost to meet the needs of a diverse set of utility customer data access use cases.

## Use Case Examples <a id="use-case-examples" href="#use-case-examples" class="permalink">🔗</a>

Below are some examples of use cases that this specification can meet when implemented.
Many more use cases are also possible!
If you have a use case you think should be listed here, we encourage you to [make an issue](https://github.com/lfe-cds/CDS-Customer-Data/issues) or [email the mailing list](https://lists.lfenergy.org/g/cds-customer-data-wg).

* Providing enterprise customers access to their account details, usage intervals, or billing data in a standardized format.
* Providing utility vendors secure, managed access to customer datasets that are limited to only data the vendors need.
* Providing a utility-managed way for enterprise customers share their usage data with their own energy managers and vendors.
* Providing a secure and standardized way of sharing bulk, aggregated customer data to academic researchers or government regulators.
* Providing a self-service consent process for customers to authorize approved third party entities access to their data.
* Providing whole-building usage data access to building owners in territories with energy benchmarking regulations.
* Providing a secure, scalable process customers to share their rate plan or program eligibility with approved energy apps.

## Technical Summary <a id="technical-summary" href="#technical-summary" class="permalink">🔗</a>

The [`cds-wg3-01`]({{ "/specs/cds-wg3-01" | relative_url }}) specification is fundamentally an extension of the CDS Client Registration specification ([`cds-wg1-02`](https://cds-registration.lfenergy.org/specs/cds-wg1-02)), which is a framework for how utilities and other central entities ("Servers") can offer standardized connectivity to external parties ("Clients").
By extending `cds-wg1-02`, the built-in Client registration, communication, and management features from that specification are also part of this customer data specification, which significantly increases scalability and flexibility for Server operators.
For example, a utility or other central entity could deploy a small, low cost Server for niche use cases and ad-hoc customer data access needs, then expand the Server's enabled use cases, scopes, and datasets over time.

As part of the extension, this specification defines new [OAuth scope values]({{ "/specs/cds-wg3-01" | relative_url }}#authorization-scopes) that standardize various levels of access for common customer data access use cases.
These scopes cover use cases where customer consent is required (using the widely adopted OAuth `authorization_code` grant flow) and use cases where only Server authorization is required (using the OAuth `client_credentials` grant flow).
When customer consent is required, the specification defines authorization user interface requirements that streamline the customer authorization experience while also keeping the authorization process secure.

Another part of the extension is a set of APIs and data formats for customer data (listed below).

* Customer account and meter details:
    * [Accounts API]({{ "/specs/cds-wg3-01" | relative_url }}#accounts-api) - Standardizes details about customer billing accounts (e.g. the number at the top of your utility bill).
    * [Service Contracts API]({{ "/specs/cds-wg3-01" | relative_url }}#service-contracts-api) - Standardizes details about customer service contracts (e.g. your individual electric or gas service).
    * [Service Points API]({{ "/specs/cds-wg3-01" | relative_url }}#service-points-api) - Standardizes details about utility service point locations (e.g. the box on the side of your home).
    * [Meter Devices API]({{ "/specs/cds-wg3-01" | relative_url }}#meter-devices-api) - Standardizes details about utility meter devices (e.g. the physical meter reading your usage).
* Customer bill data:
    * [Bill Statements API]({{ "/specs/cds-wg3-01" | relative_url }}#bill-statements-api) - Standardizes details about a customer's bill statement (how much you owe, payment due dates, etc.).
    * [Bill Sections API]({{ "/specs/cds-wg3-01" | relative_url }}#bill-sections-api) - Standardizes usage and cost breakdowns for each of a customer's service contracts (your electric usage, line item costs, etc.).
* Aggregated datasets:
    * [Aggregations API]({{ "/specs/cds-wg3-01" | relative_url }}#aggregations-api) - Standardizes how to document aggregated usage groups (whole-building usage, zip-code level usage, etc.).
* Customer usage data:
    * [Usage Segments API]({{ "/specs/cds-wg3-01" | relative_url }}#usage-segments-api) - Standardizes how to provide usage data, both individual (e.g. kWh intervals) and aggregated (e.g. monthly whole-building usage).
* Energy Attribute Certificates:
    * [Energy Attribute Certificates API]({{ "/specs/cds-wg3-01" | relative_url }}#eac-api) - Standardizes how to provide EACs from utilities and other central entities to Clients.

**NOTE:** Servers do NOT have to provide access to all of this information.
They can limit access by customer and Client to only the specific subset of data within these APIs that is needed for that specific use case.
What data fields are included in the responses from these APIs is determined by what scopes the Client's `access_token` has been granted.

Overall, this specification provides a highly flexible set of authorization scopes, APIs, and data formats to support many customer data access use cases.
This allows utilities and other central entities to quickly deploy Servers that are tailor to their individual use cases, and allows Clients to automate integration with new Servers for scalable interoperability and adoption.

## Examples <a id="examples" href="#examples" class="permalink">🔗</a>

Here are some examples of how to programmatically interact with a Server's APIs using `curl` and `jq` from the command line.

<span style="background-color:yellow">TODO</span>

---

# Other Drafts <a id="other-drafts" href="#other-drafts" class="permalink">🔗</a>

* Maintainer's draft: [[Website](https://daniel-roesler.github.io/CDS-Customer-Data/specs/cds-wg3-01/overview)] [[Code](https://github.com/daniel-roesler/CDS-Customer-Data/blob/main/website/specs/cds-wg3-01/overview.md)]

