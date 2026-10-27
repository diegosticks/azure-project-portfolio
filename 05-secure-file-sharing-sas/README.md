# Project 5 — Secure File Sharing with SAS Tokens and Lifecycle Management

Shared a file with an external partner without giving them an account or making anything public: a private container, a time-limited SAS controlled by a stored access policy, and revocation by deleting the policy — plus a lifecycle rule to clean up shared files automatically.

## What was built

- Resource group **rg-gp-file-exchange**
- Storage account **stgpfilexchgcpm12** with a **private** container, **partner-drop**
- `monthly-report.txt` uploaded to the container
- Stored access policy **partner-read-policy** — Read only, roughly a one-hour window
- A SAS token generated from the policy (permissions and expiry are greyed out in the SAS form because the policy controls them)
- Lifecycle management rule **delete-shared-files** — blobs under `partner-drop/` are deleted when last modified more than 30 days ago

## How it was validated (tested in an Incognito window)

- The file's direct URL was denied: **PublicAccessNotPermitted** — the container is genuinely private.
- The SAS URL rendered the file — partner access works.
- The stored access policy was deleted, and the *same* SAS URL then failed with **AuthenticationFailed** ("SAS identifier cannot be found for specified signed identifier") — access revoked without touching the file, which remained intact.
- The lifecycle rule was confirmed in place with its Base blobs and Filter set configuration.
- All resources were deleted after the lab (cleanup confirmed). The stored access policy and resource group are gone, so the SAS used in the lab is retired regardless; as captured, the browser also truncated the SAS URLs in the screenshots, so no signature value is legible.

## What this demonstrates

- Private-by-default storage design for external file sharing
- Stored access policies as the control point for SAS tokens — one deletion revokes every token issued from the policy
- Time-boxed, read-only, least-privilege sharing links
- Lifecycle management for automatic retention cleanup

## Screenshots

### Exercise 1 — Create storage and upload the file

![Resource group rg-gp-file-exchange](images/01-resource-group.png)
*Resource group rg-gp-file-exchange.*

![Storage deployment complete](images/02.1-storage-account-deployment-complete.png)
*Storage account deployment complete.*

![Storage account overview](images/02.2-storage-account-overview.png)
*Storage account stgpfilexchgcpm12 — overview.*

![Private container partner-drop](images/03-private-container.png)
*Private container partner-drop created.*

![Inside partner-drop](images/04-inside-partner-drop.png)
*monthly-report.txt uploaded to partner-drop.*

### Exercise 2 — Create an access policy and generate a SAS link

![Policy form filled in](images/01.1-add-policy-form-filled-in.png)
*Stored access policy form — partner-read-policy, Read only.*

![Access policy saved](images/01.2-saved-access-policy-page.png)
*Access policy saved on the container.*

![SAS generated from the policy](images/02-generate-the-sas.png)
*SAS generated from the stored access policy — permissions and expiry are policy-controlled.*

### Exercise 3 — Test partner access behaviour

![Direct URL denied in Incognito](images/01-error-page-incognito-window.png)
*Direct URL in an Incognito window — access denied (PublicAccessNotPermitted).*

![SAS URL works in Incognito](images/02-success-page-incognito-window.png)
*SAS URL in an Incognito window — the report file renders.*

### Exercise 4 — Revoke partner access

![SAS confirmed working before revocation](images/01-confirm-sas-still-works.png)
*SAS link confirmed working before revocation.*

![Stored access policy deleted](images/02-delete-the-stored-access-policy.png)
*Stored access policy deleted.*

![Access revoked](images/03-verify-access-is-revoked.png)
*The same SAS URL after policy deletion — AuthenticationFailed; access revoked.*

![File still exists](images/04-confirm-the-file-still-exists.png)
*The file itself is untouched after revocation.*

### Exercise 5 — Configure lifecycle management

![Lifecycle rule — Base blobs tab](images/01.1-base-blobs-tab-before-you-move-on.png)
*Lifecycle rule — Base blobs tab (delete when last modified over 30 days ago).*

![Lifecycle rules list](images/01.2-lifecycle-management-rules.png)
*Lifecycle rules list — delete-shared-files present.*

![Rule detail — Base blobs](images/02.1-base-blobs-tab.png)
*Rule detail — Base blobs settings.*

![Rule detail — Filter set](images/02.2-filter-set-tab.png)
*Rule detail — Filter set, prefix partner-drop/.*
