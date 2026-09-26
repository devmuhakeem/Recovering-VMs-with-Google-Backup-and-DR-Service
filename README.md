# Recovering VMs with Google Backup and DR Service

An advanced hands-on Google Cloud lab where I set up a backup plan template, onboarded a Compute Engine instance, and restored it as a new VM — both within the same project and across two different Google Cloud projects.

## Scenario
A fictional bank's incident response team had contained a security incident and was moving into recovery: restoring the VMs affected during the response. My job was to build and test the backup/restore workflow they'd rely on for that recovery.

## What I did

### 1. Connected to the Backup and DR management console
Logged into the Backup and DR management console and verified the management appliance's connectivity status was healthy before doing anything else.

### 2. Created a backup plan template
Built a template (`vm-backup`) with a snapshot policy set to continuous scheduling, running every 2 hours — defining both when backups happen and how they're retained.

### 3. Verified appliance service account permissions
Confirmed the backup/recovery appliance's dedicated service account already had the correct IAM role (Backup and DR Cloud Storage Operator) needed to do its job.

### 4. Onboarded a Compute Engine instance
Used the onboarding wizard to discover an existing VM (`lab-vm`) and attach the `vm-backup` template to it, triggering an actual backup job, then confirmed it completed in the Jobs monitor.

### 5. Restored the VM in the same project
Mounted the backup image as a brand new Compute Engine instance (`lab-vm-recovered`) in a different region/zone than the original — demonstrating recovery isn't limited to the exact original location.

### 6. Restored the VM to a completely different project
Granted the first project's backup appliance service account the right roles (Backup and DR Compute Engine Operator, Backup and DR Cloud Storage Operator) in a second Google Cloud project, then mounted the same backup image there as `lab-vm-project2` — proving a VM can be recovered into an entirely separate project, not just a different zone.

## Key takeaways
- A backup policy is only useful if it's actually tested — building the template is the easy part, proving you can restore from it is what matters
- Cross-project recovery requires deliberately granting the source appliance's service account access in the target project first; it's not automatic
- Being able to restore into a different region, zone, or even project entirely is what makes a backup strategy resilient to more than just "one server died" — it covers broader disaster scenarios too

## Tools
Google Cloud Backup and DR Service, Compute Engine, IAM

---
*Completed as a Google Cloud Skills Boost lab.*
