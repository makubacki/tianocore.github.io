Build a MinPlatform board port for the Raspberry Pi.

* Difficulty: Medium
* Language: C
* Mentor: 
* Suggested by: [@nate-desimone](https://github.com/nate-desimone)

# Background
UEFI system firmware is often referred to as the "BIOS" though the correctness of calling UEFI firmware a "BIOS" has been a subject of debate. [MinPlatform](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/) is a software framework that makes it easier to build UEFI system firmware based on EDK II. MinPlatform aims to bring more modularity and more consistency to EDK II system firmware development.

While the core EDK II packages like [MdeModulePkg](https://github.com/tianocore/edk2/tree/master/MdeModulePkg) provide a great foundation for the creation of UEFI system firmware, they leave a large amount of implementation work as a motherboard specific exercise. For example, [OvmfPkg](https://github.com/tianocore/edk2/tree/master/OvmfPkg) is UEFI system firmware for Qemu that was built using a traditional EDK II design. OvmfPkg contains a large amount of code, and it has been observed that much of that code could be written in a more generic way that would be applicable to UEFI system firmware for platforms other than Qemu. The goal of [MinPlatformPkg](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg) is increase the amount of generic code available for motherboard firmware developers to reuse.

# Details
This project is to add support for the Raspberry Pi to MinPlatform. The Raspberry Pi is already well supported by EDK II and is an active project within the community, however the [current implementation](https://github.com/tianocore/edk2-platforms/tree/master/Platform/RaspberryPi) is a traditional EDK II platform firmware design. Building a Raspberry Pi board port that follows the [MinPlatform Architecture](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/) would serve as an example of MinPlatform on the ARM architecture.

# Development Environment
Building: As the existing Raspberry Pi UEFI firmware is built using a GCC AARCH64 cross compiler, it is recommended to use that toolchain.

# Links for More Information
* https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/
* https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg
* https://github.com/tianocore/edk2-platforms/tree/master/Platform/RaspberryPi

# Further Discussion
Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

# See Also
[[Tasks]]