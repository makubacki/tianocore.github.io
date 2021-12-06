## Overview
 
To promote a more inclusive and open ecosystem, TianoCore is dedicated to removing archaic terminology that holds negative connotation.
In collaboration with UEFI, we will be following the same [[Inclusive Language Implementation Guidelines|https://uefi.org/sites/default/files/resources/UEFI_Inclusive%20Language.pdf]] (as of Nov 1, 2021) as stated on [[UEFI.org|https://uefi.org/]].

In our plan below, we have steps for dealing with both "_non-legacy_" and "_legacy_".  For these terms we define "_non-legacy_" as UEFI BIOS/specifications and beyond, where as "_legacy_" is BIOS/specifications that predates UEFI.  There are references to legacy specifications not controlled by the TianoCore Community, and they may not follow these guidelines. In order to preserve compatibility for code that reads on legacy specifications, particularly where that specification is no longer under maintenance or development, language in this specification may appear out of sync with the guidelines. In these cases, the Community will work with other standards development bodies to eliminate such examples over time. In the meanwhile, by acknowledging and calling attention to this issue the hope is to promote discussion and action towards more complete use of Inclusive Language reflective of the diverse and innovative population of the technical community that works on standards.

 
 ## Plan
    
1. Announcement of intent, and all check-ins from here onwards will need to abide by Inclusive Language Implementation Guidelines  
2. Scrubbing of all comments, documentation, and Wiki pages  
3. Scrubbing of all non-legacy code  
4. Working with UEFI to scrub legacy code 
    
    
 ## Implementation Guidelines
    
 ### Master/Slave to not be used together nor alone.
 Alternatives:
 Master | Slave
 -------|-------
 Main | Secondary, Subordinate
 Primary | Secondary, Replica
 Host | Target
 Leader | Follower
 Orchestrator | Worker
 Initiator | Responder
 
 Or similar descriptive terminology
    
 ### Blacklist/Whitelist to not be used together nor alone.
 Alternatives:
 Blacklist | Whitelist
 ----------|----------
 Blocklist | Passlist
 Denylist | Allowlist
 Refused, Denied | Permitted
 
 Or similar descriptive terminology