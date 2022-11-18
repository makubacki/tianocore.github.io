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
    Note that you can find the latest version of Visual Studio supported by edk2 on the
    <a href="https://github.com/tianocore/edk2#core-ci-build-status" target="_blank">CI Status</a> section of the repo readme file.
    <ul>
      <li><a href="https://aka.ms/vs/17/release/vs_BuildTools.exe" target="_blank">Visual Studio 2022 Build Tools</a>
      <li><a href="https://aka.ms/vs/16/release/vs_BuildTools.exe" target="_blank">Visual Studio 2019 Build Tools</a>
      <li><a href="https://aka.ms/vs/15/release/vs_BuildTools.exe" target="_blank">Visual Studio 2017 Build Tools</a>
    </ul>
    <br>
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
      <kbd>start /w vs_BuildTools.exe --quiet --add Microsoft.VisualStudio.Workload.VCTools;includeRecommended --noUpdateInstaller --norestart</kbd>
      </li>
    </ol>
    <br>
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
    <br>
    Click <strong>Download</strong> and complete the installation process.
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

The remainder of this page contains more details about the `stuart_ci_build` command.

---

<details>
<summary>More Advanced <code>stuart_ci_build</code> Options</summary>
<i>Todo</i>
</details>

## Examples

<details>
<summary>Example: I want to build MdeModulePkg to test a change I made there.</summary>
<i>Todo</i>
</details>

<details>
<summary>Example: I want to build OvmfPkg to test a change I made there.</summary>
<i>Todo</i>
</details>
<details>

<summary>Example: I want to build OvmfPkg and automatically run with my firmware after build.</summary>
<i>Todo</i>
</details>

<details>
<summary>Example: I want to build BaseTools.</summary>
<i>Todo</i>
</details>

<details>
<summary>Example: I just want to check if my changes will pass all the non-compiler checks in CI.</summary>
<i>Todo</i>
</details>

<details>
<summary>Example: I want to fix all the spelling errors in my package. How do I just run the spell
         check plugin?</summary>
<i>Todo</i>
</details>

## Common Questions

<details>
<summary>What is CI?</summary>
<i>Todo</i>
</details>

<details>
<summary>What is CISettings.py?</summary>
<i>Todo</i>
</details>

<details>
<summary>What does <code>stuart_ci_build</code> do exactly?</summary>
<i>Todo</i>
</details>

<details>
<summary>How do I get more detailed information if an error happens?</summary>
<i>Todo</i>
</details>

<details>
<summary>How do I know what tool chain, package, architecture, and targets are available?</summary>
<ul>
  <li>
    <details>
    <summary>How do I know what tool chain tags are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary>How do I know what packages are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary>How do I know what architectures are available?</summary>
    <i>Todo</i>
    </details>
  </li>
  <li>
    <details>
    <summary>How do I know what targets are available?</summary>
    <i>Todo</i>
    </details>
  </li>
</ul>
</details>

<details>
<summary>What are "plugins"?</summary>
<i>Todo</i>
</details>

<details>
<summary>Where are plugins stored?</summary>
<i>Todo</i>
</details>

<details>
<summary>How are plugins configured?</summary>
<i>Todo</i>
</details>

<details>
<summary>How do I skip a certain plugin from running?</summary>
<i>Todo</i>
</details>

<details>
<summary>How do I skip all plugins from running?</summary>
<i>Todo</i>
</details>
