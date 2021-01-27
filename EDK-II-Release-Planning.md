# edk2-stable202102 tag planning

## Proposed Schedule

| Date (00:00:00 UTC-8)| Description                              |
| ---------------------| ---------------------------------------- |
| 2020-11-27           | Beginning of development                 |
| 2021-02-15           | Feature Planning Freeze                  |
| 2021-02-22           | [Soft Feature Freeze](SoftFeatureFreeze) |
| 2021-03-01           | [Hard Feature Freeze](HardFeatureFreeze) |
| 2021-03-05           | Release                                  |

## Proposed Features
* [CryptoPkg/OpensslLib: Add native instruction support for X64](https://bugzilla.tianocore.org/show_bug.cgi?id=2507)
* [ArmVirtPkg: support extra pci root bridges (pxb)](https://bugzilla.tianocore.org/show_bug.cgi?id=3059)
* [SEV Encrypted Boot for Ovmf (remote attestation)](https://bugzilla.tianocore.org/show_bug.cgi?id=3077)
* [virtio-fs driver for OvmfPkg and ArmVirtPkg](https://bugzilla.tianocore.org/show_bug.cgi?id=3097)
* [Apply SEV-ES mitigations for encryption bit position and MMIO](https://bugzilla.tianocore.org/show_bug.cgi?id=3108)
* [OVMF RFE: VCPU hot-unplug with SMI](https://bugzilla.tianocore.org/show_bug.cgi?id=3132)
* [Add Core CI support for StandaloneMmPkg](https://bugzilla.tianocore.org/show_bug.cgi?id=3150)
* [Update LZMA module to LZMA SDK latest version 19.00](https://bugzilla.tianocore.org/show_bug.cgi?id=3101)
* [IntelFsp2Pkg: Support FSP private temporary memory](https://bugzilla.tianocore.org/show_bug.cgi?id=3153)
* [Port open source JSON library (jansson)](https://bugzilla.tianocore.org/show_bug.cgi?id=3163)
* [add file buffering to the UEFI shell's COMP command](https://bugzilla.tianocore.org/show_bug.cgi?id=3123)
* [Shell: pathname / filename sorting](https://bugzilla.tianocore.org/show_bug.cgi?id=3151)
* [Extend support of peripheral x64 MM_STANDALONE drivers](https://bugzilla.tianocore.org/show_bug.cgi?id=3129)
* [BaseTools: Convert the Split tool from C language to Python](https://bugzilla.tianocore.org/show_bug.cgi?id=3165)
* TBD

## Wiki
* TBD

## Update Notes
* If the user has the windows bat script that calls Split in it，it needs to change to "call Split" because Split will be a bat script but not an executable file.
* Shell depends on library class OrderedCollectionLib. Platform DSC needs to configure it in [LibraryClasses]
OrderedCollectionLib|MdePkg/Library/BaseOrderedCollectionRedBlackTreeLib/BaseOrderedCollectionRedBlackTreeLib.inf
* Some struct fields in SmBios.h have typos and get fixed in these code change [0db8](https://github.com/tianocore/edk2/commit/0db89a661f38b10012ff4f62e1853bfc48efd462), [bd9d](https://github.com/tianocore/edk2/commit/bd9da7b1da2639fcea8a156fa92a32bbc4209367), [e157](https://github.com/tianocore/edk2/commit/e157c8f9ed173a390d2c9d29069a46e9662e0d04). Details are listed below.
In struct ```SMBIOS_TABLE_TYPE17```:
&nbsp;```FirwareVersion ==> FirmwareVersion```
In struct ```SMBIOS_TABLE_TYPE4```:
&nbsp;```ProcessorManufacture ==> ProcessorManufacturer```
In struct ```PROCESSOR_CHARACTERISTIC_FLAGS```:
&nbsp;```Processor64BitCapble ==> Processor64BitCapable```
&nbsp;```ProcessorEnhancedVirtulization ==> ProcessorEnhancedVirtualization```
&nbsp;```Processor128bitCapble ==> Processor128BitCapable```
Platform code that uses those fields need modifications.
* TBD

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
* TBD

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
