---
title: Defender for Cloud Planning and Operations Guide
description: This document helps you to plan before adopting Defender for Cloud and considerations regarding daily operations.
ms.topic: tutorial
ms.date: 12/14/2021
---
# Planning and operations guide

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

This guide is for information technology (IT) professionals, IT architects, information security analysts, and cloud administrators planning to use Defender for Cloud.


## Planning guide
This guide covers tasks that you can follow to optimize your use of Defender for Cloud based on your organization's security requirements and cloud management model. To take full advantage of Defender for Cloud, it is important to understand how different individuals or teams in your organization use the service to meet secure development and operations, monitoring, governance, and incident response needs. The key areas to consider when planning to use Defender for Cloud are:

* Security Roles and Access Controls
* Security Policies and Recommendations
* Data Collection and Storage
* Onboarding non-Azure resources
* Ongoing Security Monitoring
* Incident Response

In the next section, you will learn how to plan for each one of those areas and apply those recommendations based on your requirements.


> [!NOTE]
> Read [Defender for Cloud frequently asked questions (FAQ)](faq-general.yml) for a list of common questions that can also be useful during the designing and planning phase.

## Security roles and access controls
Depending on the size and structure of your organization, multiple individuals and teams may use Defender for Cloud to perform different security-related tasks. In the following diagram, you have an example of fictitious personas and their respective roles and security responsibilities:

![Roles.](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Defender for Cloud enables these individuals to meet these various responsibilities. For example:

**Jeff (Workload Owner)**

* Manage a cloud workload and its related resources
* Responsible for implementing and maintaining protections in accordance with company security policy

**Ellen (CISO/CIO)**

* Responsible for all aspects of security for the company
* Wants to understand the company's security posture across cloud workloads
* Needs to be informed of major attacks and risks

**David (IT Security)**

* Sets company security policies to ensure the appropriate protections are in place
* Monitors compliance with policies
* Generates reports for leadership or auditors

**Judy (Security Operations)**

* Monitors and responds to security alerts 24/7
* Escalates to Cloud Workload Owner or IT Security Analyst

**Sam (Security Analyst)**

* Investigate attacks
* Work with Cloud Workload Owner to apply remediation

Defender for Cloud uses [Azure role-based access control (Azure RBAC)](../role-based-access-control/role-assignments-portal.md), which provides [built-in roles](../role-based-access-control/built-in-roles.md) that can be assigned to users, groups, and services in Azure. When a user opens Defender for Cloud, they only see information related to resources they have access to. Which means the user is assigned the role of Owner, Contributor, or Reader to the subscription or resource group that a resource belongs to. In addition to these roles, there are two specific Defender for Cloud roles:

- **Security reader**: a user that belongs to this role is able to view only Defender for Cloud configurations, which include recommendations, alerts, policy, and health, but it won't be able to make changes.
- **Security admin**: same as security reader but it can also update the security policy, dismiss recommendations and alerts.

The Defender for Cloud roles described above do not have access to other service areas of Azure such as Storage, Web & Mobile, or Internet of Things.

Using the personas explained in the previous diagram, the following Azure RBAC would be needed:

**Jeff (Workload Owner)**

* Resource Group Owner/Contributor

**Ellen (CISO/CIO)**

* Subscription Owner/Contributor or Security Admin

**David (IT Security)**

* Subscription Owner/Contributor or Security Admin

**Judy (Security Operations)**

* Subscription Reader or Security Reader to view Alerts
* Subscription Owner/Contributor or Security Admin required to dismiss Alerts

**Sam (Security Analyst)**

* Subscription Reader to view Alerts
* Subscription Owner/Contributor required to dismiss Alerts
* Access to the workspace may be required

Some other important information to consider:

* Only subscription Owners/Contributors and Security Admins can edit a security policy.
* Only subscription and resource group Owners and Contributors can apply security recommendations for a resource.

When planning access control using Azure RBAC for Defender for Cloud, be sure to understand who in your organization will be using Defender for Cloud. Also, what types of tasks they will be performing and then configure Azure RBAC accordingly.

> [!NOTE]
> We recommend that you assign the least permissive role needed for users to complete their tasks. For example, users who only need to view information about the security state of resources but not take action, such as applying recommendations or editing policies, should be assigned the Reader role.
>
>

## Security policies and recommendations
A security policy defines the desired configuration of your workloads and helps ensure compliance with company or regulatory security requirements. In Defender for Cloud, you can define policies for your Azure subscriptions, which can be tailored to the type of workload or the sensitivity of data.

Defender for Cloud policies contain the following components:
- [Data collection](enable-data-collection.md): agent provisioning and data collection settings.
- [Security policy](tutorial-security-policy.md): an [Azure Policy](../governance/policy/overview.md) that determines which controls are monitored and recommended by Defender for Cloud, or use Azure Policy to create new definitions, define additional policies, and assign policies across management groups.
- [Email notifications](configure-email-notifications.md): security contacts and notification settings.
- [Pricing tier](enhanced-security-features-overview.md): with or without Microsoft Defender for Cloud's enhanced security features, which determine which Defender for Cloud features are available for resources in scope (can be specified for subscriptions and workspaces using the API).

> [!NOTE]
> Specifying a security contact will ensure that Azure can reach the right person in your organization if a security incident occurs. Read [Provide security contact details in Defender for Cloud](configure-email-notifications.md) for more information on how to enable this recommendation.

### Security policies definitions and recommendations
Defender for Cloud automatically creates a default security policy for each of your Azure subscriptions. You can edit the policy in Defender for Cloud or use Azure Policy to create new definitions, define additional policies, and assign policies across Management Groups (which can represent the entire organization, a business unit in it etc.), and monitor compliance to these policies across these scopes.

Before configuring security policies, review each of the [security recommendations](review-security-recommendations.md), and determine whether these policies are appropriate for your various subscriptions and resource groups. It is also important to understand what action should be taken to address Security Recommendations and who in your organization will be responsible for monitoring for new recommendations and taking the needed steps.

## Data collection and storage
Defender for Cloud uses the Log Analytics agent – this is the same agent used by the Azure Monitor service – to collect security data from your virtual machines. [Data collected](enable-data-collection.md) from this agent will be stored in your Log Analytics workspace(s).

### Agent

When automatic provisioning is enabled in the security policy, the Log Analytics agent (for [Windows](../azure-monitor/agents/agent-windows.md) or [Linux](../azure-monitor/vm/monitor-virtual-machine.md)) is installed on all supported Azure VMs, and any new ones that are created. If the VM or computer already has the Log Analytics agent installed, Defender for Cloud will leverage the current installed agent. The agent's process is designed to be non-invasive and have very minimal impact on VM performance.

The Log Analytics agent for Windows requires use TCP port 443. See the [Troubleshooting article](troubleshooting-guide.md) for additional details.

If at some point you want to disable Data Collection, you can turn it off in the security policy. However, because the Log Analytics agent may be used by other Azure management and monitoring services, the agent will not be uninstalled automatically when you turn off data collection in Defender for Cloud. You can manually uninstall the agent if needed.

> [!NOTE]
> To find a list of supported VMs, read the [Defender for Cloud frequently asked questions (FAQ)](faq-vms.yml).

### Workspace

A workspace is an Azure resource that serves as a container for data. You or other members of your organization might use multiple workspaces to manage different sets of data that is collected from all or portions of your IT infrastructure.

Data collected from the Log Analytics agent (on behalf of Defender for Cloud) will be stored in either an existing Log Analytics workspace(s) associated with your Azure subscription or a new workspace(s), taking into account the Geo of the VM.

In the Azure portal, you can browse to see a list of your Log Analytics workspaces, including any created by Defender for Cloud. A related resource group will be created for new workspaces. Both will follow this naming convention:

* Workspace: *DefaultWorkspace-[subscription-ID]-[geo]*
* Resource Group: *DefaultResourceGroup-[geo]*

For workspaces created by Defender for Cloud, data is retained for 30 days. For existing workspaces, retention is based on the workspace pricing tier. If you want, you can also use an existing workspace.

If your agent reports to a workspace other than the **default** workspace, any Microsoft Defender plans providing [enhanced security features](enhanced-security-features-overview.md) that you've enabled on the subscription should also be enabled on the workspace.

> [!NOTE]
> Microsoft makes strong commitments to protect the privacy and security of this data. Microsoft adheres to strict compliance and security guidelines—from coding to operating a service. For more information about data handling and privacy, read [Defender for Cloud Data Security](data-security.md).
>

## Onboard non-Azure resources

Defender for Cloud can monitor the security posture of your non-Azure computers but you need to first onboard these resources. Read [Onboard non-Azure computers](quickstart-onboard-machines.md) for more information on how to onboard non-Azure resources.

## Ongoing security monitoring
After initial configuration and application of Defender for Cloud recommendations, the next step is considering Defender for Cloud operational processes.

The Defender for Cloud Overview provides a unified view of security across all your Azure resources and any non-Azure resources you have connected. The example below shows an environment with many issues to be addressed:

![dashboard.](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig11.png)

> [!NOTE]
> Defender for Cloud will not interfere with your normal operational procedures, it will passively monitor your deployments and provide recommendations based on the security policies you enabled.

When you first opt in to use Defender for Cloud for your current Azure environment, make sure that you review all recommendations, which can be done in the **Recommendations** page.

Plan to visit the threat intelligence option as part of your daily security operations. There you can identify security threats against the environment, such as identify if a particular computer is part of a botnet.

### Monitoring for new or changed resources

Most Azure environments are dynamic, with resources regularly being created, spun up or down, reconfigured, and changed. Defender for Cloud helps ensure that you have visibility into the security state of these new resources.

When you add new resources (VMs, SQL DBs) to your Azure Environment, Defender for Cloud will automatically discover these resources and begin to monitor their security. This also includes PaaS web roles and worker roles. If Data Collection is enabled in the [Security Policy](tutorial-security-policy.md), additional monitoring capabilities will be enabled automatically for your virtual machines.

You should also regularly monitor existing resources for configuration changes that could have created security risks, drift from recommended baselines, and security alerts. 

### Hardening access and applications

As part of your security operations, you should also adopt preventative measures to restrict access to VMs, and control the applications that are running on VMs. By locking down inbound traffic to your Azure VMs, you are reducing the exposure to attacks, and at the same time providing easy access to connect to VMs when needed. Use [just-in-time VM access](just-in-time-access-usage.md) access feature to hardening access to your VMs.

You can use [adaptive application controls](adaptive-application-controls.md) to limit which applications can run on your VMs located in Azure. Among other benefits, this helps harden your VMs against malware. Using machine learning, Defender for Cloud analyzes processes running in the VM to help you create allowlist rules.


## Incident response
Defender for Cloud detects and alerts you to threats as they occur. Organizations should monitor for new security alerts and take action as needed to investigate further or remediate the attack. For more information on how Defender for Cloud threat protection works, read [How Defender for Cloud detects and responds to threats](alerts-overview.md#detect-threats).

While this article doesn't have the intent to assist you creating your own Incident Response plan, we are going to use Microsoft Azure Security Response in the Cloud lifecycle as the foundation for incident response stages. The stages are shown in the following diagram:

![Stages of the incident response in the cloud lifecycle.](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> You can use the National Institute of Standards and Technology (NIST) [Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) as a reference to assist you building your own.
>

You can use Defender for Cloud Alerts during the following stages:

* **Detect**: identify a suspicious activity in one or more resources.
* **Assess**: perform the initial assessment to obtain more information about the suspicious activity.
* **Diagnose**: use the remediation steps to conduct the technical procedure to address the issue.

Each Security Alert provides information that can be used to better understand the nature of the attack and suggest possible mitigations. Some alerts also provide links to either more information or to other sources of information within Azure. You can use the information provided for further research and to begin mitigation, and you can also search security-related data that is stored in your workspace.

The following example shows a suspicious RDP activity taking place:

![Suspicious activity.](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

This page shows the details regarding the time that the attack took place, the source hostname, the target VM and also gives recommendation steps. In some circumstances, the source information of the attack may be empty. Read [Missing Source Information in Defender for Cloud Alerts](/archive/blogs/azuresecurity/missing-source-information-in-azure-security-center-alerts) for more information about this type of behavior.

Once you identify the compromised system, you can run a [workflow automation](workflow-automation.md) that was previously created. These are a collection of procedures that can be executed from Defender for Cloud once triggered by an alert.

In the How to Leverage the Defender for Cloud & Microsoft Operations Management Suite for an Incident Response video, you can see some demonstrations that show how Defender for Cloud can be used in each one of those stages.

> [!NOTE]
> Read [Managing and responding to security alerts in Defender for Cloud](managing-and-responding-alerts.md) for more information on how to use Defender for Cloud capabilities to assist you during your Incident Response process.
>
>

## Next steps
In this document, you learned how to plan for Defender for Cloud adoption. To learn more about Defender for Cloud, see the following:

* [Managing and responding to security alerts in Defender for Cloud](managing-and-responding-alerts.md)
* [Monitoring partner solutions with Defender for Cloud](./partner-integration.md) — Learn how to monitor the health status of your partner solutions.
* [Defender for Cloud FAQ](faq-general.yml) — Find frequently asked questions about using the service.
* [Azure Security blog](/archive/blogs/azuresecurity/) — Find blog posts about Azure security and compliance.
