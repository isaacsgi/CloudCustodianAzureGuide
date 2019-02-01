# CloudCustodianAzureGuide
## Guide to Cloud Custodian on Azure
### Introduction
**TO-DO Why / What / How Cloud Custodian?**
Business Value
Security/Governance/Control



**TO-DO Typical use cases**

**TO-DO Customer examples**
### Architecture

!["Cloud Custodian uses simple Yaml to enforce policy"](./pictures/yml-picture.jpg "Cloud Custodian uses simple Yaml to enforce policy")

!["Cloud Custodian enables a consistent policy enforcement across clouds"](./pictures/cc-multi-cloud.jpg "Cloud Custodian enables a consistent policy enforcement across clouds")

### Component Overview

### Deployment
**To-Do: Need Azure Version**
https://cloudcustodian.io/docs/overview/deployment.html

### Process flow & How it Works
Custodian uses a flexible query language for filtering resources to a particular subset that allows for compound querying. This essentially allows you to filter for things like instances with attached Azure Blob Storage disks or stopped instances. This filtering can take into account external data sources. It also provides for resource specific actions around deletion, stopping, starting, encryption, tagging, etc.

The stateless design of Custodian greatly simplifies feature development and operations. It also provides flexibility around execution environment (local cli, vm-based server or serverless container).

When a user runs Custodian, Custodian will run the specified policy against the account and region specified by the user. Custodian will iterate over all resources defined in the policy. In the CLI, users specify the account and region they want to target. During the run, each policy in the config will generate metrics that are sent to **TO-DO: Azure flow? ** in the account that is targeted. 

The run will also generate structured record output and logs that can be sent to an Azure Blob Storage Account object. in the account Custodian was run from. 

If Custodian is being run without **TO-DO: Azure flow?**  Assume Roles, all output will be put into the same account. 

Custodian is built with the ability to be run from different accounts and leverage STS Role Assumption for cross-account access. Users can leverage the metrics that are being generated after each run by creating Custodian Dashboards in **TO-DO Azure flow?** Azure OMS (other?).

### Concepts
[Policy](https://cloudcustodian.io/docs/azure/policy/index.html)

**TO-DO Resource**

**TO-DO Run Mode**

[Action](https://cloudcustodian.io/docs/azure/policy/genericarmaction.html#azure-genericarmaction)

[Filters - Metric/Tag/Marked for Op](https://cloudcustodian.io/docs/filters.html#filters)

[On/Off Hours](https://cloudcustodian.io/docs/quickstart/offhours.html)

## Getting Started

[Getting Started and writing your first policy]( https://cloudcustodian.io/docs/azure/gettingstarted.html)

**TO-DO: Confirm install details - Windows, Linux, Mac, Docker)**

[Setting up Azure Authentication](https://cloudcustodian.io/docs/azure/authentication.html)

**NOTE: These Azure Security Roles are required for the Cloud Custodian Service Principal**
- Set Role to Contributor
- Storage Roles
- If writing logs to Azure Blob Storage or leveraging Storage Queues for Mailer use case, also assign Storage roles, either at the subscription level or resource group/storage account level.
  - Blob Data Contributor  
  - Queue Data Contributor
 
**For reference:** Why/What of Authentication on Azure

- [Create an Azure service principal with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

- [Azure Authentication Basics](https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-scenarios)

- [Application and service principal objects in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)

### Cloud Custodian Drill Down - Basic

[Basic Examples](https://cloudcustodian.io/docs/azure/examples/index.html)

[Advanced Usage](https://cloudcustodian.io/docs/azure/advanced/index.html)

## Cloud Custodian Drill down - Basic Examples
**To-Do: Azure ARM templates to set up and demonstrate policy execution results**

- [Automatically tag the creator of a resource or resource group](https://cloudcustodian.io/docs/azure/examples/autotagusers.html)
- VM Tagging
    - [Add Tags](https://cloudcustodian.io/docs/azure/examples/tagadd.html)
    - [Remove Tags](https://cloudcustodian.io/docs/azure/examples/tagremove.html)
    - [Trim Extra Tags](https://cloudcustodian.io/docs/azure/examples/tagtrim.html)
- Filtering
    - [Filter load balancer by front end public ip](https://cloudcustodian.io/docs/azure/examples/lbfilterbyfrotendip.html)
    - [Filter resources by metrics from Azure Monitor](https://cloudcustodian.io/docs/azure/examples/metrics.html)
    - [Find Stopped Virtual Machines](https://cloudcustodian.io/docs/azure/examples/stoppedvm.html)
    - [Find Virtual Machines with Public IP address (Using JMESPath)](https://cloudcustodian.io/docs/azure/examples/vmwithpublicip.html)
- Take Action
    - [Remove Empty Resource Groups](https://cloudcustodian.io/docs/azure/examples/rgremoveempty.html)
    - Enforce SSH Keys **To-Do: From Brown Bag **
    - [Deny access to Network Security Group](https://cloudcustodian.io/docs/azure/examples/nsg.html)
    - [Resize an Application Service Plan](https://cloudcustodian.io/docs/azure/examples/resizeappplan.html)

## Cloud Custodian Drill down - Advanced
**To-Do: Additional Set-Up or Instructions needed?**
- [Examples and documentation for specific Azure Services](https://cloudcustodian.io/docs/azure/policy/index.html)
- Specialized Features
    - [KeyVault Whitelist Filters](https://cloudcustodian.io/docs/azure/policy/resources/keyvault.html)
    - [Related services, eg., frontend-public-ip on load balancer](https://cloudcustodian.io/docs/azure/policy/resources/loadbalancer.html)
    - [Network Security Groups: Ingress / Egress; Open / Close](https://cloudcustodian.io/docs/azure/policy/resources/nsg.html)
    - **TO-DO Azure Policy - compare / contrast?**
    - **TO-DO VM filters / actions - drill down / scenarios?**
- Advanced Architecture
  - [Mailer Notification](https://cloudcustodian.io/docs/azure/examples/notify.html)
  - [Azure Functions](https://cloudcustodian.io/docs/azure/advanced/azurefunctions.html)
    - Architecture
    - Flow (eg, auto-create Azure resources, etc)
    - Provision and Execution Options
    - [Event Grid Functions](https://github.com/cloud-custodian/cloud-custodian/blob/master/tools/c7n_azure/c7n_azure/azure_events.py)
   - [Managing Cloud Custodian across multiple Azure Subscriptions](https://cloudcustodian.io/docs/azure/advanced/multiplesubs.html)
   - [Logging - Blob Storage](https://cloudcustodian.io/docs/azure/advanced/bloboutput.html)
   - **TO-DO: Alternative Architecture? (Andy Gee)**
- [Installation for Developers](https://cloudcustodian.io/docs/developer/installing.html#developer-installing)
    - **To-Do: ** Notes / Guidance from H/F?
  