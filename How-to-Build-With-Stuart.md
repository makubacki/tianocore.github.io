# How to Build in edk2 with Stuart

EDK II packages are easy to build with set of tools called "stuart".

>💡 If you are familiar with the `build` command and would like to learn about `build` vs `stuart`, click here.

Steps are split into two categories: [(1) one-time](#one-time-steps) and [(2) regular use](#regular-use-steps).

For any step, you can click the ℹ️ icon to get more detailed information about that step. If you run into an error,
look in the detailed information to see if it is described there.

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
          <a href="https://github.com/tianocore/edk2#core-ci-build-status" target="_blank">CI Status</a> section of the repo readme file.
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
  </ul>
  <hr>
</details>

1. Clone the edk2 repo [ℹ️](How-to-Build-With-Stuart-Details.md#clone-the-edk2-repo)
   - Open a command-prompt in the directory where you would like to keep the edk2 repo
   - Clone the repo
     - Example: `git clone edk2`

2. Change into the edk2 directory [ℹ️](How-to-Build-With-Stuart-Details.md#change-into-the-edk2-directory)
   - `cd edk`

3. Create a Python virtual environment [ℹ️](How-to-Build-With-Stuart-Details.md#create-a-python-virtual-environment)
    - Note that the steps differ between Linux and Windows.
      - <details>
        <summary>Linux Instructions</summary>
        <code>python -m venv .venv</code>
        <br>
        <code>source .venv/bin/activate</code>
        </details>
      - <details>
        <summary>Windows Instructions</summary>
        <code>py -m venv .venv</code>
        <br>
        <code>.venv\Scripts\activate.bat</code>
        </details>

4. Tell Git to ignore the Python virtual environment [ℹ️](How-to-Build-With-Stuart-Details.md#tell-git-to-ignore-the-python-virtual-environment)

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

The steps are split into two categories: (1) once per session and (2) before each build.

### Once Per Session Steps

These assume your command prompt is in the edk2 repository directory.

1. Activate the Python virtual environment [ℹ️](How-to-Build-With-Stuart-Details.md#activate-the-virtual-environment)
    - Linux
      - `source .venv/bin/activate`
    - Windows
      - `.venv\Scripts\activate.bat`

2. Update Python PIP modules [ℹ️](How-to-Build-With-Stuart-Details.md#update-pip-modules)
     - `pip install -r pip-requirements.txt --upgrade`

3. Get updated code dependencies [ℹ️](How-to-Build-With-Stuart-Details.md#get-updated-code-dependencies)
     - `stuart_setup -c .pytool/CISettings.py`

That's it!

Now every time you would like to build the code, you only need to run the following commands until you end this
session and return.

1. Update other dependencies (like binaries) [ℹ️](How-to-Build-With-Stuart-Details.md#update-other-dependencies)
    - `stuart_update -c .pytool/CISettings.py`

2. Run CI build (--help will give you options) [ℹ️](How-to-Build-With-Stuart-Details.md#run-ci-build)
    - `stuart_ci_build -c .pytool/CISettings.py TOOL_CHAIN_TAG=<your tag here>`

      - Common options:
        - `-p <pkg1,pkg2,pkg3>`: To build only certain packages use a comma-separated list
        - `-a <arch1,arch2,arch3>`: To run only certain architectures use a comma-separated list
        - `-t <target1,target2>`: To run only tests related to certain targets use a comma-separated list

That's it to do a basic build.

The remainder of this page contains more details about the `stuart_ci_build` and `stuart_build` commands.

---

<details>
<summary>More Advanced <code>stuart_ci_build</code> Options</summary>
<i>Todo - Talk about reports, skipping certain build points, clean build, shell config files, etc.</i>
</details>

## Examples

<details>
<summary>Example: I want to build MdeModulePkg to test a change I made there.</summary>
<hr>
<p>
The important parameter here is the <code>-p</code> parameter which specifies that <code>MdeModulePkg</code> should
be built.
</p>
<p>
The example below uses:
<ul>
<li>
The <code>TOOL_CHAIN_TAG</code> parameter to specify the build should use <code>VS2019</code> (Visual Studio 2019).
</li>
<li>
The <code>-a</code> parameter is used to specify that the <code>IA32</code> and <code>X64</code> architectures should
be built.
</li>
</ul>
</p>

<kbd>stuart_ci_build -c .pytool/CISettings.py -p MdeModulePkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019</kbd>
<hr>
</details>

<details>
<summary>Example: I want to build OvmfPkg to test a change I made there.</summary>
<hr>
<p>
OvmfPkg is considered a "platform firmware" for the
<a href="https://www.qemu.org/" target="_blank">QEMU open-source emulator</a>.

<ul>
<li>
Therefore, it provides a platform build file (see <a href="#what-is-platformbuild-py">What is PlatformBuild.py?</a>)
<ul>
<li>
Located at <a href="https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/PlatformBuild.py" target="_blank">OvmfPkg/PlatformCI/PlatformBuild.py</a>
</li>
</ul>
</li>
<li>
Because we are building a platform build file, the build command will be <code>stuart_build</code> instead of
<code>stuart_ci_build</code> to compile the code
</li>
</ul>

<kbd>stuart_build -c PlatformBuild.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019</kbd>

<p>
If you want to run CI checks such as CI plugins, you can use <code>stuart_ci_build</code> with the CI build file.
</p>

<kbd>stuart_ci_build -c .pytool/CISettings.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=VS2019</kbd>

<hr>
</details>
<details>

<summary>Example: I want to build OvmfPkg and automatically run with my firmware after build.</summary>
<hr>
<p>
OvmfPkg is considered a "platform firmware" for the
<a href="https://www.qemu.org/" target="_blank">QEMU open-source emulator</a>.

<ul>
<li>
Therefore, it provides a platform build file (see <a href="#what-is-platformbuild-py">What is PlatformBuild.py?</a>)
<ul>
<li>
Located at <a href="https://github.com/tianocore/edk2/blob/master/OvmfPkg/PlatformCI/PlatformBuild.py" target="_blank">OvmfPkg/PlatformCI/PlatformBuild.py</a>
</li>
</ul>
</li>
<li>
Because we are building a platform build file, the build command will be <code>stuart_build</code> instead of
<code>stuart_ci_build</code>
</li>
</ul>

To see what parameters are supported by this platform build file (at the time this page was written), we can pass the
<code>--help</code> argument to the <code>stuart_build</code> command:

<pre>
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
</pre>
</p>
<p>
The <code>--flashonly</code> and <code>--flashrom</code> commands are especially useful with OvmfPkg. They
automatically load QEMU with the newly built firmware.

The example below uses:
<ul>
<li>The <code>TOOL_CHAIN_TAG</code> parameter to specify that the build should use <code>GCC5</code>
to build with GCC.
</li>
<li>
The <code>-a</code> parameter is used to specify the <code>IA32</code> and <code>X64</code> architectures should be
built.
</li>
<li>
The <code>--flashrom</code> parameter is used to load the firmware in QEMU and boot QEMU after the firmware build
is completed.
</li>
</ul>
</p>

<kbd>stuart_build -c PlatformBuild.py -p OvmfPkg -a IA32,X64 TOOL_CHAIN_TAG=GCC5 --flashrom</kbd>
<hr>
</details>

<details>
<summary>Example: I want to build BaseTools.</summary>
<hr>
<a href="#what-are-basetools">BaseTools</a> has its own build script that leverages
<a href="#what-are-edk2-pytools">edk2-pytools</a> to build the BaseTools applications.
<br>
<br>
<details>
<summary>Linux (Ubuntu) Pre-Steps</summary>
<kbd>sudo apt-get update</kbd>
<br>
<kbd>sudo apt-get install gcc g++ make uuid-dev</kbd>
</details>
<p>
The file <a href="https://github.com/tianocore/edk2/blob/master/BaseTools/Edk2ToolsBuild.py" target="_blank">BaseTools/Edk2ToolsBuild.py</a>
can be called as a standalone Python script. You just need to pass the tool chain tag you would like to build with.

Example:
<kbd>python BaseTools/Edk2ToolsBuild.py -t GCC5</kbd>
</p>
<hr>
</details>

<details>
<summary>Example: I just want to check if my changes will pass all the non-compiler checks in CI.</summary>
<hr>
<p>
The <code>NO-TARGET</code> build target specifies that the actual firmware source code should not be built for any
particular target and, instead, the other parts of the CI process will be active such as the non-compiler checks
(<a href="#what-are-plugins">plugins</a>).
</p>
<p>
In the following example, the CI plugins will be run against all packages supported by the
<a href="#what-is-ci-settings-py">CISettings.py</a> file.
</p>
<kbd>stuart_ci_build -c .pytool/CISettings.py -t NO-TARGET</kbd>
<p>
The CI checks could be run against a single package (or a selection of packages) by passing the package names to
with the <code>-p</code> parameter.
</p>
<kbd>stuart_ci_build -c .pytool/CISettings.py -p MdePkg,UefiCpuPkg -t NO-TARGET</kbd>
<hr>
</details>

<details>
<summary>Example: I want to fix all the spelling errors in my package. How do I just run the spell
         check plugin?</summary>
<hr>
<p>
Plugins are automatically discovered in the workspace by stuart.
</p>
<p>The easiest way to have stuart only one run plugin if many others are present (as is the case in edk2) is to simply
delete the other plugin directories so they are not discovered. You can then test with the remaining plugins and then
use git to restore the deleted plugin directories back when done testing.
</p>
<p>
For example, to only test with the <code>SpellCheck</code> plugin, delete every other plugin folder from
<a href="https://github.com/tianocore/edk2/tree/master/.pytool/Plugin" target="_blank">.pytool/Plugin</a> in your
workspace.
</p>
<p>
Run the command to only perform CI checks:
<br>
<kbd>stuart_ci_build -c .pytool/CISettings.py -t NO-TARGET</kbd>
</p>
<p>
When done, restore the other plugin directories:
<br>
<kbd>git restore .pytool/Plugin/**</kbd>
</p>
<hr>
</details>

## Common Questions

<details>
<summary id="what-is-ci">What is CI?</summary>
<i>Todo</i>
</details>

<details>
<summary id="what-are-basetools">What are BaseTools?</summary>
<i>Todo</i>
</details>

<details>
<summary id="what-are-edk2-pytools">What are edk2-pytools?</summary>
<i>Todo</i>
</details>

<details>
<summary id="what-is-ci-settings-py">What is CISettings.py?</summary>
<i>Todo</i>
</details>

<details>
<summary id="what-is-platformbuild-py">What is PlatformBuild.py?</summary>
<i>Todo</i>
</a>
</details>

<details>
<summary id="stuart-ci-build-vs-stuart-build">What is the difference between <code>stuart_ci_build</code> and <code>stuart_build</code>?</summary>
<i>Todo</i>
</details>

<details>
<summary id="what-does-stuart-ci-build-do">What does <code>stuart_ci_build</code> do exactly?</summary>
<i>Todo</i>
</details>

<details>
<summary id="how-do-i-get-more-detailed-error-info">How do I get more detailed information if an error happens?</summary>
<i>Todo</i>
</details>

<details>
<summary id="tool-chain-package-etc-available">How do I know what tool chain, package, architecture, and targets are available?</summary>
<ul>
  <li>
    <details>
    <summary id="tool-chain-tags-available">How do I know what tool chain tags are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary id="packages-available">How do I know what packages are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary id="architectures-available">How do I know what architectures are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary id="targets-available">How do I know what targets are available?</summary>
    <i>Todo</i>
    </details>
  </li>
</ul>
</details>

<details>
<summary id="what-are-plugins">What are "plugins"?</summary>
<i>Todo</i>
</details>

<details>
<summary id="where-are-plugins-stored">Where are plugins stored?</summary>
<i>Todo</i>
</details>

<details>
<summary id="how-are-plugins-configured">How are plugins configured?</summary>
<i>Todo</i>
</details>

<details>
<summary id="how-do-i-skip-a-plugin">How do I skip a certain plugin from running?</summary>
<i>Todo</i>
</details>

<details>
<summary id="how-do-skip-all-plugins">How do I skip all plugins from running?</summary>
<i>Todo</i>
</details>
