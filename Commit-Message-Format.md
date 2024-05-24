# Commit Message Format

This page documents our source control commit message format.

This format is also documented in [Contributions.txt](https://github.com/tianocore/edk2/raw/master/Contributions.txt)
which may be available in the source tree as well.

Use this format for commit messages, and when providing the log message for a patch.

```txt
Pkg-Module: Brief-single-line-summary

Full-commit-message

Signed-off-by: Contributor Name <contributor@email.server>
```

Where:

* **Pkg-Module**: The EDK II package and optionally the module.
* **Brief-single-line-summary**: A short summary of the change.
  * The length of `Pkg-Module: Brief-single-line-summary` should be less than 72 characters.
  * Changes for CVE fixes need to append CVE number in `Brief-single-line-summary`. The format is
   `Pkg-Module: Brief-single-line-summary (CVE-Year-Number)`. Its length should be less than 92 characters.
* Blank lines should be an empty line with no whitespace.
* `Full-commit-message` is the full message describing the change. Its first line may be the bugzilla URL. It may
  include test items that have been verified.
  * Line length should be less than 76 characters when possible.
* Signatures is one or more lines with signatures. Please see the [[Commit Signature Format]] page for more information.
* The entire log message should use only standard ASCII text characters

An example would be:

```txt
Package/Module: Short one line description of change

REF:https://bugzilla.tianocore.org/show_bug.cgi?id=1000

Several lines of
description for the
change.

Signed-off-by: Contributor Name <contributor@email.server>
```

A CVE example would be:

```txt
Package/Module: Short one line description of change (CVE-2018-12180)

REF:https://bugzilla.tianocore.org/show_bug.cgi?id=2000

Several lines of
description for the
change.

Signed-off-by: Contributor Name <contributor@email.server>
```

> After moving to the pull request contribution process, the `Cc`, `Reviewed-by`, `Acked-by`, and `Tested-by` tags are
> no longer required in commit messages.

## See Also

* [[Code-Style]]
* [[Oneline Release Notes Generation]]
