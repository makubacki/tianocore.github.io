# How to Build in edk2 with Stuart

EDK II packages are easy to build with set of tools called "stuart".

>💡 If you are familiar with the `build` command and would like to learn about `build` vs `stuart`,
>   [click here](https://github.com/tianocore/tianocore.github.io/wiki/Build-Instructions#build-option-comparison).

Steps are split into two categories: [(1) one-time](#one-time-steps) and [(2) regular use](#regular-use-steps).

## One-Time Steps

If you've already completed these steps you don't need to run them again.

<details>
  <summary>Pre-requisites (Git, Python, Compiler)</summary>
  <hr>

  <ul>
  <li>
  <strong>Git - Source Control Management (SCM) Tool</strong>

  Git is the source control management tool used by this project.

  You need <code>git</code> to pull the edk2 source code onto your system, make changes in the code, and submit
  your changes back to the GitHub repository.

  <a href="https://git-scm.com/downloads" target="_blank">Git Download Page</a>
  </li>
  <li>
  <strong>Python</strong>

  Python is a programming language that many of the edk2 build tools are written in.

  You will need Python to run the edk2 build tools including <code>stuart</code>, which is written in Python.

  It is recommended you install a Python version that is equal to the version used in the
  <code>UsePythonVersion@0</code> step in this file
  <a href="https://github.com/tianocore/edk2/blob/master/.azurepipelines/templates/pr-gate-steps.yml" target="_blank">.azurepipelines/templates/pr-gate-steps.yml</a>.

  That version is constantly tested against the code in the repository.

  <a href="https://www.python.org/downloads/" target="_blank">Python Download Page</a>
  </li>
  <li>
  <strong>C Compiler</strong>

  A C compiler is needed to compile the firmware code.

  Several options are available. This is an area where direct guidance cannot be provided.

  You will need to choose a compiler supported on your host operating system and the particular firmware packages
  you are building.

  However, it is common to use:
  <ul>
    <li><a href="https://gcc.gnu.org/" target="_blank">GCC</a> on Linux</li>
    <details>
      <summary>Ubuntu GCC Installation Instructions</summary>
      <code>apt-get update && apt-get install -y build-essential git nasm wget m4 bison flex uuid-dev python unzip acpica-tools gcc-multilib</code>
    </details>
    <li><a href="https://visualstudio.microsoft.com/downloads/" target="_blank">Visual Studio</a> on Windows</li>
    <details>
      <summary>Visual Studio Installation Instructions (Windows)</summary>
      <br>
      <details>
        <summary>Visual Studio 2022 Installation Instructions</summary>
        <hr>
        <p>
          Click to download <a href="https://aka.ms/vs/17/release/vs_BuildTools.exe" target="_blank">Visual Studio 2022 Build Tools</a>
        </p>
        <ol>
          <li>
            Open an <strong>Administrator Command Prompt</strong> by right-clicking on <strong>Command Prompt</strong>
            and select <strong>Run as Administrator</strong>
          </li>
          <li>
            Change to the directory where you downloaded the <code>vs_BuildTools.exe</code> file
            (e.g. <code>C:\Downloads</code>)
          </li>
          <li>
            Enter the following command:
            <br>
            <kbd>
              start /w vs_BuildTools.exe --quiet --wait --norestart --nocache --installPath C:\BuildTools ^
              --add Microsoft.VisualStudio.Component.VC.CoreBuildTools --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 ^
              --add Microsoft.VisualStudio.Component.Windows11SDK.22000 --add Microsoft.VisualStudio.Component.VC.Tools.ARM ^
              --add Microsoft.VisualStudio.Component.VC.Tools.ARM64
            </kbd>
          </li>
        </ol>
        <hr>
    </details>
    <details>
      <summary>Visual Studio 2019 Installation Instructions</summary>
      <hr>
      <p>
        Click to download <a href="https://aka.ms/vs/16/release/vs_BuildTools.exe" target="_blank">Visual Studio 2019 Build Tools</a>
      </p>
      <ol>
        <li>
          Open an <strong>Administrator Command Prompt</strong> by right-clicking on <strong>Command Prompt</strong>
          and select <strong>Run as Administrator</strong>
        </li>
        <li>
          Change to the directory where you downloaded the <code>vs_BuildTools.exe</code> file
          (e.g. <code>C:\Downloads</code>)
        </li>
        <li>
          Enter the following command:
          <br>
          <kbd>
            start /w vs_BuildTools.exe --quiet --wait --norestart --nocache --installPath C:\BuildTools ^
            --add Microsoft.VisualStudio.Component.VC.CoreBuildTools --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 ^
            --add Microsoft.VisualStudio.Component.Windows10SDK.19041 --add Microsoft.VisualStudio.Component.VC.Tools.ARM ^
            --add Microsoft.VisualStudio.Component.VC.Tools.ARM64
          </kbd>
        </li>
      </ol>
      <hr>
    </details>
    <details>
      <summary>Visual Studio 2017 Installation Instructions</summary>
      <hr>
      <p>
        Click to download <a href="https://aka.ms/vs/15/release/vs_BuildTools.exe" target="_blank">Visual Studio 2017 Build Tools</a>
      </p>
      <ol>
        <li>
        Open an <strong>Administrator Command Prompt</strong> by right-clicking on <strong>Command Prompt</strong> and
        select <strong>Run as Administrator</strong>
        </li>
        <li>
        Change to the directory where you downloaded the <code>vs_BuildTools.exe</code> file
        (e.g. <code>C:\Downloads</code>)
        </li>
        <li>
          Enter the following command:
          <br>
          <kbd>
            start /w vs_BuildTools.exe --quiet --wait --norestart --nocache --installPath C:\BuildTools ^
            --add Microsoft.VisualStudio.Component.VC.CoreBuildTools --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 ^
            --add Microsoft.VisualStudio.Component.Windows10SDK.17763 --add Microsoft.VisualStudio.Component.VC.Tools.ARM ^
            --add Microsoft.VisualStudio.Component.VC.Tools.ARM64
          </kbd>
        </li>
      </ol>
      <hr>
    </details>
    <p>
      <ul>
        <li>
          Note: You can find the latest version of Visual Studio supported by edk2 on the
          <a href="https://github.com/tianocore/edk2#core-ci-build-status" target="_blank">CI Status</a> section of the
          repo readme file.
        </li>
        <li>
          Note: If you still run into build problems finding tools in the SDK, try installing the Windows SDK manually
          using the following instructions.
        </li>
      </ul>
    </p>
    <details>
      <summary>Optional: Install the Windows SDK manually</summary>
      <hr>
      <p>
        Download the Windows Software Development Kit (SDK) from
        <a href="https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/" target="_blank">Windows Dev Center - Windows SDK</a>
      </p>
      <p>
        Follow the default options until you reach the "<strong>Select the features you want to install</strong>" page.
      </p>
      Select the following options:
      <ul>
        <li>Windows SDK Signing Tools for Desktop Apps</li>
        <li>Windows SDK for UWP Managed Apps</li>
        <li>Windows SDK for UWP C++ Apps</li>
        <li>Windows SDK for Desktop C++ x86 Apps</li>
        <li>Windows SDK for Desktop C++ amd64 Apps</li>
        <li>Windows SDK for Desktop C++ arm Apps</li>
      </ul>
      <p>
        Click <strong>Download</strong> and complete the installation process.
      </p>
      <hr>
    </details>
  </ul>
  </li>
  <li>
  <strong>Mono (Linux)</strong>
    <p><a href="https://www.mono-project.com" target="_blank">Mono</a> needs to be installed on Linux.</p>
    <kbd>apt-get install mono-complete</kbd>
  </li>
  </ul>
  <hr>
</details>

1. Clone the edk2 repo
   - Open a command-prompt in the directory where you would like to keep the edk2 repo
   - Clone the repo
     - Example: `git clone https://github.com/tianocore/edk2.git`

2. Change into the edk2 directory
   - `cd edk`

3. Create a Python virtual environment
    - Note that the steps differ between Linux and Windows.
      - <details>
        <summary>Linux Instructions</summary>
        <code>python3 -m venv .venv</code>
        <br>
        <code>source .venv/bin/activate</code>
        </details>
      - <details>
        <summary>Windows Instructions</summary>
        <code>py -m venv .venv</code>
        <br>
        <code>.venv\Scripts\activate.bat</code>
        </details>

4. Tell Git to ignore the Python virtual environment

    The problem: Git will try to track your Python virtual environment as new code changes if it is not told to
    ignore it.

    - Open the file `.git/info/exclude`
      - `cd .git`
      - `cd info`
      - Open the `exclude` file in a text editor
      - Add the following line to the end of the file:
        - `*venv*/**`
      - Close the file
    - Note: Git will no longer try to track your Python virtual environments in this repository.

That's it!

Your terminal may now indicate that a virtual environment is active by showing `(.venv)` before the
current line.

## Regular Use Steps

These are steps you should run on a regular basis.

The steps are split into three categories: (1) once per session, (2) when dependencies are updated, and (3) before
each build.

### Once Per Session Steps

These assume your command prompt is in the edk2 repository directory.

1. Activate the Python virtual environment
    - Linux
      - `source .venv/bin/activate`
    - Windows
      - `.venv\Scripts\activate.bat`

2. Update Python PIP modules
     - `pip install -r pip-requirements.txt --upgrade`

3. Get updated code dependencies
     - `stuart_setup -c .pytool/CISettings.py`

That's it!

### When Dependencies are Updated Steps

The edk2 repo has a number of dependencies on external content. For example, it depends on git submodules, Python pip
modules, tools in the form of application binaries, etc. If the corresponding version information for these is updated
in the repo, you will need to pull the update.

The recommended steps to update dependencies are in this section.

#### Git Submodules

`git submodule update --init --recursive`

#### Python PIP Modules

`pip install -r pip-requirements.txt --upgrade`

#### Rebuild BaseTools

See the [BaseTools build example](#example-i-want-to-build-basetools).

### Before Each Build Steps

Now every time you would like to build the code, you only need to run the following commands until you end this
session and return.

1. Update other dependencies (like binaries)
    - `stuart_update -c .pytool/CISettings.py`

       > **Note:** It is recommended to specify the architecture and tool chain in the update command (see the
       > `stuart_ci_build` command below) so any binaries specific to that architecture and tool chain are
       > downloaded in this step.

       > **Note:** The binaries downloaded by this step can be very large, it may take a long time to complete.

2. Run CI build (--help will give you options)
    - `stuart_ci_build -c .pytool/CISettings.py TOOL_CHAIN_TAG=<your tag here>`

      - Common options:
        - `-p <pkg1,pkg2,pkg3>`: To build only certain packages use a comma-separated list
        - `-a <arch1,arch2,arch3>`: To run only certain architectures use a comma-separated list
        - `-t <target1,target2>`: To run only tests related to certain targets use a comma-separated list

That's it to do a basic build.

The remainder of this page contains more details about the `stuart_ci_build` and `stuart_build` commands.

---

## Examples

<details>
  <summary>Example: I want to build MdeModulePkg to test a change I made there</summary>

### Example: I want to build MdeModulePkg to test a change I made there

---

The important parameter here is the `-p` parameter which specifies that `MdeModulePkg` should be built.

The example below uses:

- The `TOOL_CHAIN_TAG` parameter to specify the build should use `VS2019` (Visual Studio 2019).
- The `-a` parameter is used to specify that the `IA32` and `X64` architectures should be built.

`stuart_ci_build -c .pytool/CISettings.py -p MdeModulePkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019`

---

</details>

<details>
  <summary>Example: I want to build OvmfPkg to test a change I made there</summary>

### Example: I want to build OvmfPkg to test a change I made there

---

OvmfPkg is considered a "platform firmware" for the [QEMU open-source emulator](https://www.qemu.org).

- Therefore, it provides a platform build file (see [What is PlatformBuild.py](#what-is-platformbuild-py)?</a>)
  - Located at [OvmfPkg/PlatformCI/PlatformBuild.py](https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/PlatformBuild.py)
- Because we are building a platform build file, the build command will be `stuart_build` instead of `stuart_ci_build`
  to compile the code

`stuart_build -c PlatformBuild.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019`

If you want to run CI checks such as CI plugins, you can use `stuart_ci_build` with the CI build file.

`stuart_ci_build -c .pytool/CISettings.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019`

---

</details>

<details>
  <summary>Example: I want to build OvmfPkg and automatically run with my firmware after build</summary>

### Example: I want to build OvmfPkg and automatically run with my firmware after build

---

OvmfPkg is considered a "platform firmware" for the [QEMU open-source emulator](https://www.qemu.org).

- Therefore, it provides a platform build file (see [What is PlatformBuild.py?](#what-is-platformbuild-py)</a>)
  - Located at [OvmfPkg/PlatformCI/PlatformBuild.py](https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/PlatformBuild.py)
- Because we are building a platform build file, the build command will be `stuart_build` instead of `stuart_ci_build`

To see what parameters are supported by this platform build file (at the time this page was written), we can pass
the `--help` argument to the `stuart_build` command:

```plaintext
    ❯ stuart_build -c PlatformBuild.py --help
    usage: stuart_build [-h] [--SKIPBUILD] [--SKIPPREBUILD] [--SKIPPOSTBUILD] [--FLASHONLY] [--FLASHROM]
                        [--UPDATECONF] [--CLEAN] [--CLEANONLY] [--OUTPUTCONFIG OUTPUTCONFIG] [-a BUILD_ARCH]
                        [--build-config BUILD_CONFIG] [--verbose]

    options:
      -h, --help            show this help message and exit
      --SKIPBUILD, --skipbuild, --SkipBuild
                            Skip the build process
      --SKIPPREBUILD, --skipprebuild, --SkipPrebuild
                            Skip prebuild process
      --SKIPPOSTBUILD, --skippostbuild, --SkipPostBuild
                            Skip postbuild process
      --FLASHONLY, --flashonly, --FlashOnly
                            Flash rom after build.
      --FLASHROM, --flashrom, --FlashRom
                            Flash rom. Rom must be built previously.
      --UPDATECONF, --updateconf, --UpdateConf
                            Update Conf. Builders Conf files will be replaced with latest template files
      --CLEAN, --clean, --CLEAN
                            Clean. Remove all old build artifacts and intermediate files
      --CLEANONLY, --cleanonly, --CleanOnly
                            Clean Only. Do clean operation and don't build just exit.
      --OUTPUTCONFIG OUTPUTCONFIG, --outputconfig OUTPUTCONFIG, --OutputConfig OUTPUTCONFIG
                            Provide shell variables in a file
      -a BUILD_ARCH, --arch BUILD_ARCH
                            Optional - CSV of architecture to build. IA32 will use IA32 for Pei & Dxe. X64 will use
                            X64 for both PEI and DXE. IA32,X64 will use IA32 for PEI and X64 for DXE. default is
                            IA32,X64
      --build-config BUILD_CONFIG
                            Provide shell variables in a file
      --verbose, --VERBOSE, -v
                            verbose

    positional arguments:
      <key>=<value>              - Set an env variable for the pre/post build process
      BLD_*_<key>=<value>        - Set a build flag for all build types
                                  (key=value will get passed to build process)
      BLD_<TARGET>_<key>=<value> - Set a build flag for build type of <target>
                                  (key=value will get passed to build process for given build type)
```

The `--flashonly` and `--flashrom` commands are especially useful with OvmfPkg. They automatically load QEMU with the
newly built firmware.

The example below uses:

- The `TOOL_CHAIN_TAG` parameter to specify that the build should use `GCC5` to build with GCC.
- The `-a` parameter is used to specify the `IA32` and `X64` architectures should be built.
- The `--flashrom` parameter is used to load the firmware in QEMU and boot QEMU after the firmware build is completed.

`stuart_build -c PlatformBuild.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=GCC5 --flashrom`

---

</details>

<details>
  <summary id="example-i-want-to-build-basetools">Example: I want to build BaseTools</summary>

### Example: I want to build BaseTools

---

[BaseTools](#what-are-basetools) has its own build script that leverages [edk2-pytools](#what-are-edk2-pytools) to
build the BaseTools applications.

  <details>
    <summary>Linux (Ubuntu) Pre-Steps</summary>

- `sudo apt update`
- `sudo apt install build-essential uuid-dev`

  </details>

The file [BaseTools/Edk2ToolsBuild.py](https://github.com/tianocore/edk2/blob/master/BaseTools/Edk2ToolsBuild.py)
can be called as a standalone Python script. You just need to pass the tool chain tag you would like to build with.

Example:
`python BaseTools/Edk2ToolsBuild.py -t GCC5`

---

</details>

<details>
  <summary>Example: I just want to check if my changes will pass all the non-compiler checks in CI</summary>

### Example: I just want to check if my changes will pass all the non-compiler checks in CI

---

The `NO-TARGET` build target specifies that the actual firmware source code should not be built for any
particular target and, instead, the other parts of the CI process will be active such as the non-compiler checks
([plugins](#what-are-plugins)).

In the following example, the CI plugins will be run against all packages supported by the
[CISettings.py](#what-is-ci-settings-py) file.

`stuart_ci_build -c .pytool/CISettings.py -t NO-TARGET`

The CI checks could be run against a single package (or a selection of packages) by passing the package names to
with the `-p` parameter.

`stuart_ci_build -c .pytool/CISettings.py -p MdePkg,UefiCpuPkg -t NO-TARGET`

---

</details>

<details>
  <summary>
    Example: I want to fix all the spelling errors in my package. How do I just run the spell check plugin?
  </summary>

### Example: I want to fix all the spelling errors in my package. How do I just run the spell check plugin?

---

Plugins are automatically discovered in the workspace by stuart.

The easiest way to have stuart only one run plugin if many others are present (as is the case in edk2) is to simply
delete the other plugin directories so they are not discovered. You can then test with the remaining plugins and
then use git to restore the deleted plugin directories back when done testing.

For example, to only test with the <code>SpellCheck</code> plugin, delete every other plugin folder from
[.pytool/Plugin](https://github.com/tianocore/edk2/tree/master/.pytool/Plugin) in your workspace.

Run the command to only perform CI checks:

`stuart_ci_build -c .pytool/CISettings.py -t NO-TARGET`

When done, restore the other plugin directories:
`git restore .pytool/Plugin/**`

---

</details>

## Common Questions

<details>
  <summary id="what-is-ci">What is CI?</summary>

### What is CI?

---

[Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration)

Continuous integration is used in edk2 to test new contributions before they are merged to the edk2 main branch.
Stuart is used within the edk2 CI process to pull build dependencies and build the code.

You can use stuart to perform the same CI checks locally that are done on the server (see the examples section).

Also see
[EDK II Continuous Integration](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Continuous-Integration).

---

</details>

<details>
  <summary id="what-are-basetools">What are BaseTools?</summary>

### What are BaseTools?

---

A collection of build related tools for edk2.

Examples:

- AutoGen
- Build
- GenSec
- GenFV
- GenFW
- GenRds

Each tool has a user manual located in [BaseTools/UserManuals](https://github.com/tianocore/edk2/tree/master/BaseTools/UserManuals).

A more complete list of BaseTools is located in the [EDK II Tools List](https://github.com/tianocore/edk2/tree/master/BaseTools/UserManuals).

---

</details>

<details>
  <summary id="what-are-edk2-pytools">What are edk2-pytools?</summary>

### What are edk2-pytools?

---

A collection of Python code for working with edk2.

- [edk2-pytool-library](https://github.com/tianocore/edk2-pytool-library) - Python
  library code that seeks to provide an easy way to organize and share edk2 related functionality to facilitate
  reuse across environments, tools, and scripts.
- [edk2-pytool-extensions](https://github.com/tianocore/edk2-pytool-extensions) - A
  Python project that consists of command line and other tools and extensions for building and maintaining an edk2
  based UEFI firmware code tree.

---

</details>

<details>
  <summary id="what-is-ci-settings-py">What is CISettings.py?</summary>

### What is CISettings.py?

---

`CISettings.py` is a common name given to a configuration file used with Stuart for CI. It is often
stored in a folder named `.pytools` in the root of a repository. So you'll likely encounter commands
like the following be used with the file:

- `stuart_ci_setup -c .pytool/CISettings.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`
- `stuart_update -c .pytool/CISettings.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`
- `stuart_ci_build -c .pytool/CISettings.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`

---

</details>

<details>
  <summary id="what-is-platformbuild-py">What is PlatformBuild.py?</summary>

### What is PlatformBuild.py?

---

`PlatformBuild.py` is a common name given to a configuration file used with Stuart for platform build.
It is often stored in the root directory of the package it builds.

For example:

- `stuart_setup -c SomePkg/PlatformBuild.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`
- `stuart_update -c SomePkg/PlatformBuild.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`
- `stuart_build -c SomePkg/PlatformBuild.py TOOL_CHAIN_TAG=PUT_TAG_VALUE_HERE`

Like Stuart CI has "CI plugins", the build process has "build plugins". These can hook into the build in
"pre-build" or "post-build".

> **Note:** Build plugins will also run during CI if a CompilerPlugin is present that builds the code.

---

</details>

<details>
  <summary id="stuart-ci-build-vs-stuart-build">
    What is the difference between stuart_ci_build and stuart_build?
  </summary>

### What is the difference between stuart_ci_build and stuart_build?

---

- `stuart_ci_build` - Runs CI plugins. By default, often runs CI on several packages at once. This
  includes all of the checks needed to consider the code ready for integration to the mainline.
- `stuart_build` - Does not run CI plugins. Builds one platform. Platforms often expose
  platform-specific parameters as defined in their `PlatformBuild.py` file.

---

</details>

<details>
  <summary id="what-does-stuart-ci-build-do">What does stuart_ci_build do exactly?</summary>

### What does stuart_ci_build do exactly?

---

The Stuart CI process is composed of "CI plugins" that get discovered in the code tree at CI time and hook into
the CI process. Some examples of CI plugins are a host-based unit test compile and execution, spell checking the
code, performing markdown lint on the code, etc. Firmware (C code) compilation is performed during CI by a compiler
CI plugin.

Each plugin reports back a pass/fail status. If any plugin fails, CI fails. However, plugins usually provide some
level of customization in a "CI package configuration file". If this file is present, it is in the root of the
package with the naming convention `PkgName.ci.yaml`. For example,
[MdePkg.ci.yaml](https://github.com/tianocore/edk2/blob/master/MdePkg/MdePkg.ci.yaml) is the CI package configuration
file for `MdePkg`. Sometimes, CI plugins will allow the plugin to be set to run in "audit mode" so the plugin will run
and report results but not fail CI if errors are found. As an example, some packages in edk2 currently use this file to
run the spell checker CI plugin in audit mode.

The two main places to look for CI settings are:

- The CISettings.py file - Usually has repo-wide CI settings
- The CI package configuration file - Has package-specific CI settings for a given package

---

</details>

<details>
  <summary id="how-do-i-get-more-detailed-error-info">
    How do I get more detailed information if an error happens?
  </summary>

### How do I get more detailed information if an error happens?

---

You can pass the `-v` argument to Stuart commands to get more detailed output.

Also, look in your `/Build` directory.


Each Stuart command produces a separate file. Open the file corresponding to the command you're using that has the
failure.

- `stuart_ci_setup` - `CISETUP.txt`
- `stuart_setup` - `SETUPLOG.txt`
- `stuart_update` - `UPDATE_LOG.txt`
- `stuart_ci_build` - `CI_BUILDLOG.txt`
- `stuart_build` - `BUILDLOG_PACKAGENAME.txt`

---

</details>

<details>
  <summary id="what-are-plugins">What are plugins?</summary>

### What are plugins?

---

The different types of plugins supported by Stuart and details about each type are available in the
[edk2-pytool-extensions Plugin Manager](https://github.com/tianocore/edk2-pytool-extensions/blob/master/docs/user/features/plugin_manager.md)
page.

---

</details>

<details>
  <summary id="how-do-i-get-lower-level-technical-details">How do I get lower-level technical details?</summary>

### How do I get lower-level technical details?

---

Start in the
[edk2-pytool-extensions User Documentation](https://github.com/tianocore/edk2-pytool-extensions/blob/master/docs/user/index.md).

---

</details>
