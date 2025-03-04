# EDK II Code First Process

The EDK II Code First Process is a process by which new features can be added
to UEFI Forum specifications after first having been designed and prototyped
in the open.

This process lets changes and the development of new features happen in the
open, without violating the UEFI forum bylaws which prevent publication of
code for in-draft features/changes.

The process does not in fact change the UEFI bylaws - the change is that the
development (of both specification and code) happens in the open. The resulting
specification update is then submitted to the appropriate working group as an
Engineering Change Request (ECR), and voted on. For the UEFI Forum, this is a
change in workflow, not a change in process.

## UEFI Forum Tracking

ECRs are tracked in a [UEFI Forum Mantis](https://mantis.uefi.org/) instance,
with access restricted to UEFI Forum Members.

## TianoCore Tracking

TianoCore enables this process by using [GitHub issues](https://github.com/features/issues)
to track specification updates, reference implementations, and any other
associated changes, such as links to content within the [TianoCore GitHub](https://github.com/tianocore)
organization, related to the "code first" change.

Code first implementation targeting the EDK II open source project are initially
held in draft pull requests within a TianoCore GitHub repository.

## Intended workflow

1. Create a new GitHub issue in the primary TianoCore repository for the change
   using the "Code First" form. Ensure that the **"Specification Draft Change"**
   section is filled in per the template in [Specification Text Template](#specification-text-template).
   - Note: The primary repository will most frequently be [edk2](https://github.com/tianocore/edk2).
   - Note: Ensure all specifications impacted by the change are selected in the form.
     - Note: A specification draft change must be included in the issue for each specification impacted.
2. Make the changes in a new branch with the prefix `GI####-<BranchName>` that
   meets the content requirements for the code first process described in this document.
    - Note: `####` in `GI####` is the GitHub issue number from *step 1*.
    - Note: `<BranchName>` is a brief description of the change.
    - Note: Code content must follow the coding style and naming conventions in
      the [Source Code](#source-code) section of this document.
3. Push the branch to your fork of the relevant repository.
4. Create a draft pull request into the default branch on the repository from the fork branch.
   - Check the "Code First" box in the PR template so the `type:code-first` label is applied to the PR.
5. Add a comment in the PR to the GitHub issue created in *step 1*.

If the change impacts repsoitories other than edk2, such as integration changes in
[edk2-platforms](https://github.com/tianocore/edk2-platforms), those changes should
be kept in a branch on a fork of the repository and the PR process described above
should be followed in that repository as well.

Any other relevant branches, issues, discussions, or forks should be linked to the issue in *step 1*.

When the change is ready for review, the PR should be marked as ready for review (taken out of draft status).

## Source Code

In order to ensure draft code does not accidentally leak into production use,
and to signify when the changeover from draft to final happens, *all* new or
modified[1] identifiers must be prefixed with the relevant `GI####` identifiers.

- [1] Modified in a non-backwards-compatible way. If, for example, a statically
      sized array is grown - this does not need to be prefixed. But a tag in a
      comment would be *highly* recommended.

### File names

New public header files require the prefix (i.e. `Gi1234MyNewProtocol.h`).
Private header files do not need the prefix.

### Contents

The tagging must follow the coding style used by each affected code base.
Examples:

| Released in spec | Draft version in tree | Comment |
| ---              | ---                   | ---     |
| `FunctionName`   | `Gi1234FunctionName`  |         |
| `HEADER_MACRO`   | `GI1234_HEADER_MACRO` |         |

For data structures or enums, any new or non-backwards-compatible structs or
fields require a prefix. As above, growing an existing array in an existing
struct requires no prefix.

| Released in spec      | Draft version in tree | Comment               |
| ---                   | ---                   | ---                   |
| `typedef SOME_STRUCT` | `GI1234_SOME_STRUCT`  | Typedef only [2]      |
| `StructField`         | `Gi1234StructField`   | In existing struct[3] |
| `typedef SOME_ENUM`   | `GI1234_SOME_ENUM`    | Typedef only [2]      |
| `EnumValue`           | `Gi1234EnumValue`     | In existing enum[3]   |

- [2] If the struct or enum definition is separate from the typedef in the public
      header, the definition does not need the prefix.
- [3] Individual fields in newly added struct or enum do not need prefix, the
      struct or enum already carried the prefix.

Variable prefixes indicating global scope (`g` or `m`) go before the `GI` prefix.

| Released in spec | Draft version in tree | Comment |
| ---              | ---                   | ---     |
| `gSomeGuid`      | `gGi1234SomeGuid`     |         |

Local identifiers, including module-global ones (`m`-prefixed) do not require a
`GI` prefix.
