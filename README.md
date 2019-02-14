# Guide to Cloud Custodian on Azure

**What is Cloud Custodian?**

As enterprises evolve their computing environments from their on-premises data centers to include deployment across multiple public clouds, they are concerned about new business risk exposures that might arise.  

These risk exposure concerns generally center around: 
1) Security Vulnerabilities
2) Cost Controls
3) Audit compliance and governance

Cloud Custodian is a solution for unifying the diverse landscape of tools most organizations use for managing these issues into one, consistent policy enforcement paradigm.  Cloud Custodian uses open source (Python) technology to provide a stateless rules engine for cross-cloud policy definition and enforcement, metrics and detailed reporting.  

Organizations can use Cloud Custodian to ensure compliance to security policies, tag deployed service instances, shut down unused resources, and provide cost management via off-hours resource management.  This is all managed and executed from a single point of control with policies written in simple, YAML-based configuration files.  

Cloud Custodian also provides the flexibility to deploy to a variety of execution environments: local command line interface or scripting, virtual machine servers, or serverless (eg., Docker, Kubernetes).  As a result, versioning and deployment of the Cloud Custodian Python-based executables and YAML-based configuration files can all be managed via standard, existing IT DevOps processes.

The Cloud Custodian policies are based on a common vocabulary of FILTERS, RESOURCES and ACTIONS.  The policies define filters to select a subset of cloud resources.  The policies also instruct Cloud Custodian what action or actions to take on this subset.  This approach and vocabulary is comparable across public cloud providers.

Some typical use cases where Cloud Custodian policies enforce corporate controls:
- Disable Port 22 on all Virtual Machines within an Azure Security Group
- Shut down any Virtual Machines not provisioned to use SSH Key user authentication
- Limit Virtual Machine deployment to a set of economical SKUs
- Remove un-used Azure Storage Disks
- Shut down Cloud services between certain hours

**To get started quickly, visit [here](https://cloudcustodian.io/docs/azure/gettingstarted.html)**

**For a quick reference of the currently supported Azure resources, and much more, visit [here](https://cloudcustodian.io/docs/azure/policy/index.html)**

**Architecture**

!["Cloud Custodian uses simple Yaml to enforce policy"](./pictures/yml-picture.jpg "Cloud Custodian uses simple Yaml to enforce policy")

!["Cloud Custodian enables a consistent policy enforcement across clouds"](./pictures/cc-multi-cloud.jpg "Cloud Custodian enables a consistent policy enforcement across clouds")

### **TO-DO: Component Overview**

### Deployment
**To-Do: Need Azure Version**
https://cloudcustodian.io/docs/overview/deployment.html

### Process flow & How it Works
The language also supports compound querying. This essentially allows you to filter for things like running VM instances with attached disks that are not set to delete on instance termination or stopped instances. This filtering can take into account external data sources. It also provides for resource specific actions around deletion, stopping, starting, encryption, tagging, etc.

When a user runs Cloud Custodian, it will run the specified policy against the subscription specified by the user. Custodian will iterate over all resources defined in the policy. In the CLI, users specify the account and region they want to target. During the run, each policy in the config will generate metrics that are sent to CloudWatch in the account that is targeted. The run will also generate structured record output and logs that can be sent to an S3 Bucket and CloudWatch Log Groups in the account Custodian was run from. If Custodian is being run without Assume Roles, all output will be put into the same account. Custodian is built with the ability to be run from different accounts and leverage STS Role Assumption for cross-account access. Users can leverage the metrics that are being generated after each run by creating Custodian Dashboards in CloudWatch.

Multi Subscription:
https://github.com/cloud-custodian/cloud-custodian/tree/master/tools/c7n_org


Custodian uses a flexible query language for filtering resources to a particular subset that allows for compound querying. This essentially allows you to filter for things like instances with attached Azure Blob Storage disks or stopped instances. This filtering can take into account external data sources. It also provides for resource specific actions around deletion, stopping, starting, encryption, tagging, etc.

The stateless design of Custodian greatly simplifies feature development and operations. It also provides flexibility around execution environment (local cli, vm-based server or serverless container).

When a user runs Custodian, Custodian will run the specified policy against the account and region specified by the user. Custodian will iterate over all resources defined in the policy. In the CLI, users specify the account and region they want to target. During the run, each policy in the config will generate metrics that are sent to **TO-DO: Azure flow?** in the account that is targeted. 

The run will also generate structured record output and logs that can be sent to an Azure Blob Storage Account object. in the account Custodian was run from. 

If Custodian is being run without **TO-DO: Azure flow?**  Assume Roles, all output will be put into the same account. 

Custodian is built with the ability to be run from different accounts and leverage STS Role Assumption for cross-account access. Users can leverage the metrics that are being generated after each run by creating Custodian Dashboards in **TO-DO Azure flow?Azure OMS (other?)**.

### Concepts
[Policy](https://cloudcustodian.io/docs/azure/policy/index.html)

**TO-DO Resource**

**TO-DO Run Mode**

[Action](https://cloudcustodian.io/docs/azure/policy/genericarmaction.html#azure-genericarmaction)

[Filters - Metric/Tag/Marked for Op](https://cloudcustodian.io/docs/filters.html#filters)

[On/Off Hours](https://cloudcustodian.io/docs/quickstart/offhours.html)

## Getting Started

[Getting Started and writing your first policy]( https://cloudcustodian.io/docs/azure/gettingstarted.html)

**TO-DO: Based on H/F feedback - Confirm install details (Windows, Linux, Mac, Docker)**

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
   - [Using Azure Functions and Logic Apps](https://github.com/plooploops/azure-functions-c7n-test/blob/master/scenarios-read-me/azure-functions-logic-apps-readme.md)
- [Installation for Developers](https://cloudcustodian.io/docs/developer/installing.html#developer-installing)
    - **To-Do: ** Notes / Guidance from H/F?
  