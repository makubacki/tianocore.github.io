# edk2-stable202105 tag planning

## Proposed Schedule

| Date (00:00:00 UTC-8)| Description                              |
| ---------------------| ---------------------------------------- |
| 2021-03-05           | Beginning of development                 |
| 2021-05-10           | Feature Planning Freeze                  |
| 2021-05-17           | [Soft Feature Freeze](SoftFeatureFreeze) |
| 2021-05-24           | [Hard Feature Freeze](HardFeatureFreeze) |
| 2021-05-28           | Release                                  |

## Proposed Features
* [OVMF RFE: VCPU hot-unplug with SMI](https://bugzilla.tianocore.org/show_bug.cgi?id=3132)
* [Add non-MMRAM memory protection for Standalone MM environment](https://bugzilla.tianocore.org/show_bug.cgi?id=3168)
* [OpenSSL Update OpenSSL version to version 1.1.1j to include CVE fix](https://bugzilla.tianocore.org/show_bug.cgi?id=3266)
* [Add a new library class RegisterFilterLib](https://bugzilla.tianocore.org/show_bug.cgi?id=3246)
* [Add a new MicrocodeLib for microcode loading](https://bugzilla.tianocore.org/show_bug.cgi?id=3303)
* [EDKII Redfish Config Handler Protocol](https://bugzilla.tianocore.org/show_bug.cgi?id=2911)
* [Implementation of UEFI spec 31.1 Redfish Discover Protocol](https://bugzilla.tianocore.org/show_bug.cgi?id=2906)
* [Add RedfishLib (from libredfish)](https://bugzilla.tianocore.org/show_bug.cgi?id=3304)
* [Add the ArmPlatformPkg to the azurepipeline](https://bugzilla.tianocore.org/show_bug.cgi?id=3349)
* [Add the ArmPkg to the azurepipeline](https://bugzilla.tianocore.org/show_bug.cgi?id=3348)
* [Support Tcg2Smm under Standalone MM environment](https://bugzilla.tianocore.org/show_bug.cgi?id=3169)
* [UefiCpuPkg/SmmCpuFeaturesLib: Add Standalone MM support](https://bugzilla.tianocore.org/show_bug.cgi?id=3218)
* TBD

## Wiki
* TBD

## Update Notes
* MdeModulePkg VariableSmmRuntimeDxe.inf depends on library class MmUnblockMemoryLib. Platforms supporting variable service through SMM should configure platform DSC in [LibraryClasses] 
```MmUnblockMemoryLib|MdePkg/Library/MmUnblockMemoryLib/MmUnblockMemoryLibNull.inf```
* SecurityPkg Tcg2Smm is split into 2 drivers: Tcg2Smm and Tcg2Acpi. Platforms supporting TCG2 Physical Presence and Memory Clear through ACPI method should add a new entry in [Components] section of platform DSC as well as the corresponding FV section in platform FDF
```SecurityPkg/Tcg/Tcg2Acpi/Tcg2Acpi.inf```
* Platform DSC needs to include ```MdePkg/MdeLibs.dsc.inc``` for the required library instance.

# edk2-stable202108 tag planning

## Proposed Schedule

| Date (00:00:00 UTC-8)| Description                              |
| ---------------------| ---------------------------------------- |
| 2021-05-28           | Beginning of development                 |
| 2021-08-12           | Feature Planning Freeze                  |
| 2021-08-19           | [Soft Feature Freeze](SoftFeatureFreeze) |
| 2021-08-26           | [Hard Feature Freeze](HardFeatureFreeze) |
| 2021-08-30           | Release                                  |

## Proposed Features
* TBD

# edk2-stable202111 tag planning

## Proposed Schedule

| Date (00:00:00 UTC-8)| Description                              |
| ---------------------| ---------------------------------------- |
| 2021-08-30           | Beginning of development                 |
| 2021-11-08           | Feature Planning Freeze                  |
| 2021-11-15           | [Soft Feature Freeze](SoftFeatureFreeze) |
| 2021-11-22           | [Hard Feature Freeze](HardFeatureFreeze) |
| 2021-11-26           | Release                                  |

## Proposed Features
* TBD

## [Previous edk2 stable tags](https://github.com/tianocore/edk2/tags)

---
