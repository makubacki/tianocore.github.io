# edk2-stable202311 tag planning

## Proposed Schedule

| Date (00:00:00 UTC-8)| Description                              |
| ---------------------| ---------------------------------------- |
| 2023-08-25           | Beginning of development                 |
| 2023-11-06           | [Soft Feature Freeze](SoftFeatureFreeze) |
| 2023-11-10           | [Hard Feature Freeze](HardFeatureFreeze) |
| 2023-11-24           | Release                                  |

## Proposed Features / Bug Fixes
(The list extends up to and including edk2 commit f945b72331d7.)

* [UefiPayloadPkg:Enhance the build processing for Universalpayload](https://bugzilla.tianocore.org/show_bug.cgi?id=4532)
* [SplitFspBin.py cannot support FSP binary with child FV included](https://bugzilla.tianocore.org/show_bug.cgi?id=4502)
* [Cache Disable should not be set by default in CR0 after ResetVector in x64 build](https://bugzilla.tianocore.org/show_bug.cgi?id=4511)
* [SMM perf record is copied multiple times to FPDT table if multiple ReadyToBoot events are signaled](https://bugzilla.tianocore.org/show_bug.cgi?id=4470)
* [In some cases, LocateHandleBuffer() may allocate a callee freed buffer when an error occurs](https://bugzilla.tianocore.org/show_bug.cgi?id=4543)
* [OvmfPkg/IoMmuDxe: don't rely on TPLs to manage concurrency](https://bugzilla.redhat.com/show_bug.cgi?id=2211060)
* [Recent OVMF build.sh change breaks useful functionality](https://bugzilla.tianocore.org/show_bug.cgi?id=4528)
* [UEFI cryptography agile solution - separate crypto algorithm (phase I)](https://bugzilla.tianocore.org/show_bug.cgi?id=3413)
* [Use MpService2Ppi to wakeup CPU in Smm CpuS3](https://edk2.groups.io/g/devel/message/108115)
* [Move RngLibTimer from MdePkg to MdeModulePkg](https://bugzilla.tianocore.org/show_bug.cgi?id=4504)
* [EFI_RNG_PROTOCOL Describe the DRBG algorithm used in the Arm RNDR instruction](https://bugzilla.tianocore.org/show_bug.cgi?id=4441)
* [Faulty Rng algo selection for Arm](https://bugzilla.tianocore.org/show_bug.cgi?id=4151)
* [RngDxe assert](https://bugzilla.tianocore.org/show_bug.cgi?id=4491)
* [Add New Intel Processor family for SMBIOS Type 4 from SMBIOS 3.7.0](https://bugzilla.tianocore.org/show_bug.cgi?id=4547)
* [NetworkPkg: HTTP protocol throughput too small](https://bugzilla.tianocore.org/show_bug.cgi?id=4505)
* [MailBoxVersion should be 0 according to the ACPI spec 6.5](https://bugzilla.tianocore.org/show_bug.cgi?id=4527)
* [Pyrite support - Secure erase is only available if encryption is supported](https://bugzilla.tianocore.org/show_bug.cgi?id=3004)
* [Remove assembly/tool logic that creates AP waking vector in 4G-20h](https://bugzilla.tianocore.org/show_bug.cgi?id=4494)
* [MdeModulePkg/Bus/Ata/AtaBusDxe: Coverity scan flags SIGN_EXTENSION issue](https://bugzilla.tianocore.org/show_bug.cgi?id=4209)
* [MdeModulePkg/Bus/Pci/NvmExpressPei: Coverity scan flags DEADCODE issue](https://bugzilla.tianocore.org/show_bug.cgi?id=4220)
* [MdeModulePkg/Bus/Pci/UhciDxe: fix Coverity issues](https://bugzilla.tianocore.org/show_bug.cgi?id=4211)
* [DynamicTablesPkg: Add support for generating ACPI ThermalZones](https://edk2.groups.io/g/devel/message/108800)
* [DynamicTablesPkg: Add support for PCI IO using Qword resources](https://edk2.groups.io/g/devel/message/108969)
* [MdeModulePkg/XhciDxe: Use Performance Timer for XHCI Timeouts](https://bugzilla.tianocore.org/show_bug.cgi?id=2948)
* [MdeModulePkg/Bus/Pci/XhciDxe: Need to abort the command for command timeout](https://bugzilla.tianocore.org/show_bug.cgi?id=4552)
* [BaseTools: Add support for LOONGARCH64 R_LARCH_RELAX relocation](https://bugzilla.tianocore.org/show_bug.cgi?id=4559)
* [UefiPayloadPkg: Add FIT support](https://edk2.groups.io/g/devel/message/109043)
* [SMBIOS BCD revision is not match SMBIOS version](https://bugzilla.tianocore.org/show_bug.cgi?id=4544)
* [Xhci: Skip size round up for TRB when getting PCI device/host memory address](https://bugzilla.tianocore.org/show_bug.cgi?id=4560)
* [MdePkg: various fixes to ARM/AArch64 SetJump/LongJump](https://edk2.groups.io/g/devel/message/109074)
* [TlsLib should not have a list of Ciphers which may or may not agree with what is available](https://bugzilla.tianocore.org/show_bug.cgi?id=2541)
* [MtrrLib modules and Unit test Enhancement](https://edk2.groups.io/g/devel/message/108556)
* [Use the base SortLib for Redfish modules only](https://bugzilla.tianocore.org/show_bug.cgi?id=4566)
* [evaluate the feasibility of using mbedtls as crypto library](https://bugzilla.tianocore.org/show_bug.cgi?id=4177)
* [bogus RealTimeClockLib class interface: LibRtcVirtualNotifyEvent](https://bugzilla.tianocore.org/show_bug.cgi?id=4564)
* [OvmfPkg/VirtioFsDxe: tolerate opening an absolute pathname relative to a regular file](https://github.com/rhboot/shim/issues/382#issuecomment-1781922322)
* [UefiDevicePathLib DevPathToTextAcpiEx overflows the device path node when searching for optional strings](https://bugzilla.tianocore.org/show_bug.cgi?id=4555)
* [DynamicTablesPkg/TableHelperLib updates](https://edk2.groups.io/g/devel/message/109363)
* [Update Edk2-pytools to latest versions](https://edk2.groups.io/g/devel/message/109935)
* [duplicate installation of EFI_REAL_TIME_CLOCK_ARCH_PROTOCOL in RealTimeClockLib instances](https://bugzilla.tianocore.org/show_bug.cgi?id=4565)
* [UefiCpuPkg/BaseXApicX2ApicLib: fix CPUID_V2_EXTENDED_TOPOLOGY detection](https://bugzilla.redhat.com/show_bug.cgi?id=2241388)
* [OvmfPkg/AcpiPlatformDxe: Coverity scan flags FORWARD_NULL and UNUSED_VALUE issues](https://bugzilla.tianocore.org/show_bug.cgi?id=4568)
* [RedfishPkg/RedfishLib: Return HTTP headers to caller](https://edk2.groups.io/g/devel/message/109990)
* [ArmVirtPkg: support two PL011 UARTs](https://bugzilla.tianocore.org/show_bug.cgi?id=4577)

## [Previous edk2 stable tags](https://github.com/tianocore/edk2/tags)
