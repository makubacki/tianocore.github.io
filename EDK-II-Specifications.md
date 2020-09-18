# **EDK II Specifications**
This page contains the released versions of the EDK II Specifications published using [Gitbook](https://gitbook.com/).<br>
* The latest draft versions of the EDK II Specification can be found on the [[EDK II Draft Specification]] page.
* The previous versions of the EDK II Specifications can be found on the [[EDK II Specifications Archived]] page.<Br>

For an understanding of the basic setup and to see examples of how to use the EDK II Specification files [.DEC](#dec),[.DSC](#dsc) and [.INF](#inf), see the wiki page [[Build Description Files]].<Br>

The _EDK II Template Specification_ is an example document that may be used as a template 
for a new document.  For complete details on how to contribute to TianoCore documents, please
see the information [here](https://github.com/tianocore-docs/edk2-TemplateSpecification/wiki).

**_Descriptions for each specification is listed below._**

| EDK II Specification | Revision  | Date | Download |
| ---------------------| --------- | ---- |---------------------------------------------|
|[Build](#build) |v1.30   | July 2019   | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-build-specification/), [Github ](https://github.com/tianocore-docs/edk2-BuildSpecification) |
|[DEC](#dec)     |v1.29  | July 2019 |[Gitbook](https://edk2-docs.gitbook.io/edk-ii-dec-specification), [Github ](https://github.com/tianocore-docs/edk2-DecSpecification)|
|[DSC](#dsc)     |v1.30   | July 2019  | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-dsc-specification), [Github ](https://github.com/tianocore-docs/edk2-DscSpecification)|
|[FDF](#fdf)     |v1.29   | March 2019  |  [Gitbook](https://edk2-docs.gitbook.io/edk-ii-fdf-specification), [Github ](https://github.com/tianocore-docs/edk2-FdfSpecification)|
|[IDF](#idf)     |v1.0    | April 2017 | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-idf-specification), [Github ](https://github.com/tianocore-docs/edk2-IdfSpecification/tree/release/1.00) |
|[INF](#inf)     |v1.29   | July 2019  |  [Gitbook](https://edk2-docs.gitbook.io/edk-ii-inf-specification), [Github ](https://github.com/tianocore-docs/edk2-InfSpecification) |
|[Meta-Data](#meta-data)  | v1.30      | March 2018|  [Gitbook](https://edk2-docs.gitbook.io/edk-ii-meta-data-expression-syntax-specification/), [Github ](https://github.com/tianocore-docs/edk2-MetaDataExpressionSyntaxSpecification/tree/release/1.30) |
|[PCD](#pcd)     | v0.56  | April 2017 | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-pcd-specification), [Github ](https://github.com/tianocore-docs/edk2-PcdSpecification/tree/release/0.56) |
|[UNI](#uni)     | v1.4   | May 2017   | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-uni-specification/), [Github ](https://github.com/tianocore-docs/edk2-UniSpecification/tree/release/1.40) |
|[VFR](#vfr)     | v1.92  | April 2018 |[Gitbook](https://edk2-docs.gitbook.io/edk-ii-vfr-specification), [Github ](https://github.com/tianocore-docs/edk2-VfrSpecification/tree/release/1.92) |
| [C Coding Standards](#c-coding-standards) | v 2.2 | June 2017 | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-c-coding-standards-specification), [Github ](https://github.com/tianocore-docs/edk2-CCodingStandardsSpecification/tree/release/2.20)|
|[Min-Platform](#Min-Platform) |v 0.7| May 2019 | [Gitbook](https://edk2-docs.gitbook.io/edk-ii-minimum-platform-specification/), [Github ](https://github.com/tianocore-docs/edk2-MinimumPlatformSpecification)









---

## Build 
Build Specification -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-build-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-BuildSpecification)
\] - This document describes the EDK II Build Architecture. This specification was designed to support new build requirements for building EDK II modules and EDK components within the EDK II build infrastructure as well as to generate binary firmware images and Unified Extensible Firmware Image (UEFI) applications.

## DEC
Declaration file format-\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-dec-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-DecSpecification)
\] - This document describes the EDK II Declaration (DEC) file format. This format was designed to support building packaging and distribution of EDK II modules, as well as for building platforms and modules using the EDK II build infrastructure. EDK II declaration files may be created during installation of a distribution that follows the UEFI Platform Initialization Distribution Package Specification. They may also be created manually. The EDK II Build Infrastructure supports generation of UEFI 2.5 and PI 1.4 (Unified EFI, Inc.) compliant binary images.

## DSC
Platform Description file format -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-dsc-specification/details),
[GitHub ](https://github.com/tianocore-docs/edk2-DscSpecification)
\] -This document describes the EDK II Platform Description file (DSC) format. The EDK Build Tools are included as part of the EDK II compatibility package. In order to use EDK II Modules or the EDK II Build Tools, an EDK II DSC and FDF file must be used. EDK II tools use INI style text based files to describe components, platforms and firmware volumes. While similar to EDK DSC files, the EDK II DSC file format is different, and new utilities have been provided to process these files. The EDK II Build Infrastructure supports creation of binary images that comply with Unified EFI (UEFI) 2.5 and UEFI Platform Infrastructure (PI) 1.4 specifications.


## FDF
Flash Description file format - \[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-fdf-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-FdfSpecification)
\] - This document describes the EDK II Flash Description (FDF) file format. This format was designed to support new build requirements of building EDK and EDK II modules within the EDK II build infrastructure. The EDK II Build Infrastructure supports generation of current Unified EFI, Inc. (UEFI 2.5 and PI 1.4) compliant binary images. The FDF file is used to describe the content and layout of binary images. Binary images described in this file may be any combination of boot images, capsule images or PCI Options ROMs.

## IDF
Image Definition IDF File Format Specification -  \[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-idf-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-IdfSpecification)
\] -
This document describes file format for Image Description files that are used to create HII Image Packages introduced in the Unified Extensible Firmware Interface Specification, Version 2.1.

## INF
information file format - \[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-inf-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-InfSpecification)
\] - This document describes the EDK II build information (INF) file format. This format supports the new build requirements of build EDK components and EDK II modules within the EDK II build infrastructure. The EDK II Build Infrastructure supports creation of binary images that comply with Unified EFI (UEFI) 2.5 and UEFI Platform Infrastructure (PI) 1.4 specifications.

## Meta-Data
Meta-Data Expression Syntax Specification -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-meta-data-expression-syntax-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-MetaDataExpressionSyntaxSpecification)
\] - This document describes the syntax of expression statements for EDK II Meta-data files used in data fields, feature flag expressions and conditional directive statements.

## PCD
EDK II PCD Specification -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-pcd-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-PcdSpecification)
\] - This document discusses the mechanisms and configuration entries required to make it easy to write portable silicon modules and to port the Framework from platform to platform.


## UNI
Unicode Format File Specification -  \[
[Gitbook](https://edk2-docs.gitbook.io/edk2-docs/edk-ii-uni-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-UniSpecification)
\] - This document describes the Multi-String build information (UNI) file format . See details in the Revision History in the document for more details.

## VFR
Visual Form Representation Specification -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-vfr-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-VfrSpecification)
\] - To simplify the creation of Internal Forms Representation (IFR), a high-level Visual Forms Representation (VFR) language is described in this document. Using this language syntax, a compiler can be designed to take an ordinary text file containing VFR as an input, and output IFR for use in a user’s program. There are various methods to define the VFR language.


## C Coding Standards
EDK II C Coding Standards Specification - \[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-c-coding-standards-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-CCodingStandardsSpecification)
\] - The EDK II C Coding Standards Specification establishes a set of rules intended not as
a constraint, but as an enabling philosophy which will:
  * Establish uniformity of style.
  * Set minimum information content requirements.
  * Allow all programmers to easily understand the code.
  * Facilitate support tasks by establishing uniform conventions.
  * Ease documentation efforts by embedding the design documentation in the code.
  * Reduce customer and new employee learning curves by providing accurate code documentation and uniform style.

  These rules apply to all code developed.

## Min-Platform
EDK II Minimum Platform Specification -\[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-minimum-platform-specification/), [Github ](https://github.com/tianocore-docs/edk2-MinimumPlatformSpecification)\]


This specification details the required and optional elements for an EDK II
based platform design with the following objectives:<br>
1. Define a structure that enables developers to consistently navigate source code, execution flow, and the functional results of bootstrapping a system.
2. Enable a minimal platform where minimal is defined as the minimal firmware implementation required to produce a basic solution that can be further extended to meet a multitude of client, server, and embedded market needs.
3. Minimize coupling between common, silicon, platform, and board packages.
4. Enable large granularity binary solutions.
<br>
A key aspect of these objectives is to improve the transparency and security
quality across the client, server, and embedded ecosystems.


---

* **_EDK II Template Specification_** \[
[Gitbook](https://edk2-docs.gitbook.io/edk-ii-template-specification),
[GitHub ](https://github.com/tianocore-docs/edk2-TemplateSpecification)
\] This document is a template that can be copied to start a new Tianocore Gitbook document. It also provides examples for styles and formats commonly found in Tianocore specifications.

