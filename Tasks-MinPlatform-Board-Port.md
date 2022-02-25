Build a MinPlatform board port for a new motherboard of your choice.

* Project Size: Large
* Difficulty: Medium
* Language: C
* Mentor: 
* Suggested by: [@nate-desimone](https://github.com/nate-desimone)

# Background
UEFI system firmware is often referred to as the "BIOS" though the correctness of calling UEFI firmware a "BIOS" has been a subject of debate. [MinPlatform](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/) is a software framework that makes it easier to build UEFI system firmware based on EDK II. MinPlatform aims to bring more modularity and more consistency to EDK II system firmware development.

While the core EDK II packages like [MdeModulePkg](https://github.com/tianocore/edk2/tree/master/MdeModulePkg) provide a great foundation for the creation of UEFI system firmware, they leave a large amount of implementation work as a motherboard specific exercise. For example, [OvmfPkg](https://github.com/tianocore/edk2/tree/master/OvmfPkg) is UEFI system firmware for Qemu that was built using a traditional EDK II design. OvmfPkg contains a large amount of code, and it has been observed that much of that code could be written in a more generic way that would be applicable to UEFI system firmware for platforms other than Qemu. The goal of [MinPlatformPkg](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg) is increase the amount of generic code available for motherboard firmware developers to reuse.

Thus far, most MinPlatform motherboard ports are for Intel RVPs (Reference and Validation Platform) which are generally not available for sale. The only commercially available systems with MinPlatformm board ports (at time of writing) are the [Up Xtreme](https://up-board.org/up-xtreme/) and a discontinued model of the [System76 Galago Pro](https://system76.com/guides/galp3). Supporting more motherboards would greatly benefit the community.

# Details
This project is to add support for a new motherboard to MinPlatform. The motherboard can be any motherboard of your choice.

It is ***strongly*** recommended that you choose a motherboard which utilizes a processor/SoC/chipset which is already supported by an existing MinPlatform *BoardPkg. Adding support for a new processor is a difficult task; even a highly experienced EDK II developer would not be able to complete that task within a reasonable time frame. The current list of *BoardPkgs can be found here: https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel

If you are looking for ideas for a board to use, the [UP Xtreme i11](https://up-shop.org/up-xtreme-i11-boards-series.html) would probably be a good choice. We already have support for the [original Up Xtreme board](https://up-shop.org/up-xtreme-series.html) in the WhiskeyLakeOpenBoardPkg. Support for the UP Xtreme i11 could be added to TigerLakeOpenBoardPkg.

If you don't have any specific motherboard in mind and would like help finding a good motherboard, feel free post on [edk2-devel](https://edk2.groups.io/g/devel) and we can help you pick one that would be a good development target.

# Development Environment
Building: This project can be completed on any edk2 supported OS or toolchain. Ideally, your new board port should be compiler agnostic and be able to build with either Clang, GCC, or Visual C++ though that is not a strict requirement.

# Links for More Information
* https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/
* https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg
* https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel
* https://up-shop.org/up-xtreme-i11-boards-series.html

# Further Discussion
Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

# See Also
[[Tasks]]