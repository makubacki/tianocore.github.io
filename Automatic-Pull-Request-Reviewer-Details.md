## Purpose

This page is meant to document some brief information about how reviewers are automatically added to pull requests in edk2 for reference in case of debug and/or updates. The average edk2 contributor does not need to know these details, reviewers should be added to pull requests automatically.

Things to be aware of:

1. This requires `Actions` to be operational in GitHub - See [GitHub Status](https://www.githubstatus.com/)
   - It is rare, but GitHub does have occasional service interruptions
2. The workflow to add reviewers must be queued and may take up to several minutes to run
   - The status can always be checked in the pull request
   - This is what a queued run for the workflow looks like

     <img width="396" alt="image" src="https://github.com/user-attachments/assets/a6ab3e65-c222-43e2-9efc-612b2845e168">

If the workflow to add reviewers runs successfully it will look like this in the PR Status Checks area. The workflow itself should usually execute in less than a minute.

<img width="601" alt="image" src="https://github.com/user-attachments/assets/547c9aa1-a019-4687-b5ce-f364a0641b43">

If the workflow fails, click "Details" link and follow instructions in the "Debug" section of this page.

## Main Components

- The GitHub workflow: `.github/workflows/request-reviews.yml`
- A helper Python script: `.github/scripts/GitHub.py`

## Overview

The GitHub workflow is recognized as a workflow due to its placement in the `.github/workflows` directory. The trigger types cause the workflow to run when:

1. The pull request is not a draft pull request
2. The pull request is in the "tianocore" organization (not a fork)
3. A pull request is opened
4. A draft pull request is marked as ready for review
5. A pull request is reopened
6. A pull request is synchronized (the PR branch is updated)

### First Time Contributors

Because this is a GitHub workflow and `edk2` currently does not run workflows for security reasons for first time contributors, a maintainer will need to click the "Approve and run" button to allow the workflow to run for first time contributors to edk2. For this reason, maintainers should have notifications set up on pull requests in edk2 and be ready to approve workflows after reviewing the pull request content. This is already the procedure today for existing workflows.

- [More Details](https://docs.github.com/actions/managing-workflow-runs/approving-workflow-runs-from-public-forks#approving-workflow-runs-on-a-pull-request-from-a-public-fork)

### How it Works

- `PR Branch`: The branch with the pull request author's changes
- `Target Branch`: The branch targeted by the pull request (`master` at this time in edk2)

The workflow determines the set of commits in the PR branch and calls `GetMaintainer.py` for each commit collecting the total set of GitHub usernames to add as reviewers. This allows `Maintainers.txt` to be used with the same results as running the script locally. The PR author will be removed from the set of reviewers if present in the set. This list of reviewers is then added to the current pull request. The workflow does not remove reviewers.

> If code in a pull request is moved to a new area where an earlier set of reviewers no longer applies, the reviewers can be removed manually. This is to ensure all relevant stakeholders are aware of ongoing changes once added to a pull request.

### A Reviewer is not a Collaborator

A GitHub user can only be added as a reviewer if they are a collaborator in the repository. This is true regardless of this workflow. If a reviewer is found that is not a collaborator, the following comment will be posted in the pull request and reviewers will not be added. The list of GitHub usernames shown in the comment is the total list of which one or more may not be collaborators.

![image](https://github.com/user-attachments/assets/c91ea771-d6fe-4d70-b2e7-66cd1b71e4cd)

An administrator will need to add the user. Email the "Tianocore Stewards" in [Maintainers.txt](https://github.com/tianocore/edk2/blob/master/Maintainers.txt) for assistance.

### Debug

If the workflow fails, click the "Details" link next to the workflow in the pull request. This will take you to the page for that run of the workflow on the pull request. Click the "Add Pull Request Reviewers" job on the left and check which step does not have a check. Click the step to drop down more information. Most of the workflow is performed in the "Add Reviewers to Pull Request" step shown below.

![image](https://github.com/user-attachments/assets/cd18dce7-56ef-4466-87d7-836c4748ae69)

Check for any errors in that step. It also shows the set of GitHub usernames for the reviewers it found during that run.