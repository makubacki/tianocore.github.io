Disclaimer
==========

**NOTE**: This document describes a deprecated process of contributing changes using
patches on a mailing list. Information in this document may still be helpful
for setting up your git configuration, but the latest process for contributing
changes via pull requests is available in
[EDK II Development Process](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Development-Process).

---

This is a quick and dirty, simplified and slightly modified description
of my own edk2 workflow with git. It expressly defers to the [EDK II
Development Process
article](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Development-Process)
on the TianoCore wiki. It doesn't try to be generic, focuses only on
GNU/Linux (that's what I use). It will not go into many details about
git; if you are interested, you'll have to research those concepts on
the web yourself.

Also, this is very specific to edk2. Other projects have different
workflows.

Contributor workflow
====================

1.   <a name="contrib-01" href="#contrib-01">&sect;</a>
     Create an account on GitHub.

2.   <a name="contrib-02" href="#contrib-02">&sect;</a>
     Enable SSH authentication for your account.

     https://help.github.com/articles/generating-an-ssh-key/

     When completing this step, you should end up with a new keypair
     under `~/.ssh/`, for example:

     ```
     id_rsa_for_github
     id_rsa_for_github.pub
     ```

     and the following stanza in your `~/.ssh/config`:

     ```
     Host github.com
       User          git
       IdentityFile  ~/.ssh/id_rsa_for_github
     ```

3.   <a name="contrib-03" href="#contrib-03">&sect;</a>
     Fork the following repository on GitHub into your own GitHub
     account, using the GitHub web GUI:

       https://github.com/tianocore/edk2/

     (My personal fork is at https://github.com/lersek/edk2/)

4.   <a name="contrib-04" href="#contrib-04">&sect;</a>
     Clone the official edk2 repository to your local computer:

     ```
     cd some-appropriate-directory
     git clone https://github.com/tianocore/edk2.git
     ```

5.   <a name="contrib-05" href="#contrib-05">&sect;</a>
     Implement the following git settings for your local clone, i.e.,
     while standing in your local edk2 directory (these steps don't need
     customization):

     ```
     git config am.keepcr              true
     git config am.signoff             true
     git config cherry-pick.signoff    true
     git config color.diff             true
     git config color.grep             auto
     git config commit.signoff         true
     git config core.abbrev            12
     git config core.pager             cat
     git config core.whitespace        cr-at-eol
     git config diff.algorithm         patience
     git config diff.ini.xfuncname     '^\[[A-Za-z0-9_., ]+]'
     git config diff.renames           copies
     git config format.coverletter     true
     git config format.numbered        true
     git config format.signoff         false
     git config notes.rewriteRef       refs/notes/commits
     git config sendemail.chainreplyto false
     git config sendemail.thread       true
     git config sendemail.to           devel@edk2.groups.io
     ```

6.   <a name="contrib-06" href="#contrib-06">&sect;</a>
     Also implement the following -- they need customization:

     ```
     git config sendemail.smtpserver FQDN_OF_YOUR_LOCAL_SMTP_SERVER
     git config user.email           "Your Email Address"
     git config user.name            "Your Name"
     ```

7.   <a name="contrib-07" href="#contrib-07">&sect;</a>
     Create a file called `tianocore.template` somewhere outside your
     edk2 clone, with the following contents. Note that the last line
     requires customization.

     ```
     [empty line]
     [empty line]
     Signed-off-by: Your Name <Your Email Address>
     ```

8.   <a name="contrib-08" href="#contrib-08">&sect;</a>
     Standing in your edk2 clone, implement the following setting
     (requires customization):

     ```
     git config commit.template                   \
       FULL_PATHNAME_OF_FILE_CREATED_IN_LAST_STEP
     ```

9.   <a name="contrib-09" href="#contrib-09">&sect;</a>
     Open the file

     ```
     .git/info/attributes
     ```

     (create it if it doesn't exist), and add the following contents:

     ```
     *.efi     -diff
     *.EFI     -diff
     *.bin     -diff
     *.BIN     -diff
     *.raw     -diff
     *.RAW     -diff
     *.bmp     -diff
     *.BMP     -diff
     *.dec     diff=ini
     *.dsc     diff=ini
     *.dsc.inc diff=ini
     *.fdf     diff=ini
     *.fdf.inc diff=ini
     *.inf     diff=ini
     ```

10.  <a name="contrib-10" href="#contrib-10">&sect;</a>
     Create a file called `edk2.diff.order` somewhere outside your local
     clone, with the following contents:

     ```
     *.dec
     *.dsc.inc
     *.dsc
     *.fdf
     *.inf
     *.h
     *.vfr
     *.c
     ```

     From git version 1.9.0 onwards, this can be configured permanently
     for the current repository with
     ```
     git config --add diff.orderFile <full path to edk2.diff.order>
     ```
     If using an older version of git, you need to pass this path in
     manually when generating patches.

11.  <a name="contrib-11" href="#contrib-11">&sect;</a>
     Add your own fork of edk2 that lives on GitHub as a *remote* to
     your local clone:

     ```
     git remote add -f --no-tags              \
       YOUR_GITHUB_ID                         \
       git@github.com:YOUR_GITHUB_ID/edk2.git
     ```

12.  <a name="contrib-12" href="#contrib-12">&sect;</a>
     At this point you are ready to start developing. Refresh your local
     master branch from the upstream master branch:

     ```
     git checkout master
     git pull
     ```

     The first command is extremely important. You should only run `git
     pull` while you are standing on your local master branch that
     tracks (and never diverges from) the upstream master branch.

     These commands will fetch any new commits from upstream master, and
     fast-forward your local tracking branch to the new HEAD.

13.  <a name="contrib-13" href="#contrib-13">&sect;</a>
     Create and check out a topic branch for the feature or bugfix that
     you would like to work on. The topic branch name requires
     customization of course.

     ```
     git checkout -b implement_foo_for_bar_v1 master
     ```

14.  <a name="contrib-14" href="#contrib-14">&sect;</a>
     Make sure you have the build environment set up:

     ```
     source edksetup.sh
     make -C "$EDK_TOOLS_PATH"
     ```

15.  <a name="contrib-15" href="#contrib-15">&sect;</a>
     Implement the next atomic, logical step in your feature or bugfix.
     Test that it builds and works. You should not cross module (driver,
     library class, library instance) boundaries within a single patch,
     if possible.

16.  <a name="contrib-16" href="#contrib-16">&sect;</a>
     Add your changes gradually to the staging area of git (it is called
     the "index"):

     ```
     git add -p
     ```

     This command will ask you interactively about staging each separate
     hunk, for files that git already tracks. In order to stage the
     addition of a new file, use

     ```
     git add pathname
     ```

     Finally, for staging the removal of a file that git has been
     tracking, issue

     ```
     git rm pathname
     ```

17.  <a name="contrib-17" href="#contrib-17">&sect;</a>
     When done, you can run

     ```
     git status
     ```

     This will list the files with staged and unstaged changes. You can
     show the diff that is staged for commit:

     ```
     git diff --staged
     ```

     and also the diff that is not staged yet:

     ```
     git diff
     ```

18.  <a name="contrib-18" href="#contrib-18">&sect;</a>
     If you are happy with the staged changes, run:

     ```
     git commit
     ```

     This will commit the staged changes to your local branch called
     `implement_foo_for_bar_v1`. You created this branch in [step
     13](#contrib-13).

     Before the commit occurs, git will fire up your preferred editor
     (from the `EDITOR` environment variable) for you to edit the commit
     message. The commit message will be primed from the template
     created in [step 7](#contrib-07) and configured in [step
     8](#contrib-08).

     Above the template, you should add:

     - a subject line, no longer than 74-76 characters, consisting of

       ```
       PackageName: ModuleName: summary of changes
       ```

     - an empty line

     - one or more paragraphs that describe the changes in the patch. No
       line should be longer than 74 characters.

     - an empty line

     - One or more tags directly above the `Signed-off-by` line
       (which comes from the template) that CC the package maintainers
       that are relevant for this specific patch. Consult the
       `Maintainers.txt` file. For example, if you wrote a patch for
       OvmfPkg, add:

       ```
       Cc: Jordan Justen <jordan.l.justen@intel.com>
       Cc: Laszlo Ersek <lersek@redhat.com>
       Cc: Ard Biesheuvel <ard.biesheuvel@linaro.org>
       Cc: Anthony Perard <anthony.perard@citrix.com>
       Cc: Julien Grall <julien.grall@linaro.org>
       ```

19.  <a name="contrib-19" href="#contrib-19">&sect;</a>
     When you have committed the patch, it is best to confirm it adheres
     to the edk2 coding style. Run:

     ```
     python BaseTools/Scripts/PatchCheck.py -1
     ```

20.  <a name="contrib-20" href="#contrib-20">&sect;</a>
     If the command in [step 19](#contrib-19) reports problems, modify
     the source code accordingly, then go back to [step 16](#contrib-16)
     and continue from there. However, as a small but important change
     for [step 18](#contrib-18), run `git commit` with the `--amend`
     option:

     ```
     git commit --amend
     ```

     This will *squash* your fixups into the last commit, and it will
     also let you re-edit the commit message (the `PatchCheck.py` script
     can also find problems with the commit message format).

     Re-run [step 19](#contrib-19) as well, to see if your patch is now
     solid.

21.  <a name="contrib-21" href="#contrib-21">&sect;</a>
     Write your next patch. That is, repeat this procedure (goto [step
     15](#contrib-15)) until you are happy with the series -- *each*
     single one of your patches builds and runs (implementing the next
     atomic, logical step in the bugfix or feature), and at the last
     patch, the feature / bugfix is complete.

22.  <a name="contrib-22" href="#contrib-22">&sect;</a>
     It is now time to publish your changes for review.

     (At this point, at the latest, it is important to review your full
     series using a git GUI; for example `gitk`. Practically, any time
     you are in doubt, and especially before publishing patches, run
     `git status` and `gitk` (or another GUI tool) and go over your
     series.)

     First, we'll push your local branch to your personal repository on
     GitHub, under the same branch name.

     ```
     git push YOUR_GITHUB_ID master implement_foo_for_bar_v1
     ```

     This command will connect to github using your remote configuration
     added in [step 11](#contrib-11), employing the SSH authentication
     configured in [step 2](#contrib-02).

     It will update the master branch in your personal repo on GitHub to
     your local master branch (which in turn follows the official master
     branch, see [step 12](#contrib-12)).

     It will also push the topic branch you created under [step
     13](#contrib-13).

23.  <a name="contrib-23" href="#contrib-23">&sect;</a>
     Now we'll format the patches as email messages, and send them to
     the list. Standing in the root of your edk2 directory, run the
     following (note that the `-O` option needs customization: please
     update the pathname to the file created in [step 10](#contrib-10)):

     ```
     rm -f -- *.patch

     git format-patch                               \
       --notes                                      \
       -O"/fully/qualified/path/to/edk2.diff.order" \
       --subject-prefix="PATCH v1"                  \
       --stat=1000                                  \
       --stat-graph-width=20                        \
       master..implement_foo_for_bar_v1
     ```

     This command will generate an email file for each one of your
     commits. The patch files will be numbered. The file numbered 0000
     is a *cover letter*, which you should edit in-place:

     - in the `Subject:` line, summarize the goal of your series,

     - in the message body, describe the changes on a broad level,

     - *reference*, by complete URL, the `implement_foo_for_bar_v1`
       branch in your personal GitHub repo -- the one that you pushed in
       [step 22](#contrib-22),

     - finally, add *all* of the `Cc:` tags to the cover letter that you
       used across all of the patches. This will ensure that even if a
       maintainer is involved in reviewing one or two of your patches
       across the series, he or she will get a copy of your cover
       letter, which outlines the full feature or bugfix.

     If `diff.orderFile` has been configured as described
     <a href="#contrib-10">above</a>, the -O parameter can be left out.

24.  <a name="contrib-24" href="#contrib-24">&sect;</a>
     Time to mail-bomb the list! Do the following:

     ```
     git send-email               \
       --suppress-cc=author       \
       --suppress-cc=self         \
       --suppress-cc=cc           \
       --suppress-cc=sob          \
       --suppress-cc=misc-by      \
       --transfer-encoding=base64 \
       *.patch
     ```

     (On Windows, you may have to move the `*.patch` files to a
     dedicated temporary directory, and specify that directory on the
     command line, in place of the `*.patch` glob.)

     This command might ask you about sending the messages in response
     to another message (identified by `Message-Id`). Just press Enter
     without entering anything.

     You might want to run the command first with the `--dry-run`
     parameter prepended.

     The messages will be posted to the list only, and to the
     maintainers that you CC'd explicitly in the commit messages.

     Once the messages are sent, you can remove the local patch files
     with:

     ```
     rm -f -- *.patch
     ```

25.  <a name="contrib-25" href="#contrib-25">&sect;</a>
     On the list, you will get feedback. In the optimal case, each patch
     will get a `Reviewed-by` tag (or an `Acked-by` tag) from at least
     one maintainer that is responsible for the package being touched by
     that patch. If you are lucky, you will also get `Tested-by` tags
     from some of them.

     Once all tags are in place, one of the maintainers will pick up the
     entire series, update the commit messages to include the above tags
     (`Reviewed-by`, `Acked-by`, `Tested-by`) at the bottom, and commit
     and push your patches to upstream master. If this happens, pop the
     champagne, and goto [step 12](#contrib-12).

26.  <a name="contrib-26" href="#contrib-26">&sect;</a>
     More frequently though, you will get requests for changes for
     *some* of your patches, while *others* of your patches will be
     fine, and garner `Reviewed-by`, `Acked-by`, and `Tested-by` tags.
     What you need to do in this case is:

     - create the next version of your local branch
     - pick up the tags that you got on the list
     - implement the requested changes
     - mark the v2 changes on each patch outside of the commit message
     - push the next version to your personal repo again
     - post the next version to the list

 In the following steps, we'll go through each of these in more detail.

27.  <a name="contrib-27" href="#contrib-27">&sect;</a>
     Create the next version of your local branch. Run the following
     commands in your edk2 tree:

     ```
     git checkout master
     git pull

     git checkout                  \
       -b implement_foo_for_bar_v2 \
       implement_foo_for_bar_v1

     git rebase master implement_foo_for_bar_v2
     ```

     These commands do the following: first they refresh (fast forward)
     your local master branch to the current upstream master.

     Then they fork version 2 of your topic branch off of version 1 of
     the same. Finally version 2 of the topic branch is rebased,
     non-interactively, on top of the refreshed master branch.

     The upshot is that at this point, you have an *identical* version
     of your series on top of *refreshed* master, under a name that
     states *v2*, without touching the original *v1* series.

     Theoretically the last command could run into conflicts, but those
     are unlikely for a low-churn project like edk2, and if you get
     those, you should ask for help on the list. Conflict resolution is
     outside of the scope of this writeup. For now we'll assume that
     your last command completes without errors.

28.  <a name="contrib-28" href="#contrib-28">&sect;</a>
     Pick up the tags that you got on the list. Run the following
     command:

     ```
     git rebase -i master implement_foo_for_bar_v2
     ```

     This will open your `EDITOR` with a list of your patches,
     identified by commit hash and subject line, each prefixed with a
     rebase *action*. By default, the rebase action will be `pick`.

     You should carefully go through the feedback you received on the
     list for the v1 posting. (An email client that supports threading
     is a hard requirement for this.) For each v1 patch where you
     received a tag (`Reviewed-by`, `Tested-by`, `Acked-by`), *replace*
     the `pick` action with `reword`. Be sure not to modify anything
     else in the rebase action list.

     Once you modified these actions, save the file, and quit the
     editor. Git will now rebase your patches again (to the same
     refreshed local master branch), but now it will also stop at each
     patch that you marked `reword`, and will let you edit the commit
     message for the patch. This is when you append the tags from the
     mailing list feedback to the very end of the commit message,
     underneath your own `Signed-off-by` tag. Save the updated commit
     message and quit the editor; git will continue the rebase.

     *Important*: when you append the `Reviewed-by`, `Tested-by`,
     `Acked-by` tags from the mailing list feedback to the very end of a
     given commit message, *never retype* those tags. *Always* cut and
     paste them with the clipboard instead. You have to treat those tags
     as opaque.

     If you mess up a commit message, don't panic. There are two options
     to bail out. First, you can update the next commit message to an
     empty text file. Git rebase will then stop and expect you to issue
     further commands at the normal shell prompt. This is when you run

     ```
     git rebase --abort
     ```

     and everything will be exactly like at the end of [step
     27](#contrib-27).

     The second option if you mess up a commit message (and you notice
     too late, i.e., the rebase finishes), is just to repeat [step
     28](#contrib-28), and fix up the commit message.

     (There is a third option: the branch can be forcibly reset to a
     chronologically earlier HEAD, which you can collect from the
     reflog. But that is a very sharp tool and not recommended for now.)

     At the end of this step, you will have picked up the feedback tags
     from the list, for each affected patch individually.

29.  <a name="contrib-29" href="#contrib-29">&sect;</a>
     Implement the requested changes. For this you run again

     ```
     git rebase -i master implement_foo_for_bar_v2
     ```

     but this time, you replace the `pick` actions of the affected (= to
     be modified) patches with `edit`. My *strong* recommendation is to
     set the `edit` action for exactly one patch in the series, and let
     the rest remain `pick`. (There are cases when this is not the best
     advice, but once you get in those situations, you won't need this
     guide.)

     Okay then, git will start the rebase, and it will stop *right
     after* the patch you marked as `edit` is *committed*. Your working
     tree will be clean (no changes relative to the staging index), your
     staging index will also be clean (no changes staged relative to the
     last commit -- which is the patch you marked as `edit`).

     At this point you modify the code as necessary, and build it and
     test it. Once satisfied, you run [step 16](#contrib-16) and [step
     17](#contrib-17). After those steps, your working tree will be
     clean relative to the staging index, and the index will have all
     the necessary changes staged relative to the last commit (which you
     marked as `edit` in the rebase action list).

     Now, if you ran

     ```
     git commit
     ```

     at this point (i.e., [step 18](#contrib-18) verbatim), then git
     would *insert* the staged changes as a *separate patch* into your
     series, so *don't do that*; that's most likely not your intent.
     Instead, run

     ```
     git commit --amend
     ```

     which will squash your staged changes into the patch-to-be-edited.

     Then see [step 19](#contrib-19) and [step 20](#contrib-20).

     (I am leaving out some editing action types here, such as: dropping
     a patch entirely, inserting a new patch, reordering patches,
     squashing patches together, and especially splitting up patches
     into smaller patches, and moving hunks between patches. Those are
     completely doable, and constitute the absolute power that git has
     over subversion, but they are definitely beyond these basics.)

     Okay, your patch is fixed up now as the reviewer(s) requested; it
     builds, it runs (at the level expected at this stage into your
     series); `PatchCheck.py` is happy with it; and you have it
     committed. Time to run:

     ```
     git rebase --continue
     ```

     This will complete the rebase.

     You can repeat this step ([step 29](#contrib-29)) as many times as
     necessary. Again, I recommend to run a full rebase per each patch
     that needs an edit.

     A small caveat: if you significantly edit a patch, say, for the v3
     posting, for which you have received a `Reviewed-by` or `Tested-by`
     earlier, you are supposed to *drop* these tags, because your
     significant edits render them stale.

30.  <a name="contrib-30" href="#contrib-30">&sect;</a>
     Mark the v2 changes on each patch outside of the commit message.
     This step is not strictly required, but it is a *huge* help for
     reviewers and maintainers.

     Each time you finish a full rebase (an iteration of [step
     29](#contrib-29)), you should run your git GUI (`gitk` or anything
     else), and locate the patch (by subject) that you just edited in
     [step 29](#contrib-29).

     Grab the SHA1 commit hash of that patch, and run:

     ```
     git notes edit COMMIT_HASH_OF_THAT_PATCH
     ```

     Git will again fire up your text editor, and allow you to attach
     *notes* to the commit. The distinction between a commit message and
     commit notes is that the notes are *ephemeral*. They will be
     included in a special section of the patch email, but they will
     never be included in the commit message itself, on the upstream
     master branch. They are perfect for communicating v1 -> v2 -> v3
     changes, per patch, during the evolution of a given patch series.

     So, please format the notes as follows:

     ```
     v2:
     - frobnicate quux [Jordan]
     - perturb xizzy [Laszlo]
     ```

     No line should be longer than 72 characters in the notes, and each
     entry should preferably mark who suggested that specific change for
     the patch.

     Save the notes file and quit your editor, git will apply the
     changes. If you need to reedit the note, just repeat this step
     ([step 30](#contrib-30)).

     Very importantly, every time you complete a rebase, your notes are
     *preserved*, even if you edit the patch itself (code or commit
     message) during the rebase. This is very important for v2 -> v3
     updates, because in that case you can add the v3 section *on top*
     of v2 in the notes!

31.  <a name="contrib-31" href="#contrib-31">&sect;</a>
     Push the next version to your personal repo again.

     Practically, repeat [step 22](#contrib-22), but using the branch
     name `implement_foo_for_bar_v2`.

     (It is *very* important that you never ever modify
     `implement_foo_for_bar_v1` after you push it to your personal
     github repo. Namely, this `feature_branch_vN` kind of branch is
     supposed to reflect your vN mailing list posting precisely. Since
     your mailing list posting is read only (you cannot modify emails
     you sent), you must not modify the corresponding branches in your
     github repo either. If a new version is necessary, you'll post a
     new version, and you'll push a new branch too.)

32.  <a name="contrib-32" href="#contrib-32">&sect;</a>
     Post the next version to the list.

     In practice, repeat [step 23](#contrib-23), with the following
     modifications:

     - The subject prefix should state

       ```
       --subject-prefix="PATCH v2"
       ```

     - The commit range given should be

       ```
       master..implement_foo_for_bar_v2
       ```

     - The cover letter should reference the v2 branch pushed in
       [step 31](#contrib-31).

     - The cover letter should include or reference (with an URL to the
       mailing list archive) the cover letter of the v1 posting, and
       also summarize the v1->v2 changes.

     Then repeat [step 24](#contrib-24).

This is it, more or less, for a contributor. Nonetheless, I recommend
reading the rest even to contributors, because it will help them
understand how maintainers are supposed to operate, and how their
actions above assist maintainers in doing their work.

Maintainer workflow
===================

1.   <a name="maint-01" href="#maint-01">&sect;</a>
     You need the same settings in your edk2 clone as a contributor.
     This includes [contributor step 1](#contrib-01) through
     [contributor step 11](#contrib-11).

2.   <a name="maint-02" href="#maint-02">&sect;</a>
     You get patches to review, either by CC, or you notice them on the
     list.

     If you can immediately point out problems with (some of) the
     patches, do so. If you are pleased with (some of) the patches,
     respond with your `Reviewed-by`, per patch. (Or, well, if you like
     it all, to the cover letter.)

     If you agree with a patch, more or less, but lack the expertise to
     review it in depth (possible for patches that target a package that
     you don't maintain), respond with your `Acked-by`.

3.   <a name="maint-03" href="#maint-03">&sect;</a>
     When reviewing a v2, v3, ... posting of a series, focus on the
     changes. The contributor is expected to support you in this with:

     - Picking up your `Reviewed-by` and `Acked-by` tags from your v1
       review. You can skip re-reviewing those patches in v2, especially
       because [contributor step 29](#contrib-29) instructs the
       contributor to drop your earlier `Reviewed-by` or `Acked-by` if
       he or she reworks the patch significantly.

     - Listing the relative changes per patch, in the git-notes section.
       Refer to [contributor step 30](#contrib-30).

     - Summarizing the changes in the v2, v3, ... cover letters. Refer
       to [contributor step 32](#contrib-32).

4.   <a name="maint-04" href="#maint-04">&sect;</a>
     Assuming the series has converged (i.e., all patches have gained
     the necessary `Reviewed-by` and/or `Acked-by` tags), plus you have
     been "elected" as the lucky maintainer to apply and push the
     series, read on.

     (The following steps are also relevant if you would like to *test*
     the series, or if you would like to review each patch in the series
     against a fully up-to-date, complete codebase, with all the
     precursor patches from the series applied.)

5.   <a name="maint-05" href="#maint-05">&sect;</a>
     The first attempt at applying the contributor's series is directly
     from emails. For this, you *absolutely* need a mail user agent
     (MUA) that allows you to save patch emails *intact*.

     So save all the patch emails into a dedicated, new folder.

6.   <a name="maint-06" href="#maint-06">&sect;</a>
     Refresh your local master branch.

     ```
     git checkout master
     git pull
     ```

     Note that it is *extremely* important to switch to the master
     branch, with the checkout command above, before you run `git pull`.

7.   <a name="maint-07" href="#maint-07">&sect;</a>
     Create an application/testing/review branch, and apply the patches
     from the files you saved in [maintainer step 5](#maint-05), from
     within your MUA:

     ```
     git checkout -b REVIEW_implement_foo_for_bar_vN master

     git am dedicated_directory/*.eml
     ```

     Now, this step can genuinely fail for two reasons. The first reason
     is very obscure and I'm sharing it only for completeness.

     So the first reason is that the patch may create or delete files,
     which implies `/dev/null` filenames in the git diff hunk headers.
     Because of the `core.whitespace` setting in [contributor step
     5](#contrib-05) -- which we absolutely need due to the source files
     using CRLF line terminators in the *internal* git representation
     --, git-am might choke on those `/dev/null` lines. This depends on
     the `Content-transfer-encoding` of the email that is saved in
     [maintainer step 5](#maint-05).

     The second reason is that the master branch may have genuinely
     diverged from where it was when the contributor prepared his or her
     patches, on top of then-master. And the patches may no longer apply
     with git-am on top of current master.

     If `git am` above fails for *any reason at all*, immediately issue

     ```
     git am --abort
     ```

     and proceed to the next step, [maintainer step 8](#maint-08).
     Otherwise, if `git am` succeeds, skip forward to [maintainer step
     11](#maint-11).

8.   <a name="maint-08" href="#maint-08">&sect;</a>
     As an alternative to [maintainer step 7](#maint-07), here we'll
     grab the contributor's patches from his or her personal GitHub
     repo.

     First add his or her personal repo as a *remote* to your local
     clone (this step only needs to be done once, during all of
     the collaboration with a given contributor):

     ```
     git remote add --no-tags                           \
       HIS_OR_HER_GITHUB_ID                             \
       https://github.com/HIS_OR_HER_GITHUB_ID/edk2.git
     ```

     At this point you should of course use the repo URL that the
     contributor shared in his or her cover letter, in [contributor step
     23](#contrib-23) or -- for a v2, v3, ... post -- in [contributor
     step 32](#contrib-32).

9.   <a name="maint-09" href="#maint-09">&sect;</a>
     Fetch any new commits and branches from the contributor's repo:

     ```
     git fetch HIS_OR_HER_GITHUB_ID
     ```

10.  <a name="maint-10" href="#maint-10">&sect;</a>
     Now, set up a local, non-tracking branch off of the contributor's
     relevant remote branch. You know about the relevant branch again
     from [contributor step 23](#contrib-23) or [contributor step
     32](#contrib-32), i.e., the cover letter.

     ```
     git checkout --no-track                         \
       -b REVIEW_implement_foo_for_bar_vN            \
       HIS_OR_HER_GITHUB_ID/implement_foo_for_bar_vN
     ```

11.  <a name="maint-11" href="#maint-11">&sect;</a>
     Rebase the contributor's series -- using your local branch that you
     created either in [maintainer step 7](#maint-07) or in [maintainer
     step 10](#maint-10) -- to the local master branch (which you
     refreshed from upstream master in [maintainer step 6](#maint-06)):

     ```
     git rebase -i master REVIEW_implement_foo_for_bar_vN
     ```

     Here you should mark those patches with `reword` that have received
     `Reviewed-by`, `Acked-by`, `Tested-by` tags on the mailing list
     after the contributor's last posting. (Patches that garnered such
     tags in earlier versions are supposed to carry those tags already,
     due to [contributor step 28](#contrib-28).)

     When rewording the relevant patches, simply append the relevant
     `Reviewed-by`, `Acked-by`, `Tested-by` tags from the mailing list
     feedback. (Refer to [contributor step 28](#contrib-28).)

     Now, this rebase has a much better chance to succeed than `git am`
     in [maintainer step 7](#maint-07), for two reasons again. The first
     reason is that the problem with the `/dev/null` headers just
     doesn't exist. The second reason is that `git rebase`, *unlike*
     `git am`, knows *whence* you are rebasing, which helps it immensely
     in calculating conflict resolutions automatically.

     Nonetheless, the rebase might still fail, if meanwhile there have
     been intrusive / conflicting changes on the upstream master branch.
     If that happens, you can try to resolve the conflicts yourself, or
     you can ask the contributor to rebase his or her work on current
     upstream master, and to post it as the next version.

12.  <a name="maint-12" href="#maint-12">&sect;</a>
     Okay, now you have the contributor's patches on top of your local
     master branch, with all the tags added from the mailing list. Time
     to build-test it! If the build fails, report it to the list, and
     ask the contributor for a new version.

     (The OCD variant of this step is to build-test the contributor's
     series at *each* constituting patch, to enforce bisectability.)

13.  <a name="maint-13" href="#maint-13">&sect;</a>
     Okay, the build test passes! Maybe you want to runtime test it as
     well. If you do, and it works, you can respond with a `Tested-by`
     to the entire series on the list, *and* immediately add your own
     `Tested-by` to the patches as well. Employ [maintainer step
     11](#maint-11) accordingly.

14.  <a name="maint-14" href="#maint-14">&sect;</a>
     Time to push the patches to upstream master. Take a big breath :)

     The steps below *attempt* to push the commits from your local
     `REVIEW_implement_foo_for_bar_vN` branch -- which is based off of
     your local master branch -- to the main github repo using a GitHub
     pull request, *and* move the upstream master branch forward to the
     final commit among those.

     If it succeeds, you deserve an alcoholic (or non-alcoholic) drink
     of your choice, you're done.

     If a merge conflict is detected, then the reason is that *another maintainer*
     executed these steps in parallel, and moved forward the upstream master
     branch *after* your [maintainer step 6](#maint-06), but before your
     [maintainer step 14](#maint-14). If github accepted your pull request in
     this case, that may cause the *other* maintainer's pull request to fail.

     Follow steps 7-9 from [The maintainer process for the EDK II project](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Development-Process#the-maintainer-process-for-the-edk-ii-project)
     substituting `<new-integration-branch>` with `REVIEW_implement_foo_for_bar_vN`.

15.  <a name="maint-15" href="#maint-15">&sect;</a>
     Repeat the following steps:

     - [maintainer step 6](#maint-06) -- Refresh your local master
       branch.

       *Do not forget* the `git checkout master` part in that step!

     - [maintainer step 11](#maint-11) -- Rebase the contributor's
       series.

       No changes should be necessary, but if conflicts are found, those
       are due to the fresh commits pushed by the other maintainer,
       mentioned in [maintainer step 14](#maint-14). The possible
       remedies are discussed in [maintainer step 11](#maint-11) -- fix
       up the conflicts yourself, or ask the contributor to rebase and
       post a new version.

     - [maintainer step 12](#maint-12) through [maintainer step
       14](#maint-14) -- rebuild, optionally retest, try pushing again.
