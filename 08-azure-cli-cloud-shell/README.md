# Project 8 — Manage Azure Resources with the Azure CLI and Cloud Shell

Ran the full resource lifecycle from the command line: created, tagged, and queried resources in Azure Cloud Shell, cross-checked everything in the portal, then removed the whole group with a single command and proved it was gone.

## What was built

- Resource group **rg-gp-cli-demo** (eastus), created with the CLI
- Storage accounts **stgpclicpm10** and **stgpclicpm20**, created with the CLI (Standard_LRS)
- Tags applied from the CLI — resource group: `environment=test`, `department=it-ops`; storage accounts: `department=development` and `department=operations`
- JMESPath queries against live resources: name-and-tags table output, a filter (`[?tags.department=='development']`), and a count (`length([])` = 2)

## Key commands

```bash
# Create the resource group
az group create --name rg-gp-cli-demo --location eastus

# Create a storage account
az storage account create --name stgpclicpm10 --resource-group rg-gp-cli-demo --location eastus --sku Standard_LRS

# Apply tags
az group update --name rg-gp-cli-demo --tags environment=test department=it-ops

# Query with JMESPath — filter resources by tag
az resource list --resource-group rg-gp-cli-demo --query "[?tags.department=='development']" --output table

# Clean up — delete the group and everything in it
az group delete --name rg-gp-cli-demo --yes --no-wait
```

## How it was validated

- `az group show` reported the new group's provisioningState as **Succeeded**.
- CLI listings and the portal cross-check matched exactly — same resources, same tags.
- Cleanup was a single `az group delete --yes --no-wait`; a follow-up `az group show` returned **ResourceGroupNotFound**, confirming the group and everything in it were gone.

## What this demonstrates

- Cloud Shell as a ready-to-use, authenticated CLI environment
- Creating and inspecting resources entirely from the Azure CLI
- Tagging resources from the CLI and querying tags with JMESPath filters and functions
- CLI-to-portal verification, and single-command lifecycle cleanup

## Screenshots

### Exercise 1 — Open Cloud Shell and explore the environment

![Cloud Shell in Bash mode](images/01-cloud-shell-bash.png)
*Cloud Shell open in Bash mode.*

![Subscription verified](images/02-verify-account-subscription.png)
*az account show — Azure subscription 1 verified.*

![Exploring the help system](images/03-explore-help-system.png)
*az group create --help — the required parameters.*

![Region list](images/04-list-region.png)
*Region list — eastus selected.*

### Exercise 2 — Create and list resources with CLI commands

![Resource group created and shown](images/01-create-and-show-resource-group.png)
*az group create followed by az group show — rg-gp-cli-demo, provisioningState Succeeded.*

![First storage account](images/02-first-storage-account.png)
*stgpclicpm10 created.*

![Second storage account](images/03-second-storage-account.png)
*stgpclicpm20 created.*

![Resource list](images/04-resource-list.png)
*Full resource list for the group.*

![Resource list filtered by type](images/05-resource-list-filtered.png)
*Resource list filtered by type.*

![Storage account detail](images/06-storage-account-show.png)
*Detailed view — az storage account show.*

### Exercise 3 — Tag, query, and clean up resources

![Tags applied to the group](images/01-group-update.png)
*Tags applied to the resource group from the CLI.*

![Querying the group's tags](images/02-group-show-query-tag.png)
*JMESPath query returning the group's tags.*

![Tag added to the first account](images/03-storage1-add-tag.png)
*Tag added to stgpclicpm10 — department=development.*

![Tag added to the second account](images/04-storage2-add-tag.png)
*Tag added to stgpclicpm20 — department=operations.*

![Names and tags table](images/05-names-tags-table.png)
*JMESPath table output — resource names and tags.*

![Filtered by department tag](images/06-filtered-result-development.png)
*JMESPath filter — [?tags.department=='development'].*

![Count query](images/07-count-output.png)
*JMESPath length() query — count of 2.*

![Portal cross-check — group overview](images/08-rg-overview.png)
*Portal cross-check — resource group overview matches the CLI.*

![Portal cross-check — group tags](images/09-rgs-tag-page.png)
*Portal cross-check — the group's tags match.*

![Portal cross-check — account tags](images/10-storage-accounts-tag-page.png)
*Portal cross-check — storage account tags match.*

![Group delete](images/11-group-delete.png)
*az group delete — single-command cleanup.*

![Group not found](images/12-group-show-not-found.png)
*az group show — ResourceGroupNotFound confirms the deletion.*
