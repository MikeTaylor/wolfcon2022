# Outline for Modularity talk

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
    * [Distribution of work across multiple roles](#distribution-of-work-across-multiple-roles)
    * [Installing apps](#installing-apps)
    * [The Okapi console](#the-okapi-console)
    * [Towards an app-store](#towards-an-app-store)
* [Part 4. The purpose **(5 minutes)**](#part-4-the-purpose-5-minutes)


## Abstract

From [the conference website](https://wolfcon2022.sched.com/event/14ANV/folio-modularity-in-practice-seamless-deployment-of-modules-from-multiple-sources):

> From the earliest genesis of FOLIO it was conceived as a modular system, in which any installation can use any set of modules. For good and practical reasons, the modules included in the flower releases are carefully audited and tested, but this has the side-effect of making it awkward to develop or deliver modules from outside of the curated set that make up those releases. The FOLIO architecture already has the necessary facilities to allow the inclusion of modules from multiple sources, much as Linux-based operating systems can install packages from multiple sources. In this session, we will survey the architectural underpinnings that enable a practical approach to modularity, and share our experiences in this area.

This is a riff on the abstract of my recent paper, [_Modularity in FOLIO: Principles, Techniques and Tools_](https://journal.calaijol.org/index.php/ijol/article/view/208) (_International Journal of Librarianship_ 6(2), 2021, doi:10.23974/ijol.2021.vol6.2.208):

> From its earliest inception, FOLIO was conceived not as an ILS (Integrated Library System), but as a true Services Platform, composed of many independent but interdependent modules, and forming a foundation on which an ILS or other library software could be built out of relevant modules. This vision of modularity is crucial to FOLIO’s appeal to the library community, because it lowers the bar to participation: individual libraries may create modules that meet their needs, or hire developers to do so, or contribute to funding modules that will be of use to a broader community — all without needing “permission” from a central authority. The technical design of FOLIO is deeply influenced by the requirements of modularity, with the establishment of standard specifications and an emphasis on machine-readable API descriptions. While FOLIO’s modular design has proved advantageous, it also introduces difficulties, including cross-module searching and data consistency. Some conventions have been established to address these difficulties, and others are in the process of crystallizing. As the ILS built on FOLIO’s platform grows and matures, and as other application suites are built on it, it remains crucial to resist the shortcuts that monolithic systems can benefit from, and retain the vision of modularity that has so successfully brought FOLIO this far.



## Part 1. The past **(10 minutes)**


### Introduction: FOLIO was designed to be modular

XXX discuss use of interfaces to make modules plug-compatible


### Structure: system diagram with apps made of UI and backend modules

XXX



## Part 2. The present **(5 minutes)**


### Organizational structure: platform-complete and flower releases

XXX


### Result: a modular monolith

XXX



## Part 3. The future **(40 minutes)**


### Smaller, lighter base platform

XXX platform-core

XXX Stripes dependence on service points


### Remove requirement for UI modules to be in `@folio` space

XXX Michal's work


### A way of packaging apps

XXX FAM format, part 1


### A way of certifying apps

XXX FAM format, part 2


### A place to upload apps

XXX Module descriptor registry

XXX Docker Hub, GitHub Packages, NPM, etc.

XXX Our own CI choices; yours may be different

XXX We don't want a parallel Index Data monolith, but an ecosystem

XXX GitHub repos of FAM files


### Distribution of work across multiple roles

XXX developer, packager, FOLIO admin, tenant admin


### Installing apps

XXX `add-app-to-platform`

XXX deployment plugins


### The Okapi console

XXX Currently hidden in Developer Settings


### Towards an app-store

XXX mod-app-manager



## Part 4. The purpose **(5 minutes)**

XXX We want _everyone_ to be able to contribute

XXX No single chokepoint

XXX A functioning ecosystem with competition

XXX We want FOLIO to outlive EBSCO and Index Data



