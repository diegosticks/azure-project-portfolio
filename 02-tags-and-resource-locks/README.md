# Project 2 — Organize and Protect Resources with Tags and Resource Locks

Used tags to organize resources for filtering and reporting, then protected them with management locks and proved the locks actually block changes.

## What was built

- Resource group **rg-gp-tags-locks**
- Storage accounts **stgptagslockcpm20** and **stgptagsopscpm25**
- Tags applied at both the resource group level and the individual resource level, then used to filter the resource list
- A **Delete lock** on a storage account and a **Read-only lock** on the resource group

## How it was validated

- Tag-based filtering returned the expected development and operations accounts.
- With the Read-only lock in place, a change attempt was blocked; with the Delete lock in place, deletion was blocked (ScopeLocked).
- Locks were then removed, normal operations were restored (tag update succeeded), and the lab's `lock-test: passed` check completed.
- All resources were deleted after the lab (cleanup confirmed).

## What this demonstrates

- Applying and using tags for organization, filtering, and cost tracking
- Choosing the right lock level (Delete vs. Read-only) and the right scope
- Testing governance controls instead of assuming they work
- Lock inheritance and the operational impact of Read-only locks

## Screenshots

### Exercise 1 — Create resources and apply tags

![Resource group rg-gp-tags-locks](images/01-resource-group.png)
*Resource group rg-gp-tags-locks.*

![Deployment complete](images/02.1-deployment-complete-screen.png)
*Deployment complete for the lab resources.*

![Resource overview](images/02.2-overview-page.png)
*Resource overview page.*

![Tags at resource group level](images/03-resource-group-tags.png)
*Tags applied at the resource group level.*

![Tags on the first storage account](images/04-storage-account-tags.png)
*Tags on the first storage account.*

![Tags on the second storage account](images/05-storage2-account-tags.png)
*Tags on the second storage account.*

![Filter — development account](images/06.1-development-account-filter.png)
*Resources filtered by tag — development account.*

![Filter — operations account](images/06.2-operatiomns-account-filter.png)
*Resources filtered by tag — operations account.*

### Exercise 2 — Apply resource locks

![Delete lock on the storage account](images/01-delete-lock-on-storage-account.png)
*Delete lock applied to the storage account.*

![Read-only lock on the resource group](images/02-read-only-lock-on-resource-group.png)
*Read-only lock applied to the resource group.*

### Exercise 3 — Test lock enforcement

![Testing the Read-only lock](images/01-testing-the-read-only-lock.png)
*Read-only lock test — the change is blocked.*

![Testing the Delete lock](images/02-testing-the-delete-lock.png)
*Delete lock test — deletion blocked (ScopeLocked).*

![Locks removed](images/03-removed-the-locks.png)
*Locks removed after testing.*

![Operations restored](images/04-operations-restored-tags-added.png)
*Operations restored — a tag update succeeds once the locks are off.*
