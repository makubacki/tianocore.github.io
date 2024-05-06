# EDK II Development Process

First check out [[Getting Started with EDK II]] for downloading the latest EDK II development project with your build
environment.

Are you new to using git? If so, then the [[New to git]] page may be helpful.

> **Note:** Commands that you should directly type into a terminal are preceded with `>$`. Unless otherwise noted,
> these commands are OS agnostic.

## EDK II Developer Onboarding

At a high-level, getting started with code development in the EDK II repo consists of the following activities:

1. Tool Setup - Performed once per development machine.
2. Workspace Setup - Performed once per development workspace.
3. Development and Test - Performed on every code contribution.
4. Code Review and CI - Performed on every code contribution.

## Tool Setup, Workspace Setup, and Development and Test

Refer to the [How to Build with Stuart](https://github.com/tianocore/tianocore.github.io/wiki/How-to-Build-With-Stuart)
documentation. After following those instructions, you should have a workspace setup and understand how to build and
test the code.

This remainder of this page focuses source management details and how to prepare for code review.

## Managing Local Changes

1. Create and checkout a topic branch for your change.

   `>$ git checkout -b <new-dev-branch> origin/master`

2. Make changes in the working tree.

3. Break up working tree changes into independent commits that do not break *git bisect*.
   - [Commit-Partitioning](Commit-Partitioning "wikilink")

   - To stage all modifications: `>$ git add -u`
   - To add new files: `>$ git add <path-to-new-file>`
   - To have git prompt you to selectively stage changes: `>$ git add -p`

5.  Follow the commit message template given below when writing commit
    messages

    - [Commit-Message-Format](Commit-Message-Format "wikilink")

    - To commit staged changes: `$ git commit`

      - Add the `-s` parameter to automatically append your
        Signed-off-by tag to the commit message.

6.  Use the ‘PatchCheck.py’ script under ‘edk2\\BaseTools\\Scripts’
    directory to verify the commits are correctly formatted

    - To check the latest <N> changes: `$ python BaseTools/Scripts/PatchCheck.py -<N>`

      - For example, 2 changes would be: `$ python BaseTools/Scripts/PatchCheck.py -2`

    - It is strongly recommended that you run PatchCheck.py after each
      commit. You can then easily amend the commit to correct any
      issues.

7.  Get the latest changes from origin

    `$ git fetch origin`

    Note: This updates origin/master, but not your local master
    branch. (origin/master may have newer commits than master.)

8.  Rebase the topic branch onto master branch

    `$ git rebase origin/master`

9. Run the automated code formatting tool (Uncrustify) against your changes

   - [EDK-II-Code-Formatting](EDK-II-Code-Formatting "wikilink")

   - The changes must pass local CI which includes a code formatting check
     in order to be merged into the code base.

   - It is strongly recommended that you format the code after each commit.
     The code can then be easily amended with the formatted output. Some
     developers might also prefer to format frequently while writing the
     code using the plugin instructions described in the code formatting
     wiki page.

10. (Optional) Push changes to the developer's fork of the EDK II project
    repository.

    - How to create a [GitHub fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)
      - **NOTE:** A GitHub fork can also be created using the command line
        utility called [`hub`](https://github.com/github/hub/releases).  The
        `hub` usage information can be found [here](https://hub.github.com/hub.1.html).

    - Add remote to the developer's fork of the EDK II project

    `$ git remote add <developer-id> https://github.com/<developer-id>/edk2.git`

    - Push the integration branch.

    `$ git push <developer-id> <new-integration-branch>`

11. (Optional) Create a GitHub pull request from the developer's
    <new-integration-branch> to edk2/master to run CI check.

    - How to create a [GitHub pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)
      - **NOTE:** A GitHub pull request can also be created using the command
        line utility called [`hub`](https://github.com/github/hub/releases).
        The `hub` usage information can be found [here](https://hub.github.com/hub.1.html).

    - Declare that it is for CI check test in the pull request title and
      description.

    - Resolve GitHub pull request issues if it fails. Please refrence step 8
      in the below **The maintainer process for the EDK II project**

12. Create patch (serial) to the [[edk2-devel]] mailing list

    - Clean out any old patches: `$ rm *.patch`

    - Generate new patch files: `$ git format-patch -M --thread origin/master`

      - Add the `--cover-letter` parameter for long patch series. (Be
        sure to edit the cover-letter.)

      - Add the `--subject-prefix="PATCH v2"` if you are sending out a
        second version of the patch series.

    - `$ git send-email *.patch --to devel@edk2.groups.io`

    - If it is the first time to send mail to edk2 mail list, please join
      https://edk2.groups.io/g/devel and expect there will be delay because
      the mail needs manual approval from the admin (gaoliming@byosoft.com.cn
      or michael.d.kinney@intel.com) of https://edk2.groups.io/g/devel

13. Modify local commits based on the review feedbacks and repeat steps
    3 to 11

    - For the latest commit, you can use `$ git commit --amend`

    - For multiple commits use `$ git rebase -i origin/master`

    - Consult your git gurus on edk2-devel or irc channel if you have
      questions.

The maintainer process for the EDK II project
---------------------------------------------

1.  Determine if a patch has met the review requirements for the package

2.  Update the origin/master from the server

    `$ git fetch origin`

3.  Create and checkout an integration branch

    `$ git checkout -b <new-integration-branch> origin/master`

4.  Add commits on the integration branch

    `$ git am <patch-file>`

5.  Rebase commit message to include any reviewed-by or other
    attributions

    `$ git rebase -i origin/master`

    - Edit lines to have an 'r' to 'reword' the commit. This will
      allow you to add the Reviewed-by attributions.

6.  Push changes to the EDK II maintainer's fork of the EDK II project
    repository.
    - How to create a [GitHub fork](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)
      - **NOTE:** A GitHub fork can also be created using the command line
        utility called [`hub`](https://github.com/github/hub/releases).  The
        `hub` usage information can be found [here](https://hub.github.com/hub.1.html).

    - Add remote to the EDK II maintainer's fork of the EDK II project

    `$ git remote add <maintainer-id> https://github.com/<maintainer-id>/edk2.git`

    - Push the integration branch.

    `$ git push <maintainer-id> <new-integration-branch>`

7.  Create a GitHub pull request from the EDK II maintainer's
    `<new-integration-branch>`  to `edk2/master` setting the **`push`** label.
    If all checks pass, the changes are strict rebase merged.  If the
    **`push`** label is not set, then the checks are run, but no changes are
    merged.

    - How to create a [GitHub pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)
      - **NOTE:** A GitHub pull request can also be created using the command
        line utility called [`hub`](https://github.com/github/hub/releases).
        The `hub` usage information can be found [here](https://hub.github.com/hub.1.html).

    - Add the [TianoCore Bugzilla](https://bugzilla.tianocore.org/) issue
      number(s) resolved by the pull request to the pull request title.

    - If `<new-integration-branch>` is a patch series, then copy the patch #0
      summary into the pull request description.

    - If the EDK II Maintainer wants the patches from `<new-integration-branch>`
      to be merged into `edk2/master` using strict rebase, then the **`push`**
      label must be set.  By default, pull requests to `edk2/master` are
      personal builds that perform checks and provide results, but no commits
      are automatically made to `edk2/master`.

    - Email notifications for pull requests, pushes, and check status results
      are enabled by watching the EDK II repository (https://github.com/tianocore/edk2).
      How to [Watch a GitHub repository](https://help.github.com/en/github/receiving-notifications-about-activity-on-github/watching-and-unwatching-repositories)

    - The figure below shows an example creating a GitHub pull request.  The
      red box at the top shows that the edk2 repository is being watched.  It
      also shows that GitHub observed that a new EDK II Maintainer branch
      `Bug_2315_Example_GitHub_Pull_Request` was pushed and is now available to
      create a pull request against `edk2/master` by selecting the button labeled
      `Compare & pull request`.

      ![Create GitHub Pull Request](images/CreateGitHubPullRequest.png)

    - The figure below shows the title and description of the pull request as
      well as the `Labels` button on the right that is used to enable/disable
      the **`push`** label.

      ![Create GitHub Pull Request Details](images/CreateGitHubPullRequest2.png)

8.  Resolve GitHub pull request issues.  A pull request to `edk2/master` may
    fail for the conditions listed below.  If a failure is detected, then the
    EDK II Maintainer must resolve the issue(s) on their local branch (outside
    of GitHub).  If any code changes are required, then the issue(s) are handed
    back to the contributor(s) to resolve, test, and perform another round of
    reviews on the mailing list.  Once the issues are resolved and reviewed,
    the EDK II Maintainer can either open a new GitHub pull request or do a
    forced push to `<new-integration-branch>`.  If a forced push is used, then
    the pull request checks are automatically restarted.

    - A merge conflict is detected.  The pull request remains open.  The EDK II
      Maintainer may attempt to resolve the merge conflicts outside of GitHub
      by rebasing `<new-integration-branch>` with `edk2/master`.  If this is
      successful with no code changes, then a forced push to
      `<new-integration-branch>` automatically restarts the checks.  If code
      changes are required, then the patches are returned to the contributor.

    - The pull request fails the `PatchCheck.py` check.  The pull request
      remains open.  The EDK II maintainer may attempt to resolve the
      `PatchCheck.py` issues with trivial changes to the commit message or patch
      set.  A forced push to `<new-integration-branch>` can be used to
      automatically restart the checks.  If code changes are required, then the
      patches are returned to the contributor.

    - The pull request fails Windows or Ubuntu checks.  The pull request
      remains open.  The patches are returned to the contributor.  The GitHub
      pull request provides links to the Azure Pipelines results that are used
      to view test results for checks that failed.

    - The pull request submitter is not a member of the TianoCore EDK II
      Maintainers team and the **`push`** label is set.  The pull request is
      ignored and automatically closed.

9.  Update [TianoCore Bugzilla](https://bugzilla.tianocore.org/) issue(s)
    resolved by the commit(s).

    - Add a pointer to the GitHub pull request (e.g.
      https://github.com/tianocore/edk2/pull/153).

    - Add the SHA hash range pushed to `edk2/master` (e.g. Pushed as
      cc6854506c..f8dd7c7018).

    - Mark BZ issues as Resolved/Fixed.

The maintainer process for the EDK II BaseTools project
-------------------------------------------------------

[EDK II BaseTools project](https://github.com/tianocore/edk2-basetools) is a Tianocore-maintained project consisting of the python source files that make up EDK2 basetools. It provides an easy way to organize and share python code to facilitate reuse across environments, tools, and scripts. In the future, this project will be the only location of the EDK II BaseTools python source code, and the EDK II project will remove all BaseTools python source code.

Now, we are in the phase where the BaseTools python code is in both the [edk2](https://github.com/tianocore/edk2) repository and the [edk2-basetools](https://github.com/tianocore/edk2-basetools) repository. The BaseTools maintainer should follow the following steps to keep the code in sync.

1. After the patch gets reviewed, the maintainer creates a PR to edk-basetools repo, and merges it into edk2-basetools repo if the CI checks pass.
2. Wait for the new version pip module generated in pypi.org.
3. Update edk2-basetools value to the latest basetools pip module version from edk2/pip-requirements.txt file. Create a PR to the edk2 repo to trigger edk2 CI to do the packages build tests.
4. Create a pip-requirement patch and send it to community review.
5. Get the Reviewed-by from the reviewers.
6. Create a PR and merge the pip-requirement change to edk2 repository.
7. Create a PR and merge the basetools patch to edk2 repository.



Updating your master branch
---------------------------

1.  Get the latest changes from origin

    `$ git fetch origin`

2.  Change to the local master branch

    `$ git checkout master`

3.  Apply the latest server changes to you local master branch.

    `$ git rebase origin/master`

**See Also**
------------

-   [[Commit-Message-Format]]
-   [[Code-Style]]
-   [[Inclusive Language Guidelines]]
