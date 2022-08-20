# Outline for Modularity talk

Mike Taylor <mike@indexdata.com>,
Index Data.

<!-- md2toc -l 2 modularity-outline.md -->
* [Abstract](#abstract)
* [Part 1. The past **(10 minutes)**](#part-1-the-past-10-minutes)
    * [Introduction: FOLIO was designed to be modular](#introduction-folio-was-designed-to-be-modular)
    * [Structure: system diagram with apps made of UI and backend modules](#structure-system-diagram-with-apps-made-of-ui-and-backend-modules)
* [Part 2. The present **(5 minutes)**](#part-2-the-present-5-minutes)
    * [Organizational structure: platform-complete and flower releases](#organizational-structure-platform-complete-and-flower-releases)
    * [Result: a modular monolith](#result-a-modular-monolith)
* [Part 3. The future **(40 minutes)**](#part-3-the-future-40-minutes)
    * [Smaller, lighter base platform](#smaller-lighter-base-platform)
    * [Remove requirement for UI modules to be in `@folio` space](#remove-requirement-for-ui-modules-to-be-in-folio-space)
    * [A way of packaging apps](#a-way-of-packaging-apps)
    * [A way of certifying apps](#a-way-of-certifying-apps)
    * [A place to upload apps](#a-place-to-upload-apps)
    * [Towards an app-store](#towards-an-app-store)
    * [Distribution of work across multiple roles](#distribution-of-work-across-multiple-roles)
    * [Installing apps](#installing-apps)
    * [The Okapi console](#the-okapi-console)
* [Part 4. The purpose **(5 minutes)**](#part-4-the-purpose-5-minutes)


## Abstract

From [the conference website](https://wolfcon2022.sched.com/event/14ANV/folio-modularity-in-practice-seamless-deployment-of-modules-from-multiple-sources):

> From the earliest genesis of FOLIO it was conceived as a modular system, in which any installation can use any set of modules. For good and practical reasons, the modules included in the flower releases are carefully audited and tested, but this has the side-effect of making it awkward to develop or deliver modules from outside of the curated set that make up those releases. The FOLIO architecture already has the necessary facilities to allow the inclusion of modules from multiple sources, much as Linux-based operating systems can install packages from multiple sources. In this session, we will survey the architectural underpinnings that enable a practical approach to modularity, and share our experiences in this area.

(This is a riff on the abstract of my recent paper, [_Modularity in FOLIO: Principles, Techniques and Tools_](https://journal.calaijol.org/index.php/ijol/article/view/208), _International Journal of Librarianship_ 6(2), 2021, doi:10.23974/ijol.2021.vol6.2.208)



## Part 1. The past **(10 minutes)**


### Introduction: FOLIO was designed to be modular

* A lot of the early ideas were thrashed out in Sint Maarten in December 2015
* It's frightening to realise that's nearly seven years ago
* From the start we had the notion of separate modules, all fronted by Okapi
* (This is not "microservices" where each service has its own API.)
* From the start, dependencies were on interface, not implementation
* Interface dependency allows completing implementations of the same interface


### Structure: system diagram with apps made of UI and backend modules

* You can think of a FOLIO system in three horizontal slices: Stripes-based UI, Okapi and modules
* Or you can think of it in many vertical slices, one per app (Users, Inventory, Settings, etc.)
* Most vertical-slice apps consist of a UI module and a backend module (e.g. `ui-users` + `mod-users`)
* Some are bigger and involve multiple UI or backend modules (e.g. ERM, ReShare)
* (According to [documentation](https://lotus.docs.folio.org/docs/erm/), ERM consists of Agreements, eHoldings, eUsage, Licenses, Local KB Admin and ERM Comparisons)

> **_[[Diagrams here]](https://docs.google.com/presentation/d/1tBI8urMK-MU6w_bjO-cudSo3KySBHuTPjCVB_sP6ECU/edit#slide=id.g14587eada5c_0_82)_**



## Part 2. The present **(5 minutes)**


### Organizational structure: platform-complete and flower releases

* We have all this flexibility where we can run any modules we want
* But we need flower releases because they are stable, tested and reliable
* But the result is that we usually have _all_ and _only_ the core set of modules
* Where is the flourishing ecosystem of competing apps from different providers?


### Result: a modular monolith

This situation has been difficult to break out of because:
* The base platform is too big to build custom FOLIO subsets on
* There are technical difficulties in adding apps not in the core
* There is no way to package modules together into an app
* There is no way to certify the origin of an app, or properties such as internationalization.
* There are no established places to upload apps, or download them from
* There is no way to discover apps that do exist
* Installing apps (UI and backend modules) is cumbersome
* There is no Okapi Console that can be used to enable/disable modules, etc.



## Part 3. The future **(40 minutes)**


### Smaller, lighter base platform

* We think of wanting to _add_ modules, but we may also want to pare down to a lean subset
* We might be able to make lean, slick checkin/checkout apps, for example
* At present the only platform supported by the OLF is [`platform-complete`](https://github.com/folio-org/platform-complete/), which is what we use to build https://folio-snapshot.dev.folio.org/
* This has 140 backend and edge modules, 29 UI apps, 30 UI settings modules, and 18 UI plugins
* We are working on a much smaller `platform-core`
* Difficult because Stripes logs in with `mod-users-bl` instead of `mod-login`, and that pulls in many other modules
* We also need to remove the concept of "service points" from being hardwired into Stripes


### Remove unexamined assumptions in the code

* e.g. Michal's work to support UI modules not in the `@folio` namespace


### A way of packaging apps

* FAM format (FOLIO Application Metadata)
* JSON-based format that points to multiple UI and backend packages:
  * Machine-readable fields (`name`, `version`)
  * Human-readable fields (`displayName`, `description`)
  * Array of installable `elements`
* Example:
```
{
  "name": "harvester-admin",
  "displayName": "Harvester admin",
  "version": "0.0.1",
  "description": "Admin console for the Index Data Harvester",
  "elements": [
    {
      "type": "ui",
      "url": "https://repository.folio.org/repository/npm-folio/@indexdata/harvester-admin",
      "descriptor": "https://registry.folio-dev.indexdata.com/_/proxy/modules/indexdata_harvester-admin-0.1.0",
      "required": true
    },
    {
      "type": "backend",
      "url": "ghcr.io/indexdata/mod-harvester-admin:v0.1.0-SNAPSHOT.7",
      "descriptor": "https://registry.folio-dev.indexdata.com/_/proxy/modules/mod-harvester-admin-0.1.0-SNAPSHOT",
      "required": true
    }
  ]
}
```
* Not yet supported: internationalization of human-readable strings


### A way of certifying apps

* We need to know who packed the app is taking resposibility for it
* We also want to know who has made claims about its accessibility, security, etc.
* Each FAM carries an array of signed certifications:
  * `type` says what is being asserted
  * `certifier` is the top-level domain of the entity asserting it
  * `signature` is a digest of the FAM file signed with the certifier's private key (and so verifiable with the corresponding public key)
* Example:
```
{
  ...,
  "certified": [
    {
      "type": "published",
      "certifier": "indexdata.com",
      "signature": "1a2b3c4d5e6f7g8h9i0j"
    },
    {
      "type": "ux",
      "certifier": "samhaeng.com",
      "signature": "1234567890abcdefghij"
    }
  ]
}
```


### A place to upload apps

An app consists of several parts:
* The FAM file
* Module descriptors for UI and backend modules
* NPM packages for UI modules
* Docker containers for backend modules

Where will we keep all these things?

* FAM files in GitHub repositories, e.g. https://github.com/MikeTaylor/example-folio-app-source
* Module descriptors in publicly accessible Okapi instances:
  * The FOLIO project's registry at http://folio-registry.aws.indexdata.com:80 XXX outdated
  * Index Data's registry at https://registry.folio-dev.indexdata.com
  * Any other registry that anyone stands up
* NPM packages in any NPM registry:
  * The FOLIO NPM registry (where core modules live)
  * The global NPM registry
  * Any other registry that anyone stands up
* Docker containers in any Docker registry:
  * DockerHub
  * FOLIO's DockerHub mirror
  * GitHub packages at https://ghcr.io
  * Any other registry that anyone stands up

* We at Index Data have made our own choices; yours may be different, and that's fine.
* We don't want an Index Data monolith, parallel with the existing FOLIO monolith
* We want an ecosystem populated by many actors


### Towards an app-store

* Anyone can publish FAM files, but how can they be discovered?
* `mod-app-manager` exists as a prototype
* Its configuration stores details about GitHub repositories
* It can return all the FAM files from all those repositories
* We have a very primitive UI in a branch of the Developer Console
* This is the beginning of an app-store that can be used for:
  * Platform administrators to add apps to platforms and deploy them
  * Tenant administrators to enable and disable apps for their tenant
  * Users to browse what exists and make installation requests


### Installing apps

* We have a script, `add-app-to-platform`
  * It reads a FAM file
  * It inserts the module descriptors of all elements
  * It deploys backend modules
  * (It does _not_ yet build and publish new Stripes bundles)
* Deployment is different for different vendors
  * `add-app-to-platform` uses a simple deployment-plugin API
  * There is currently an `okapi` plugin that uses Okapi's own deployment mechanism
  * We plan a Kubernetes plugin
  * Others are possible, e.g. DockerSwarm
* We want to build a FOLIO module that can do these things
* Then we can make a UI to allow all this to be done within FOLIO itself


### The Okapi console

* What would an admin UI look like?
* We have prototyped an Okapi Console
* It is currently hidden in Developer Settings


## Part 4. The purpose **(5 minutes)**

* We want _everyone_ to be able to contribute -- not just a few anointed organizations
* We want the barrier-to-entry to be very low, while retaining quality stamps
* We want no single chokepoint to making apps available in FOLIO
* We want work to be distributed work across multiple roles: developer, packager, certifier, platform admin, tenant admin
* We want a functioning ecosystem with competition
* We want FOLIO to outlive EBSCO and Index Data



