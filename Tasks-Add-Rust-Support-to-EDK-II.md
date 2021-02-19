Add support for the Rust programming language to EDK II

* Difficulty: Hard
* Language: Rust, C, Assembly
* Mentor: 
* Suggested by: [@nate-desimone](https://github.com/nate-desimone)

# Details
Multiple independent implementations of Rust language support for UEFI have been created, however all attempts thus far have not been integrated with the EDK II BaseTools build system. This project would involve reconciling all the various UEFI Rust implementations and reconciling Rust's cargo packaging system with the existing EDK II LibraryClass dependency tracking system. Then, UEFI services bindings would need to be defined and implemented for Rust.

Current UEFI Rust implementations require `no_std` Rust code, largely due to the fact that the chances of implementing a full std:: for UEFI in a reasonable timeframe is about 0%. The Rust language designers somewhat assumed any given target OS would provide an implementation of libc as a pre-requisite for implementing std:: however this assumption does not hold well for UEFI. While there is a libc implementation in [AppPkg's StdLib](https://github.com/tianocore/edk2-libc/tree/master/StdLib) this implementation depends on the UEFI shell and thus cannot be used to implement UEFI drivers or DXE drivers, reducing its utility for this task.

However, most firmware development use cases don't need a full implementation of std. One can use a statement like `extern crate my_std as std;` and then implement a subset of std:: and then implement a global default allocator that uses the UEFI AllocatePool() boot service. This would enable a lot of std:: functionality like std::collections. This project should create a subset of std that at a minimum provides a global default allocator for DXE. Since PEI lacks a complete heap implementation, PEIMs implemented in Rust would need to be limited to core::

# Existing Work
[@jyao1](https://github.com/jyao1) made an initial effort to build EDK II Rust support, though it is not fully complete. His work in progress is available on edk2-staging: https://github.com/tianocore/edk2-staging/tree/edkii-rust

Unfortunately the rust-osdev [uefi-rs](https://github.com/rust-osdev/uefi-rs) implementation is released under the MPL, which is a weak copyleft license and is not compatible with TianoCore's BSD+Patent licensing. Accordingly, you will need to ***NOT*** read that code at all to make sure that that TianoCore's BSD licensing is not tainted.

# Development Environment
Building: This project should ideally support all edk2 supported OSes and toolchains. Though you can start with any edk2 supported OS or toolchain of your choice.

# Links for More Information
* https://github.com/tianocore/edk2-staging/tree/edkii-rust
* https://www.rust-lang.org/

# Further Discussion
Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

# See Also
[[Tasks]]