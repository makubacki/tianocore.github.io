Build a MinPlatform Board Port for Qemu.

* Difficulty: Medium
* Language: C
* Mentor: 
* Suggested by: [@nate-desimone](https://github.com/nate-desimone)

# Background
UEFI system firmware is often referred to as the "BIOS" though the correctness of calling UEFI firmware a "BIOS" has been a subject of debate. [MinPlatform](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/) is a software framework that makes it easier to build UEFI system firmware based on EDK II. MinPlatform aims to bring more modularity and more consistency to EDK II system firmware development.

While the core EDK II packages like [MdeModulePkg](https://github.com/tianocore/edk2/tree/master/MdeModulePkg) provide a great foundation for the creation of UEFI system firmware, they leave a large amount of implementation work as a motherboard specific exercise. For example, [OvmfPkg](https://github.com/tianocore/edk2/tree/master/OvmfPkg) is UEFI system firmware for Qemu that was built using a traditional EDK II design. OvmfPkg contains a large amount of code, and it has been observed that much of that code could be written in a more generic way that would be applicable to UEFI system firmware for platforms other than Qemu. The goal of [MinPlatformPkg](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg) is increase the amount of generic code available for motherboard firmware developers to reuse.

# Details
While MinPlatform itself is vendor agnostic, its primary usage thus far has been for UEFI system firmware for real motherboards. And thus far, the only motherboards supported use Intel Processors. This makes it difficult for new developers to get started, as it can be difficult and costly to find and purchase the exact motherboard that a given MinPlatform board port was designed for.

This project aims to solve that problem by creating a MinPlatform board port for Qemu. This would allow developers to run MinPlatform on top of Qemu without the need to find and buy an exact motherboard.

The output of this project would be the creation of QemuOpenBoardPkg. Like OVMF, this would run on top of Qemu, however it would use the [MinPlatform Architecture](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/) and should serve as a simple example for new developers to get started with EDK II.

At a minimum, QemuOpenBoardPkg should run on `qemu-system-x86_64` with a 32-bit PEI and a 64-bit DXE, which is the typical UEFI system firmware environment. Other system targets can also be optionally implemented for extra credit, such as `qemu-system-i386` or `qemu-system-aarch64` or `qemu-system-riscv64`.

There is an open source Qemu FSP available [here](https://github.com/universalpayload/fspsdk/tree/qemu_fsp_x64). While this FSP does not do much silicon initialization, it is a great vehicle for testing FSP integration. At present, this open source FSP has only been tested in Slimbootloader and only supports the FSP 2.0 specification. As extra credit, this project could integrate the FSP and upgrade it to FSP 2.2. QemuOpenBoardPkg can provide a test vehicle to create a fully open source example of FSP including both API mode and Dispatch mode. QemuOpenBoardPkg could also be used as a sandbox for exploring future FSP architectural upgrades like 64-bit FSP for example.

# Development Environment
Building: This project can be completed on any edk2 supported OS or toolchain. Ideally, QemuOpenBoardPkg should be compiler agnostic and be able to build with either Clang, GCC, or Visual C++ though that is not a strict requirement.

# Links for More Information
* https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/
* https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg
* https://github.com/tianocore/edk2/tree/master/OvmfPkg
* https://www.qemu.org/

# Further Discussion
Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

# See Also
[[Tasks]]