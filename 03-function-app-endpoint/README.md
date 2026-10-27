# Project 3 — HTTP Endpoint with an Azure Function App

Built a serverless HTTP endpoint with Azure Functions, then tightened it from anonymous access to key-based authorization and proved both states.

## What was built

- Resource group **rg-gp-functions-endpoint**
- Function App **func-gp-endpoint-cpm20** — Flex Consumption plan, Node.js 24 LTS, East US, with Application Insights connected
- **GetStatus** HTTP-trigger function, created and deployed from Azure Cloud Shell (`func azure functionapp publish`)
- Authorization level changed from **anonymous** to **function** (key required)

One practical note: Flex Consumption is not available on a free-trial subscription, so the subscription was upgraded to Pay-As-You-Go (free Basic support plan kept) before the Function App could be created.

## How it was validated

- The endpoint first responded in a regular and an Incognito browser window under anonymous access.
- After the authorization change and redeploy, the same URL without the function key returned **401 Unauthorized**; with the key it returned **200**.
- HTTP 200 invocations were confirmed in the Application Insights logs.
- All resources were deleted after the lab (cleanup confirmed). The Function App no longer exists, and the function key shown during the lab is redacted in the screenshot in this folder.

## What this demonstrates

- Creating a Function App on the Flex Consumption plan
- Developing and deploying an HTTP-trigger function from Cloud Shell
- Function-level authorization and how the `code` key changes endpoint behaviour
- Verifying executions in Application Insights

## Screenshots

### Exercise 1 — Create the Function App

![Resource group created](images/01-resource-group-created.png)
*Resource group rg-gp-functions-endpoint created.*

![Monitoring tab — Application Insights](images/02.1-monitoring-tab.png)
*Function App creation — monitoring tab, Application Insights connected.*

![Review page](images/02.2-summary-page.png)
*Review page before deployment.*

![Function App overview](images/02.3-function-app-overview-page.png)
*Function App func-gp-endpoint-cpm20 — overview.*

### Exercise 2 — Create an HTTP-trigger function

![Cloud Shell session](images/01-cloud-shell.png)
*Azure Cloud Shell session.*

![Function project created](images/02-create-the-function-project.png)
*Function project scaffolded in Cloud Shell.*

![Function deployed](images/03-deploy-the-function.png)
*GetStatus function deployed with func publish.*

### Exercise 3 — Test the endpoint and review logs

![Endpoint test in the browser](images/01.1-testing-the-endpoint-in-a-browser.png)
*Endpoint test in the browser (anonymous access).*

![Endpoint test in Incognito](images/01.2-testing-the-endpoint-in-an-incognito-window.png)
*Endpoint test in an Incognito window.*

![Function verified in the portal](images/02-verify-in%20the-portal.png)
*Function verified in the portal.*

![Monitoring view](images/03-verify-monitoring.png)
*Monitoring view for the Function App.*

![authLevel in the function code](images/04.1-restrict-access-grep-output-showing-authlevel.png)
*The authLevel setting located in the function code (grep output).*

![Redeploy after authorization change](images/04.2-redeploy-publish-output.png)
*Redeploy after switching authorization to function level.*

![401 without the key](images/05.1-401-unauthorized.png)
*Request without the function key — 401 Unauthorized.*

![200 with the key](images/05.2-code-added.png)
*Request with the function key — 200 response. The key value is redacted in this copy.*

![Invocation logs](images/06-logs.png)
*Invocation logs in Application Insights.*
