# Joint CDS WG3 (Customer Data) Meeting 2026-08-20

Recording: TODO

## Agenda
* Welcome
* Review Pull Request #40 (adding Bill Section `service_programs` field):
    * Pull Request: https://github.com/lfe-cds/CDS-Customer-Data/pull/40
    * Relevant issue: https://github.com/lfe-cds/CDS-Customer-Data/issues/39
    * Rendered spec with these changes: https://daniel-roesler.github.io/CDS-Customer-Data/specs/cds-wg3-01/dev#bill-section-format

## Attendees
* Daniel Roesler (Maintainer)
* Eloi Ferrer (Flexidao)

## Minutes
* Welcome and intros
* Review Pull Request #40
* Discuss lifecycle of spec merging and versioning
    * individual pull requests per issue/topic
    * pull requests can still be merged while still in draft (i.e. `dev` version)
    * when stable (e.g. no pull requests in a while), can call for consensus for `v1` version
    * Ideally would like to have pilot deployments in place for `dev` before calling for `v1`, so we know it works in the real world
* Discussed demo status:
    * Demo server: https://cds.drxgateway.com/.well-known/cds-server-metadata.json
    * Example commands to test: https://cds-registration.lfenergy.org/specs/cds-wg1-02/overview#examples

## Closing Discussion
* Consensus to commit this to repo? Yes
