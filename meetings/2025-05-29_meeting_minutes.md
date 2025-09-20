# Joint CDSC WG1 & WG3 (Connectivity and Customer Data) Meeting 2025-05-29

Recording: https://zoom.us/rec/share/NfWVC4AZVm0_K7s5QlTpbg1PF4Sg6NFKfgOCu_NdPswydS0DdKsO1TfJAhMP5ZaV.Jj0YwP1mC91_mOnx

## Agenda
* Welcome
* Name change
    * Carbon Data Specification Consortium (CDSC) --> Connected Data Specifications
    * Connectivity WG --> Registration WG
* Starting process for merging in WG1 use cases and specs

## Attendees
* Daniel Roesler (UtilityAPI) (Maintainer)
* Don Coffin (GBA)
* Madhuban Kumar (CarbonLaces)

## Minutes
* Welcome
* Name change!
    * Carbon Data Specification Consortium (CDSC) --> Connected Data Specifications (CDS)
    * Connectivity WG (WG1) --> CDS Registration WG (WG1)
    * Customer Data WG (WG3) --> CDS Customer Data WG (WG3)
    * mailing lists have been updated (so update your address books)
        * cdsc-connectivity-wg@lists.lfenergy.org --> cds-registration-wg@lists.lfenergy.org
        * cdsc-customer-data-wg@lists.lfenergy.org --> cds-customer-data-wg@lists.lfenergy.org
    * Websites:
        * carbondataspec.org --> cds.lfenergy.org
        * connectivity.carbondatapsec.org --> cds-registration.lfenergy.org
        * customerdata.carbondataspec.org --> cds-customerdata.lfenergy.org
    * Official github:
        * github.com/carbon-data-specification --> github.com/lfe-cds
        * /Connectivity.git --> /CDS-Registration.git
        * /Customer-Data.git --> /CDS-Customer-Data.git
    * Maintainer branches:
        * github.com/daniel-utilityapi/Connectivity --> github.com/daniel-utilityapi/CDS-Registration
        * github.com/daniel-utilityapi/Customer-Data --> github.com/daniel-utilityapi/CDS-Customer-Data
        * daniel-utilityapi.github.io/Connectivity/ --> daniel-utilityapi.github.io/CDS-Registration/
        * daniel-utilityapi.github.io/Customer-Data/ --> daniel-utilityapi.github.io/CDS-Customer-Data/
    * Server metadata endpoint
        * /.well-known/carbon-data-spec.json --> /.well-known/cds-server-metadata.json
    * prefixes/namespace remains (cds_*)

* Now that name change is done, ready to start calling for merges of PRs
    * Use Cases for WG1
        * https://github.com/lfe-cds/CDS-Registration/pull/6
        * Render: https://daniel-utilityapi.github.io/CDS-Registration/use-cases
    * CDS-WG1-01 (Server Metadata)
        * https://github.com/lfe-cds/CDS-Registration/pull/4
        * Render: https://daniel-utilityapi.github.io/CDS-Registration/specs/cds-wg1-01/
        * NOTE: this merge proposal approval is NOT for adoption as final, it's merely to merge in the _draft_ as a DRAFT
    * CDS-WG1-02 (Client Registration)
        * https://github.com/lfe-cds/CDS-Registration/pull/5
        * Render: https://daniel-utilityapi.github.io/CDS-Registration/specs/cds-wg1-02/
        * NOTE: this merge proposal approval is NOT for adoption as final, it's merely to merge in the _draft_ as a DRAFT
    * CDS-WG3-01 (Customer Data)
        * Not ready yet for PR consensus merge request
        * Target for calling for PR merge is July

* Process for merging PRs
    * Maintainer emails mailing list calling for consensus in merging PR
    * Include a deadline for feedback/approval/denial
    * Approvals and Denials post their feedback/approval/denial in PR
    * Once feedback/denials are addressed, merge in PR

## Closing Discussion
* Consensus to commit this to repo? Yes

