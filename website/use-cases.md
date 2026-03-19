---
layout: base
nav: use_cases
title: Use Cases
meta_description: What are the use cases that the CDS Customer Data specifications this working group is trying to address?
---
[Home]({{ "/" | relative_url }}) / Use Cases

# Use Cases

These are the use cases that this working group is writing specifications to address.


## Use Case 1: Energy Management <a id="use-case-energy-management" href="#use-case-energy-management" class="permalink">🔗</a>

Many companies, organizations, and governments around the world are working to measure and track their energy usage and costs.
This is called [energy management](https://en.wikipedia.org/wiki/Energy_management_system_(building_management)).

While energy management analyses vary widely, most of these procedures for calculating results involve obtaining customer data, such as electric and natural gas usage, meter locations, rate plans, retailer details, generation and supply information, etc.

#### Needs:

* **Standardized Data Formats and Access Procedures:**
This data often needs to be gathered both as historical data (e.g. getting the past year of usage data for a historical analysis), current data (e.g. a customer's current retailer and rate plan), and ongoing data (e.g. getting the past day's usage for real-time tracking of energy usage).
To address this need, the specifications from this working group will establish standardized data formats and access procedures so that data can be easily obtained, analyzed, and combined from multiple sources (e.g. companies with buildings in multiple utility territories).

* **Standardized Customer Datasets:**
In order to perform the needed calculations for energy management and other Use Cases, a comprehensive dataset with the appropriate data fields must be provided.
Which data fields, granularity, and time periods are needed are highly use-case dependent, but without requirements to follow, utilities may not provide access to adequate datasets to satisfy an intended use case's needs.
Thus, specifications to address this use case must consider what specific datasets are required for common use cases in addition to how the data should be formatted and accessed.

* **Granular Production and Certification Data:**
As a component energy management, energy purchase contracts and emissions are often monitored and reported in order to meet corporate policies or regulatory requirements.
As part of tracking energy contracts and emissions, granular certificates, attestations, supply mix and details, and production meter data are important for technical calculations needed to meet energy management policies.
Examples of these policies include verifying power purchase agreements, 24/7 energy matching, and creating market signals to stimulate investment in new technologies.
To address this use case, the specifications must consider how to obtain revenue grade metered generation data and other necessary certification inputs from individual asset owners and grid operators in a centralized and secure manner.

* **Privacy, Consent, and Security:**
Since energy management calculations are typically for specific entities, the actual utility data of that specific entity is required to perform calculations (e.g. the actual kwh usage, meter locations, and rate plans for the customer for the past 12 months).
This data is typcially considered private information, so specifications to address this use case must consider privacy, consent, and security requirements as part of the scope of work.

* **Streamlined User Experience:**
Many organizations do not have dedicated staff or sufficient resources to spend large amounts of time and effort gathering utility data needed for energy management.
This effectively creates a "barrier to entry" that limits energy management to only the most well-resourced organizations and prevents widespread completion of energy efficiency goals.
Thus, in order to address this use case adequately, the specifications for this working group must consider user experience and performance to allow for easy, streamlined customer utility data access.

#### Examples of users in this use case:

* Platforms that perform energy analyses for enterprises (
[WattCarbon](https://www.wattcarbon.com/),
[FlexiDAO](https://www.flexidao.com/),
[Microsoft Cloud for Sustainability](https://www.microsoft.com/en-us/sustainability),
etc.)

* Organizations looking to adopt the [24/7 Carbon-free Energy Compact](https://gocarbonfree247.com/)
and other [ESG](https://en.wikipedia.org/wiki/Environmental%2C_social_and_corporate_governance) pledges

* Consultants and advisors helping organizations that have climate and decarbonization targets


## Use Case 2: Energy Projects <a id="use-case-energy-projects" href="#use-case-energy-projects" class="permalink">🔗</a>

Often, organizations with energy management targets (see [Use Case 1: Energy Management](#use-case-energy-management) above), will evaluate projects that may optimize their energy usage or costs.
These projects can range from physical locally (e.g. installing solar or EV charging) and remote (e.g. building renewable generation projects via wholesale power purchase agreements), to virtual direct (e.g. avoided costs by managing building usage dynamically) or financial (e.g. switching rate plans to more beneficial rates or suppliers).

#### Needs:

* **Use Case 1 Needs:**
Like energy management, the calculations for assessing energy projects typcially require the specific utility data for a specific entity.
So, all of the needs for energy management often are needed for assessing energy projects, both for the initial feasibility analysis and ongoing project measurement and verification ([M&V](https://en.wikipedia.org/wiki/Measurement_and_Verification)).

* **Bill and Cost Breakdowns:**
Energy projects are frequently analyzed based on financial payback or savings calculations (e.g. converting a fleet of company vehicles to EVs would remove gasoline costs while incurring more electricity costs).
Thus, to address this use case the specifications must consider how to obtain utility customer bill and cost breakdown information, from distributor and retailer, so that these complex financial analyses can be performed, both for initial financial analysis and ongoing cost monitoring.

#### Examples of users in this use case:

* Property owners looking to install smart energy components at their properties (EV charging, distributed solar, etc.)

* Contractors, vendors, consultants, and advisors who work with property owners on energy-related projects (EV charging vendors, solar installers, ESCOs, etc.)

* Building control and energy management systems that dynamically modify the behavior of equipment based on energy use impact or customer savings (demand charge reduction via batteries, dynamic EV charging, HVAC controls, etc.)

* Platforms and firms that analyze customer rate plan options to maximize customer savings and goals (time-of-use, community solar, direct power purchase agreements, etc.)


## Use Case 3: Grid Flexibility <a id="use-case-grid-flexibility" href="#use-case-grid-flexibility" class="permalink">🔗</a>

*"In the past, we would forecast load and deploy generation. In the future, we will forecast generation and deploy load." -overheard at utility conference*

While the first two use cases focus on individual entities' energy footprint, as more and more non-dispatchable renewables get deployed, the grid will increasingly need sources of dynamic load flexibility to partially address the intermittency of solar and wind generation.
Technologies to allow for higher and higher penetration of renewables on the grid can often be installed "behind the meter" as distributed energy resources (DERs) or virtual power plants (VPPs).

While these technologies do not directly reduce emissions, they allow for higher penetration of new, clean energy technologies.
Thus, this working group considers grid flexibility and load management technologies a key use case for bolstering energy management efforts.

#### Needs:

* **Use Case 1 Needs:**
Like energy management, the calculations for assessing the eligibility of deploying or using a distributed grid flexibility or load management technology typically requires the specific utility data for a specific entity.
So, all of the needs for energy management often are needed for assessing distributed grid flexibility technologies, both for the initial feasibility analysis and ongoing measurement and verification ([M&V](https://en.wikipedia.org/wiki/Measurement_and_Verification)).

* **Program Eligibility and Participation Information:**
Because distributed grid flexibility and load management technologies are active participants in managing load on the electrical grid, they often are registered as part of a utility or central operator program (e.g. demand response programs).
Thus, the specifications to address this use case must consider how to include program eligibility and participation details as part of the standardized customer dataset.

* **Program Market and Settlement Data:**
When customers and energy service providers sign up to participate in grid flexibility programs, compensation for their participation is often "settled" in a program-specific manner defined by regulators, energy markets, regional operators (ISOs/TSOs/DSOs), or utilities.
To calculate these settlements, participating entities, such as VPP operators, typically need to receive relevant market data and submit data about their behavior, operations, and relevant logs to central grid operators or markets.
Thus, the specifications to address this use case must consider how to include data formats of program-related market data and settlement results as part of the standardized customer dataset, as well as data formats and protocols for external entities to  submit relevant program participation data to central entities.
NOTE: This need does NOT include creating data formats and protocols for live market data and pricing, live grid status information, or live operation and control of assets (see [Not In Scope](#not-in-scope)).

#### Examples of users in this use case:

* Apps and devices that participate in demand response programs (
[FERC Order 2222](https://ferc.gov/media/ferc-order-no-2222-fact-sheet),
[CEC Load Management Standards](https://www.energy.ca.gov/programs-and-topics/topics/load-flexibility/load-management-standards),
[NYSERDA Demand Response Programs](https://www.nyserda.ny.gov/All-Programs/Energy-Storage-Program/Energy-Storage-for-Your-Business/Demand-Response-Programs),
etc.)

* DER aggregators that manage collections of distributed resources and participate in grid events and markets (
[OhmConnect](https://www.ohmconnect.com/),
[Enel Demand Response](https://www.enelnorthamerica.com/solutions/energy-solutions/demand-response),
etc.)

* Apps and devices that monitor grid conditions and control DERs and IoT prevent additional generation resources from coming online (e.g. avoided emissions).

* Utilities needing to standardize customer data access and sharing across multiple divisions within the utility itself, between internal vendors, or with utility program implementers (energy efficiency rebate programs, building electrification initiatives, etc.).


## Use Case 4: Building Benchmarking <a id="use-case-benchmarking" href="#use-case-benchmarking" class="permalink">🔗</a>

Municipalities and governments are increasingly requiring that building owners submit whole-building energy usage information in order to do regional energy reporting and measure building energy efficiency for their jurisdiction.

#### Needs:

* **Standardized Whole-Building Data Formats:**
Whole-building usage data needed to perform building benchmarking calculations is similar to Use Case 1: Energy Management, only usage can be aggregated over multiple usage points and be provided as a single combined reading.
Thus, the specifications to address benchmarkings must consider how to appropriately define how data is being aggregated, and where necessary provide meta data on the underlying individual components that were used to create the whole-building aggregate usage values.

* **Streamlined Data Request and Access Procedures:**
Unlike individual customer data requests, which are typically sent to and approved by individual customers, benchmarking data requests are typically sent to and approved by the utility directly.
This means the dynamics and requirements of a "data request" changes for this Use Case.
Thus, specifications that address this use case must consider how to best structure user data requests submissions directly to utilities rather than individual customers.
Additionally, specifications must consider that when a data request is approved, what data access procedures will be used, and will they be different from data access procedures from the other Use Cases.

* **Privacy and Consent Requirements:**
While individual customer data access frequently requires first obtaining consent from the customer, jurisdictions requiring whole-building data access often specify thresholds over which individual customer consent is not required to obtain whole-building usage data (e.g. if any individual customer's usage is no more than 15% of the whole-building usage).
If whole-building usage is under these thresholds, however, individual customer consent will still be required for each building tenant.
Thus, specifications addressing this use case must consider how to incorporate customer consent thresholds for whole-building access, and like the other use cases, the process by which users may request and obtain customer consent when required.

#### Examples of users in this use case:

* Cities and regions with building benchmarking mandates (
[Fort Collins Building Energy and Water Scoring](https://www.fcgov.com/bews/),
[Atlanta Building Efficiency](https://www.benchmarkatl.com/),
[California Building Energy Benchmarking Program](https://www.energy.ca.gov/programs-and-topics/programs/building-energy-benchmarking-program),
etc.)

* Property owners wanting to benchmark their buildings achieve green and efficiency ratings (
[Energy Star](https://www.energystar.gov/buildings),
[LEED](https://www.usgbc.org/leed),
etc.)


## Use cases NOT in scope for this working group <a id="not-in-scope" href="#not-in-scope" class="permalink">🔗</a>

The focus of this working group is to write specifications for data formats and secure protocols for Customer Data, not creating protocols for the live management and operation of grid assets or live participation in energy and financial markets.
Thus, we have limited our scope to only providing asynchronous (i.e. not live) access to and submission of Customer Data.
Below are some examples of uses cases that we consider outside of our scope as a working group.
We believe other working groups, specifications, and organizations are a better fit for addressing these use cases.

* Live grid operations and controls (actual demand response signaling for equipment, solar inverter control and generation curtailment, EV charging shut-off signals, etc.)

* Financial transactions (customer utility bill payments, load management program participation financial payouts, etc.)

* Modifying customer accounts (changing a customer's rate plan, starting or stopping utility services, etc.)

* Providing live data access to market or region-level datasets (ISO load and generation curves, capacity market live pricing data, etc.)

* Environmental Attribute Credit registry integrations for exchanging information and data on issued or retired granular certificates

* Disaggregation and analysis methods for aggregate datasets, either for single meter disaggregation (e.g. home appliance enumeration) or multi-meter analysis (e.g. whole-building-to-individual-tenant)

* Defining algorithms and methodologies for calculating energy report results (Scope 2 carbon emissions, market settlement amounts for participating entities, etc.)
