# How to report a Security Issue

**NOTE**
The latest tracking and update of security issues for EDKII can be found at (https://github.com/tianocore/tianocore.github.io/wiki/GHSA-GitHub-Security-Advisories-Proceess). The below process and details are for the former process that used bugzilla. This content page will be maintained until all of the bugzilla based items under the security purview are closed.

At present the repository tracked by tianocore infosec includes the main edkII repository (https://github.com/tianocore/edk2). For issues found in repos like (https://github.com/tianocore/edk2-platforms) recommend reaching out to the respective component who is named in the subdirectory - you can find a list of relevant companies in this domain at (https://uefi.org/security), for example.

Also, it is encouraged to attach a patch that mitigates the issues with the bug report, if possible.

# How Security Issues are Evaluated

When a Tianocore Security Issue is entered, the issue is evaluated by the **infosec** group to determine if the issue is a security issue or not.  If it is not deemed to be a security issue, then the issue is converted to a standard issue and follows the normal issue resolution process.   If the issue is confirmed to be a security issue, then the priority, severity, and impact of the issue is assessed by the **infosec** group.  Discussions, resolution, and patches are completed within Bugzilla.  A date for public disclose is determined, and on that date the issue is made public and added to the list of Security Advisories.

When reporting an issue, the outputs of tools are not sufficient. Although automation tools and fuzzers and scanners are common trade practice in the security community, they often produce many results for further investigation and can yield many false positives. Reports from automated tools or scans must include additional analysis to demonstrate the exploitability of the vulnerability. As such, edkII infosec will not accept issues without this exploitability insight.

If you are interested in being involved in the evaluation of Tianocore Security Issues, then please send an email request to join the Tianocore Bugzilla infosec group to the Tianocore Community Manager or one of the Tianocore Stewards.

**NOTE**: Never send any details related to a security issue in email.

Also, tianocore infosec team members should only share details of unmitigated issues in the infosec-tagged Bugzilla entries. Any sharing of unmitigated issues on un-encrypted email or open source prior to embargo expiry may lead to removal from the infosec group.

Now that tianocore is a CNA https://cve.mitre.org/cve/cna.html, namely https://www.cvedetails.com/product/64326/Tianocore-Edk2.html?vendor_id=19679, CVE issuance will be a “Must” for tianocore content and “May” for downstream derivatives of tianocore (open or closed). We request that the reporter perform the initial CVSS calculation.  Recommend using https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:L/I:H/A:L. If reporter doesn’t wish to grade, then infosec will propose a grade and share w/ reporter prior to applying the grading.

The tianocore infosec team uses the following flow to evaluate items
![](https://github.com/jwang36/tianocore.github.io/raw/master/security/flowchart.svg?sanitize=true)


# Security Advisories

List of current EDK II Security Advisories can be found at this Gitbook : 
**[Security Advisory Log](https://tianocore-docs.github.io/SecurityAdvisory/draft/)**

List of all Third Party EDK II Security Advisories can be found at this Gitbook :
**[Third Party Security Advisory Log]( https://github.com/tianocore/tianocore.github.io/wiki/Third-Party-Security-Advisories)**


