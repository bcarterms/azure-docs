---
title: Release notes for Microsoft Defender for Cloud
description: A description of what's new and changed in Microsoft Defender for Cloud
ms.topic: reference
ms.date: 03/10/2022
---
# What's new in Microsoft Defender for Cloud?

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Defender for Cloud is in active development and receives improvements on an ongoing basis. To stay up to date with the most recent developments, this page provides you with information about new features, bug fixes, and deprecated functionality.

This page is updated frequently, so revisit it often. 

To learn about *planned* changes that are coming soon to Defender for Cloud, see [Important upcoming changes to Microsoft Defender for Cloud](upcoming-changes.md). 

> [!TIP]
> If you're looking for items older than six months, you'll find them in the [Archive for What's new in Microsoft Defender for Cloud](release-notes-archive.md).

## March 2022

Updates in March include:

- [Deprecated the recommendations to install the network traffic data collection agent](#deprecated-the-recommendations-to-install-the-network-traffic-data-collection-agent)
- [Defender for Containers can now scan for vulnerabilities in Windows images (preview)](#defender-for-containers-can-now-scan-for-vulnerabilities-in-windows-images-preview)
- [New alert for Microsoft Defender for Storage (preview)](#new-alert-for-microsoft-defender-for-storage-preview)
- [Configure email notifications settings from an alert](#configure-email-notifications-settings-from-an-alert)
- [Deprecated preview alert: ARM.MCAS_ActivityFromAnonymousIPAddresses](#deprecated-preview-alert-armmcas_activityfromanonymousipaddresses)

### Deprecated the recommendations to install the network traffic data collection agent

Changes in our roadmap and priorities have removed the need for the network traffic data collection agent. Consequently, the following two recommendations and their related policies were deprecated.  

|Recommendation |Description |Severity |
|---|---|---|
|[Network traffic data collection agent should be installed on Linux virtual machines](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/8c3e93d3-0276-4d06-b20a-9a9f3012742c) |Defender for Cloud uses the Microsoft Dependency agent to collect network traffic data from your Azure virtual machines to enable advanced network protection features such as traffic visualization on the network map, network hardening recommendations and specific network threats. |Medium |
|[Network traffic data collection agent should be installed on Windows virtual machines](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/24d8af06-d441-40b4-a49c-311421aa9f58) |Defender for Cloud uses the Microsoft Dependency agent to collect network traffic data from your Azure virtual machines to enable advanced network protection features such as traffic visualization on the network map, network hardening recommendations, and specific network threats. |Medium |
|||

### Defender for Containers can now scan for vulnerabilities in Windows images (preview)

Defender for Container's image scan now supports Windows images that are hosted in Azure Container Registry. This feature is free while in preview, and will incur a cost when it becomes generally available.

Learn more in [Use Microsoft Defender for Container to scan your images for vulnerabilities](defender-for-container-registries-usage.md).

### New alert for Microsoft Defender for Storage (preview)

To expand the threat protections provided by Microsoft Defender for Storage, we've added a new preview alert.

Threat actors use applications and tools to discover and access storage accounts. Microsoft Defender for Storage detects these applications and tools so that you can block them and remediate your posture.

This preview alert is called `Access from a suspicious application`. The alert is relevant to Azure Blob Storage, and ADLS Gen2 only.

| Alert (alert type) | Description | MITRE tactic | Severity |
|--|--|--|--|
| **PREVIEW - Access from a suspicious application**<br>(Storage.Blob_SuspiciousApp) | Indicates that a suspicious application has successfully accessed a container of a storage account with authentication.<br>This might indicate that an attacker has obtained the credentials necessary to access the account, and is exploiting it. This could also be an indication of a penetration test carried out in your organization.<br>Applies to: Azure Blob Storage, Azure Data Lake Storage Gen2 | Initial Access | Medium |

### Configure email notifications settings from an alert

A new section has been added to the alert User Interface (UI) which allows you to view and edit who will receive email notifications for alerts that are triggered on the current subscription.

:::image type="content" source="media/release-notes/configure-email.png" alt-text="Screenshot of the new UI showing how to configure email notification.":::

Learn how to [Configure email notifications for security alerts](configure-email-notifications.md).

### Deprecated preview alert: ARM.MCAS_ActivityFromAnonymousIPAddresses

The following preview alert has been deprecated:

|Alert name| Description|
|----------------------|---------------------------|
|**PREVIEW - Activity from a risky IP address**<br>(ARM.MCAS_ActivityFromAnonymousIPAddresses)|Users activity from an IP address that has been identified as an anonymous proxy IP address has been detected.<br>These proxies are used by people who want to hide their device's IP address, and can be used for malicious intent. This detection uses a machine learning algorithm that reduces false positives, such as mis-tagged IP addresses that are widely used by users in the organization.<br>Requires an active Microsoft Defender for Cloud Apps license.|

A new alert has been created that provides this information and adds to it. In addition, the newer alerts (ARM_OperationFromSuspiciousIP, ARM_OperationFromSuspiciousProxyIP) doesn't require a license for Microsoft Defender for Cloud Apps (formerly known as Microsoft Cloud App Security).

See more alerts for [Resource Manager](alerts-reference.md#alerts-resourcemanager).

## February 2022

Updates in February include:

- [Kubernetes workload protection for Arc-enabled Kubernetes clusters](#kubernetes-workload-protection-for-arc-enabled-kubernetes-clusters)
- [Native CSPM for GCP and threat protection for GCP compute instances](#native-cspm-for-gcp-and-threat-protection-for-gcp-compute-instances)
- [Microsoft Defender for Azure Cosmos DB plan released for preview](#microsoft-defender-for-azure-cosmos-db-plan-released-for-preview)
- [Threat protection for Google Kubernetes Engine (GKE) clusters](#threat-protection-for-google-kubernetes-engine-gke-clusters)

### Kubernetes workload protection for Arc-enabled Kubernetes clusters 

Defender for Containers previously only protected Kubernetes workloads running in Azure Kubernetes Service. We've now extended the protective coverage to include Azure Arc-enabled Kubernetes clusters.

Learn how to [set up your Kubernetes workload protection](kubernetes-workload-protections.md#set-up-your-workload-protection) for AKS and Azure Arc enabled Kubernetes clusters.

### Native CSPM for GCP and threat protection for GCP compute instances

The new automated onboarding of GCP environments allows you to protect GCP workloads with Microsoft Defender for Cloud. Defender for Cloud protects your resources with the following plans:

- **Defender for Cloud's CSPM** features extend to your GCP resources. This agentless plan assesses your GCP resources according to the GCP-specific security recommendations, which are provided with Defender for Cloud. GCP recommendations are included in your secure score, and the resources will be assessed for compliance with the built-in GCP CIS standard. Defender for Cloud's asset inventory page is a multi-cloud enabled feature helping you manage your resources across Azure, AWS, and GCP.

- **Microsoft Defender for servers** brings threat detection and advanced defenses to your GCP compute instances. This plan includes the integrated license for Microsoft Defender for Endpoint, vulnerability assessment scanning, and more.

    For a full list of available features, see [Supported features for virtual machines and servers](supported-machines-endpoint-solutions-clouds-servers.md). Automatic onboarding capabilities will allow you to easily connect any existing, and new compute instances discovered in your environment.

Learn how to protect, and [connect your GCP projects](quickstart-onboard-gcp.md) with Microsoft Defender for Cloud.

### Microsoft Defender for Azure Cosmos DB plan released for preview

We have extended Microsoft Defender for Cloud’s database coverage. You can now enable protection for your Azure Cosmos DB databases. 

Microsoft Defender for Azure Cosmos DB is an Azure-native layer of security that detects any attempt to exploit databases in your Azure Cosmos DB accounts. Microsoft Defender for Azure Cosmos DB detects potential SQL injections, known bad actors based on Microsoft Threat Intelligence, suspicious access patterns, and potential exploitation of your database through compromised identities, or malicious insiders. 

It continuously analyzes the customer data stream generated by the Azure Cosmos DB services.

When potentially malicious activities are detected, security alerts are generated. These alerts are displayed in Microsoft Defender for Cloud together with the details of the suspicious activity along with the relevant investigation steps, remediation actions, and security recommendations. 

There's no impact on database performance when enabling the service, because Defender for Azure Cosmos DB doesn't access the Azure Cosmos DB account data. 

Learn more at [Introduction to Microsoft Defender for Azure Cosmos DB](concept-defender-for-cosmos.md).

We're also introducing a new enablement experience for database security. You can now enable Microsoft Defender for Cloud protection on your subscription to protect all database types, such as, Azure Cosmos DB, Azure SQL Database, Azure SQL servers on machines, and Microsoft Defender for open-source relational databases through one enablement process. Specific resource types can be included, or excluded by configuring your plan.  

Learn how to [enable your database security at the subscription level](quickstart-enable-defender-for-cosmos.md#enable-database-protection-at-the-subscription-level).

### Threat protection for Google Kubernetes Engine (GKE) clusters

Following our recent announcement [Native CSPM for GCP and threat protection for GCP compute instances](#native-cspm-for-gcp-and-threat-protection-for-gcp-compute-instances), Microsoft Defender for Containers has extended its Kubernetes threat protection, behavioral analytics, and built-in admission control policies to Google's Kubernetes Engine (GKE) Standard clusters. You can easily onboard any existing, or new GKE Standard clusters to your environment through our Automatic onboarding capabilities. Check out [Container security with Microsoft Defender for Cloud](defender-for-containers-introduction.md#vulnerability-assessment), for a full list of available features.


## January 2022

Updates in January include:

- [Microsoft Defender for Resource Manager updated with new alerts and greater emphasis on high-risk operations mapped to MITRE ATT&CK® Matrix](#microsoft-defender-for-resource-manager-updated-with-new-alerts-and-greater-emphasis-on-high-risk-operations-mapped-to-mitre-attck-matrix)
- [Recommendations to enable Microsoft Defender plans on workspaces (in preview)](#recommendations-to-enable-microsoft-defender-plans-on-workspaces-in-preview)
- [Auto provision Log Analytics agent to Azure Arc-enabled machines (preview)](#auto-provision-log-analytics-agent-to-azure-arc-enabled-machines-preview)
- [Deprecated the recommendation to classify sensitive data in SQL databases](#deprecated-the-recommendation-to-classify-sensitive-data-in-sql-databases)
- [Communication with suspicious domain alert expanded to included known Log4Shell-related domains](#communication-with-suspicious-domain-alert-expanded-to-included-known-log4shell-related-domains)
- ['Copy alert JSON' button added to security alert details pane](#copy-alert-json-button-added-to-security-alert-details-pane)
- [Renamed two recommendations](#renamed-two-recommendations)
- [Deprecate Kubernetes cluster containers should only listen on allowed ports policy](#deprecate-kubernetes-cluster-containers-should-only-listen-on-allowed-ports-policy)
- [Added 'Active Alerts' workbook](#added-active-alert-workbook)
- ['System update' recommendation added to government cloud](#system-update-recommendation-added-to-government-cloud)

### Microsoft Defender for Resource Manager updated with new alerts and greater emphasis on high-risk operations mapped to MITRE ATT&CK® Matrix

The cloud management layer is a crucial service connected to all your cloud resources. Because of this, it's also a potential target for attackers. Consequently, we recommend security operations teams closely monitor the resource management layer.

Microsoft Defender for Resource Manager automatically monitors the resource management operations in your organization, whether they're performed through the Azure portal, Azure REST APIs, Azure CLI, or other Azure programmatic clients. Defender for Cloud runs advanced security analytics to detect threats and alerts you about suspicious activity.

The plan's protections greatly enhance an organization's resiliency against attacks from threat actors and significantly increase the number of Azure resources protected by Defender for Cloud.

In December 2020, we introduced the preview of Defender for Resource Manager, and in May 2021 the plan was release for general availability. 

With this update, we've comprehensively revised the focus of the Microsoft Defender for Resource Manager plan. The updated plan includes many **new alerts focused on identifying suspicious invocation of high-risk operations**. These new alerts provide extensive monitoring for attacks across the *complete* [MITRE ATT&CK® matrix for cloud-based techniques](https://attack.mitre.org/matrices/enterprise/cloud/).

This matrix covers the following range of potential intentions of threat actors who may be targeting your organization's resources: *Initial Access, Execution, Persistence, Privilege Escalation, Defense Evasion, Credential Access, Discovery, Lateral Movement, Collection, Exfiltration, and Impact*.

The new alerts for this Defender plan cover these intentions as shown in the following table.

> [!TIP]
> These alerts also appear in the [alerts reference page](alerts-reference.md).

| Alert (alert type)                                                                                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | MITRE tactics (intentions)| Severity |
|----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------:|----------|
| **Suspicious invocation of a high-risk 'Initial Access' operation detected (Preview)**<br>(ARM_AnomalousOperation.InitialAccess)             | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to access restricted resources. The identified operations are designed to allow administrators to efficiently access their environments. While this activity may be legitimate, a threat actor might utilize such operations to gain initial access to restricted resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.                  | Initial Access            | Medium   |
| **Suspicious invocation of a high-risk 'Execution' operation detected (Preview)**<br>(ARM_AnomalousOperation.Execution)                      | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation on a machine in your subscription, which might indicate an attempt to execute code. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to access restricted credentials and compromise resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.         | Execution                 | Medium   |
| **Suspicious invocation of a high-risk 'Persistence' operation detected (Preview)**<br>(ARM_AnomalousOperation.Persistence)                  | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to establish persistence. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to establish persistence in your environment. This can indicate that the account is compromised and is being used with malicious intent.                                              | Persistence               | Medium   |
| **Suspicious invocation of a high-risk 'Privilege Escalation' operation detected (Preview)**<br>(ARM_AnomalousOperation.PrivilegeEscalation) | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to escalate privileges. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to escalate privileges while compromising resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.                     | Privilege Escalation      | Medium   |
| **Suspicious invocation of a high-risk 'Defense Evasion' operation detected (Preview)**<br>(ARM_AnomalousOperation.DefenseEvasion)           | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to evade defenses. The identified operations are designed to allow administrators to efficiently manage the security posture of their environments. While this activity may be legitimate, a threat actor might utilize such operations to avoid being detected while compromising resources in your environment. This can indicate that the account is compromised and is being used with malicious intent. | Defense Evasion           | Medium   |
| **Suspicious invocation of a high-risk 'Credential Access' operation detected (Preview)**<br>(ARM_AnomalousOperation.CredentialAccess)       | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to access credentials. The identified operations are designed to allow administrators to efficiently access their environments. While this activity may be legitimate, a threat actor might utilize such operations to access restricted credentials and compromise resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.                | Credential Access         | Medium   |
| **Suspicious invocation of a high-risk 'Lateral Movement' operation detected (Preview)**<br>(ARM_AnomalousOperation.LateralMovement)         | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to perform lateral movement. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to compromise additional resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.                                 | Lateral Movement          | Medium   |
| **Suspicious invocation of a high-risk 'Data Collection' operation detected (Preview)**<br>(ARM_AnomalousOperation.Collection)               | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempt to collect data. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to collect sensitive data on resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.                                         | Collection                | Medium   |
| **Suspicious invocation of a high-risk 'Impact' operation detected (Preview)**<br>(ARM_AnomalousOperation.Impact)                            | Microsoft Defender for Resource Manager identified a suspicious invocation of a high-risk operation in your subscription, which might indicate an attempted configuration change. The identified operations are designed to allow administrators to efficiently manage their environments. While this activity may be legitimate, a threat actor might utilize such operations to access restricted credentials and compromise resources in your environment. This can indicate that the account is compromised and is being used with malicious intent.               | Impact                    | Medium   |
|||||

In addition, these two alerts from this plan have come out of preview:

| Alert (alert type)                                                                                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | MITRE tactics (intentions)| Severity |
|----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------:|----------|
| **Azure Resource Manager operation from suspicious IP address**<br>(ARM_OperationFromSuspiciousIP)                                                      | Microsoft Defender for Resource Manager detected an operation from an IP address that has been marked as suspicious in threat intelligence feeds.                                                                                                                                                                                                                                                                                            | Execution                             | Medium   |
| **Azure Resource Manager operation from suspicious proxy IP address**<br>(ARM_OperationFromSuspiciousProxyIP)                                           | Microsoft Defender for Resource Manager detected a resource management operation from an IP address that is associated with proxy services, such as TOR. While this behavior can be legitimate, it's often seen in malicious activities, when threat actors try to hide their source IP.                                                                                                                                                     | Defense Evasion                       | Medium   |
|||||


### Recommendations to enable Microsoft Defender plans on workspaces (in preview)

To benefit from all of the security features available from [Microsoft Defender for servers](defender-for-servers-introduction.md) and [Microsoft Defender for SQL on machines](defender-for-sql-introduction.md), the plans must be enabled on **both** the subscription and workspace levels. 

When a machine is in a subscription with one of these plan enabled, you'll be billed for the full protections. However, if that machine is reporting to a workspace *without* the plan enabled, you won't actually receive those benefits. 

We've added two recommendations that highlight workspaces without these plans enabled, that nevertheless have machines reporting to them from subscriptions that *do* have the plan enabled. 

The two recommendations, which both offer automated remediation (the 'Fix' action), are:

|Recommendation |Description |Severity |
|---|---|---|
|[Microsoft Defender for servers should be enabled on workspaces](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/1ce68079-b783-4404-b341-d2851d6f0fa2) |Microsoft Defender for servers brings threat detection and advanced defenses for your Windows and Linux machines.<br>With this Defender plan enabled on your subscriptions but not on your workspaces, you're paying for the full capability of Microsoft Defender for servers but missing out on some of the benefits.<br>When you enable Microsoft Defender for servers on a workspace, all machines reporting to that workspace will be billed for Microsoft Defender for servers - even if they're in subscriptions without Defender plans enabled. Unless you also enable Microsoft Defender for servers on the subscription, those machines won't be able to take advantage of just-in-time VM access, adaptive application controls, and network detections for Azure resources.<br>Learn more in <a target="_blank" href="/azure/defender-for-cloud/defender-for-servers-introduction?wt.mc_id=defenderforcloud_inproduct_portal_recoremediation">Introduction to Microsoft Defender for servers</a>.<br />(No related policy) |Medium |
|[Microsoft Defender for SQL on machines should be enabled on workspaces](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/e9c320f1-03a0-4d2b-9a37-84b3bdc2e281) |Microsoft Defender for servers brings threat detection and advanced defenses for your Windows and Linux machines.<br>With this Defender plan enabled on your subscriptions but not on your workspaces, you're paying for the full capability of Microsoft Defender for servers but missing out on some of the benefits.<br>When you enable Microsoft Defender for servers on a workspace, all machines reporting to that workspace will be billed for Microsoft Defender for servers - even if they're in subscriptions without Defender plans enabled. Unless you also enable Microsoft Defender for servers on the subscription, those machines won't be able to take advantage of just-in-time VM access, adaptive application controls, and network detections for Azure resources.<br>Learn more in <a target="_blank" href="/azure/defender-for-cloud/defender-for-servers-introduction?wt.mc_id=defenderforcloud_inproduct_portal_recoremediation">Introduction to Microsoft Defender for servers</a>.<br />(No related policy) |Medium |
||||


### Auto provision Log Analytics agent to Azure Arc-enabled machines (preview)

Defender for Cloud uses the Log Analytics agent to gather security-related data from machines. The agent reads various security-related configurations and event logs and copies the data to your workspace for analysis.

Defender for Cloud's auto provisioning settings have a toggle for each type of supported extension, including the Log Analytics agent. 

In a further expansion of our hybrid cloud features, we've added an option to auto provision the Log Analytics agent to machines connected to Azure Arc. 

As with the other other auto provisioning options, this is configured at the subscription level.

When you enable this option, you'll be prompted for the workspace. 

> [!NOTE]
> For this preview, you can't select the default workspaces that was created by Defender for Cloud. To ensure you receive the full set of security features available for the Azure Arc-enabled servers, verify that you have the relevant security solution installed on the selected workspace.

:::image type="content" source="media/release-notes/auto-provisioning-agent-toggle.jpg" alt-text="Screenshot of how to auto provision the Log Analytics agent to your Azure Arc-enabled machines." lightbox="./media/release-notes/auto-provisioning-agent-toggle.jpg":::

### Deprecated the recommendation to classify sensitive data in SQL databases

We've removed the recommendation **Sensitive data in your SQL databases should be classified** as part of an overhaul of how Defender for Cloud identifies and protects sensitive date in your cloud resources.

Advance notice of this change appeared for the last six months in the [Important upcoming changes to Microsoft Defender for Cloud](upcoming-changes.md) page.

### Communication with suspicious domain alert expanded to included known Log4Shell-related domains 

The following alert was previously only available to organizations who would enabled the [Microsoft Defender for DNS](defender-for-dns-introduction.md) plan. 

With this update, the alert will also show for subscriptions with the [Microsoft Defender for servers](defender-for-servers-introduction.md) or [Defender for App Service](defender-for-app-service-introduction.md) plan enabled.

In addition, [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) has expanded the list of known malicious domains to include domains associated with exploiting the widely publicized vulnerabilities associated with Log4j.

| Alert (alert type)                                                                                                | Description                                                                                                                                                                                                                                                                                                                                                                                     | MITRE tactics  | Severity |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------:|----------|
| **Communication with suspicious domain identified by threat intelligence**<br>(AzureDNS_ThreatIntelSuspectDomain) | Communication with suspicious domain was detected by analyzing DNS transactions from your resource and comparing against known malicious domains identified by threat intelligence feeds. Communication to malicious domains is frequently performed by attackers and could imply that your resource is compromised.                                                                           | Initial Access / Persistence / Execution / Command And Control / Exploitation | Medium   |

### 'Copy alert JSON' button added to security alert details pane

To help our users quickly share an alert's details with others (for example, SOC analysts, resource owners, and developers) we've added the capability to easily extract all the details of a specific alert with one button from the security alert's details pane.

The new **Copy alert JSON** button puts the alert’s details, in JSON format, into the user's clipboard.

:::image type="content" source="media/release-notes/copy-alert-json.png" alt-text="Screenshot of the 'Copy alert JSON' button in the alert details pane." lightbox="./media/release-notes/copy-alert-json.png":::

### Renamed two recommendations

For consistency with other recommendation names, we've renamed the following two recommendations:

- Recommendation to resolve vulnerabilities discovered in running container images
    - Previous name: Vulnerabilities in running container images should be remediated (powered by Qualys)
    - New name: Running container images should have vulnerability findings resolved

- Recommendation to enable diagnostic logs for Azure App Service
    - Previous name: Diagnostic logs should be enabled in App Service
    - New name: Diagnostic logs in App Service should be enabled

### Deprecate Kubernetes cluster containers should only listen on allowed ports policy

We've deprecated the **Kubernetes cluster containers should only listen on allowed ports** recommendation.

| Policy name | Description | Effect(s) | Version |
|--|--|--|--|
| [Kubernetes cluster containers should only listen on allowed ports](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F440b515e-a580-421e-abeb-b159a61ddcbc) | Restrict containers to listen only on allowed ports to secure access to the Kubernetes cluster. This policy is generally available for Kubernetes Service (AKS), and preview for AKS Engine and Azure Arc enabled Kubernetes. For more information, see [https://aka.ms/kubepolicydoc](../governance/policy/concepts/policy-for-kubernetes.md). | audit, deny, disabled | [6.1.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Kubernetes/ContainerAllowedPorts.json) |

The **[Services should listen on allowed ports only](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/add45209-73f6-4fa5-a5a5-74a451b07fbe)** recommendation should be used to limit ports that an application exposes to the internet.

### Added 'Active Alert' workbook

To assist our users in their understanding of the active threats to their environments, and prioritize between active alerts during the remediation process, we've added the Active Alerts workbook.

:::image type="content" source="media/release-notes/active-alerts-workbook.png" alt-text="Screenshot showing the addition of the Active alerts workbook.":::

The active alerts workbook allows users to view a unified dashboard of their aggregated alerts by severity, type, tag, MITRE ATT&CK tactics, and location. Learn more in [Use the 'Active Alerts' workbook](custom-dashboards-azure-workbooks.md#use-the-active-alerts-workbook).

### 'System update' recommendation added to government cloud

The 'System updates should be installed on your machines' recommendation is now available on all government clouds.  

It's likely that this change will impact your government cloud subscription's secure score. We expect the change to lead to a decreased score, but it's possible the recommendation's inclusion might result in an increased score in some cases.

## December 2021

Updates in December include:

- [Microsoft Defender for Containers plan released for general availability (GA)](#microsoft-defender-for-containers-plan-released-for-general-availability-ga)
- [New alerts for Microsoft Defender for Storage released for general availability (GA)](#new-alerts-for-microsoft-defender-for-storage-released-for-general-availability-ga)
- [Improvements to alerts for Microsoft Defender for Storage](#improvements-to-alerts-for-microsoft-defender-for-storage)
- ['PortSweeping' alert removed from network layer alerts](#portsweeping-alert-removed-from-network-layer-alerts)

### Microsoft Defender for Containers plan released for general availability (GA)

Over two years ago, we introduced [Defender for Kubernetes](defender-for-kubernetes-introduction.md) and [Defender for container registries](defender-for-container-registries-introduction.md) as part of the Azure Defender offering within Microsoft Defender for Cloud.

With the release of [Microsoft Defender for Containers](defender-for-containers-introduction.md), we've merged these two existing Defender plans. 

The new plan:

- **Combines the features of the two existing plans** - threat detection for Kubernetes clusters and vulnerability assessment for images stored in container registries
- **Brings new and improved features** - including multi-cloud support, host level threat detection with over **sixty** new Kubernetes-aware analytics, and vulnerability assessment for running images
- **Introduces Kubernetes-native at-scale onboarding** - by default, when you enable the plan all relevant components are configured to be deployed automatically

With this release, the availability and presentation of Defender for Kubernetes and Defender for container registries has changed as follows:

- New subscriptions - The two previous container plans are no longer available
- Existing subscriptions - Wherever they appear in the Azure portal, the plans are shown as **Deprecated** with instructions for how to upgrade to the newer plan
    :::image type="content" source="media/release-notes/defender-plans-deprecated-indicator.png" alt-text="Defender for container registries and Defender for Kubernetes plans showing 'Deprecated' and upgrade information.":::

The new plan is free for the month of December 2021. For the potential changes to the billing from the old plans to Defender for Containers, and for more details on the benefits introduced with this plan, see [Introducing Microsoft Defender for Containers](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/introducing-microsoft-defender-for-containers/ba-p/2952317).

For more information, see:

- [Overview of Microsoft Defender for Containers](defender-for-containers-introduction.md)
- [Enable Microsoft Defender for Containers](defender-for-containers-enable.md)
- [Introducing Microsoft Defender for Containers - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/introducing-microsoft-defender-for-containers/ba-p/2952317)
- [Microsoft Defender for Containers | Defender for Cloud in the Field #3 - YouTube](https://www.youtube.com/watch?v=KeH0a3enLJ0&t=201s)


### New alerts for Microsoft Defender for Storage released for general availability (GA)

Threat actors use tools and scripts to scan for publicly open containers in the hope of finding misconfigured open storage containers with sensitive data.

Microsoft Defender for Storage detects these scanners so that you can block them and remediate your posture.

The preview alert that detected this was called **“Anonymous scan of public storage containers”**. To provide greater clarity about the suspicious events discovered, we've divided this into **two** new alerts. These alerts are relevant to Azure Blob Storage only.

We've improved the detection logic, updated the alert metadata, and changed the alert name and alert type.

These are the new alerts:

| Alert   (alert type)                                                                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | MITRE tactic | Severity |
|---------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|----------|
| **Publicly accessible storage containers successfully discovered**<br>(Storage.Blob_OpenContainersScanning.SuccessfulDiscovery) | A successful discovery of   publicly open storage container(s) in your storage account was performed in the last hour by a scanning script or tool.<br><br>            This usually indicates a reconnaissance attack, where the threat actor tries to list blobs by guessing container names, in the hope of finding misconfigured open storage containers with sensitive data in them.<br><br>            The threat actor may use their own script or use known scanning tools like   Microburst to scan for publicly open containers.<br><br>            ✔ Azure Blob Storage<br>      ✖ Azure Files<br>      ✖ Azure Data Lake Storage Gen2 | Collection   | Medium   |
| **Publicly accessible storage containers unsuccessfully scanned**<br>(Storage.Blob_OpenContainersScanning.FailedAttempt)        | A series of failed attempts to scan for publicly open storage containers were performed in the last hour. <br><br>This usually indicates a reconnaissance attack, where the threat actor tries to list blobs by guessing container names, in the hope of finding misconfigured open storage containers with sensitive data in them.<br><br> The threat actor may use their own script or use known scanning tools like Microburst to scan for publicly open containers.<br><br>            ✔ Azure Blob Storage<br>      ✖ Azure Files<br>      ✖ Azure Data Lake Storage Gen2                                         | Collection   | Low      |
|||

For more information, see:
- [Threat matrix for storage services](https://www.microsoft.com/security/blog/2021/04/08/threat-matrix-for-storage/)
- [Introduction to Microsoft Defender for Storage](defender-for-storage-introduction.md)
- [List of alerts provided by Microsoft Defender for Storage](alerts-reference.md#alerts-azurestorage)


### Improvements to alerts for Microsoft Defender for Storage

The initial access alerts now have improved accuracy and more data to support investigation.

Threat actors use various techniques in the initial access to gain a foothold within a network. Two of the [Microsoft Defender for Storage](defender-for-storage-introduction.md) alerts that detect behavioral anomalies in this stage now have improved detection logic and additional data to support investigations. 

If you've [configured automations](workflow-automation.md) or defined [alert suppression rules](alerts-suppression-rules.md) for these alerts in the past, update them in accordance with these changes. 

#### Detecting access from a Tor exit node

Access from a Tor exit node might indicate a threat actor trying to hide their identity. 

The alert is now tuned to generate only for authenticated access, which results in higher accuracy and confidence that the activity is malicious. This enhancement reduces the benign positive rate.  

An outlying pattern will have high severity, while less anomalous patterns will have medium severity. 
 
The alert name and description have been updated. The AlertType remains unchanged. 

- Alert name (old): Access from a Tor exit node to a storage account 
- Alert name (new): Authenticated access from a Tor exit node 
- Alert types:  Storage.Blob_TorAnomaly / Storage.Files_TorAnomaly
- Description: One or more storage container(s) / file share(s) in your storage account were successfully accessed from an IP address known to be an active exit node of Tor (an anonymizing proxy). Threat actors use Tor to make it difficult to trace the activity back to them. Authenticated access from a Tor exit node is a likely indication that a threat actor is trying to hide their identity. Applies to: Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen2
- MITRE tactic: Initial access
- Severity: High/Medium

#### Unusual unauthenticated access 

A change in access patterns may indicate that a threat actor was able to exploit public read access to storage containers, either by exploiting a mistake in access configurations, or by changing the access permissions. 

This medium severity alert is now tuned with improved behavioral logic, higher accuracy, and confidence that the activity is malicious. This enhancement reduces the benign positive rate. 

The alert name and description have been updated. The AlertType remains unchanged. 
 
- Alert name (old): Anonymous access to a storage account 
- Alert name (new): Unusual unauthenticated access to a storage container
- Alert types: Storage.Blob_AnonymousAccessAnomaly
- Description: This storage account was accessed without authentication, which is a change in the common access pattern. Read access to this container is usually authenticated. This might indicate that a threat actor was able to exploit public read access to storage container(s) in this storage account(s). Applies to: Azure Blob Storage
- MITRE tactic: Collection 
- Severity: Medium

For more information, see: 

- [Threat matrix for storage services](https://www.microsoft.com/security/blog/2021/04/08/threat-matrix-for-storage/)
- [Introduction to Microsoft Defender for Storage](defender-for-storage-introduction.md)
- [List of alerts provided by Microsoft Defender for Storage](alerts-reference.md#alerts-azurestorage)


### 'PortSweeping' alert removed from network layer alerts

The following alert was removed from our network layer alerts due to inefficiencies:

| Alert (alert type)                                                                                                                                               | Description     | MITRE tactics | Severity      |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------:|---------------|
| **Possible outgoing port scanning activity detected**<br>(PortSweeping) | Network traffic analysis detected suspicious outgoing traffic from %{Compromised Host}. This traffic may be a result of a port scanning activity. When the compromised resource is a load balancer or an application gateway, the suspected outgoing traffic has been originated from to one or more of the resources in the backend pool (of the load balancer or application gateway). If this behavior is intentional, please note that performing port scanning is against Azure Terms of service. If this behavior is unintentional, it may mean your resource has been compromised. | Discovery | Medium   |
||||



## November 2021

Our Ignite release includes:

- [Azure Security Center and Azure Defender become Microsoft Defender for Cloud](#azure-security-center-and-azure-defender-become-microsoft-defender-for-cloud)
- [Native CSPM for AWS and threat protection for Amazon EKS, and AWS EC2](#native-cspm-for-aws-and-threat-protection-for-amazon-eks-and-aws-ec2)
- [Prioritize security actions by data sensitivity (powered by Azure Purview) (in preview)](#prioritize-security-actions-by-data-sensitivity-powered-by-azure-purview-in-preview)
- [Expanded security control assessments with Azure Security Benchmark v3](#expanded-security-control-assessments-with-azure-security-benchmark-v3)
- [Microsoft Sentinel connector's optional bi-directional alert synchronization released for general availability (GA)](#microsoft-sentinel-connectors-optional-bi-directional-alert-synchronization-released-for-general-availability-ga)
- [New recommendation to push Azure Kubernetes Service (AKS) logs to Sentinel](#new-recommendation-to-push-azure-kubernetes-service-aks-logs-to-sentinel)
- [Recommendations mapped to the MITRE ATT&CK® framework - released for general availability (GA)](#recommendations-mapped-to-the-mitre-attck-framework---released-for-general-availability-ga)

Other changes in November include:

- [Microsoft Threat and Vulnerability Management added as vulnerability assessment solution - released for general availability (GA)](#microsoft-threat-and-vulnerability-management-added-as-vulnerability-assessment-solution---released-for-general-availability-ga)
- [Microsoft Defender for Endpoint for Linux now supported by Microsoft Defender for servers - released for general availability (GA)](#microsoft-defender-for-endpoint-for-linux-now-supported-by-microsoft-defender-for-servers---released-for-general-availability-ga)
- [Snapshot export for recommendations and security findings (in preview)](#snapshot-export-for-recommendations-and-security-findings-in-preview)
- [Auto provisioning of vulnerability assessment solutions released for general availability (GA)](#auto-provisioning-of-vulnerability-assessment-solutions-released-for-general-availability-ga)
- [Software inventory filters in asset inventory released for general availability (GA)](#software-inventory-filters-in-asset-inventory-released-for-general-availability-ga)
- [New AKS security policy added to default initiative – for use by private preview customers only](#new-aks-security-policy-added-to-default-initiative--for-use-by-private-preview-customers-only)
- [Inventory display of on-premises machines applies different template for resource name](#inventory-display-of-on-premises-machines-applies-different-template-for-resource-name)


### Azure Security Center and Azure Defender become Microsoft Defender for Cloud

According to the [2021 State of the Cloud report](https://info.flexera.com/CM-REPORT-State-of-the-Cloud#download), 92% of organizations now have a multi-cloud strategy. At Microsoft, our goal is to centralize security across these environments and help security teams work more effectively.

**Microsoft Defender for Cloud** (formerly known as Azure Security Center and Azure Defender) is a Cloud Security Posture Management (CSPM) and cloud workload protection (CWP) solution that discovers weaknesses across your cloud configuration, helps strengthen the overall security posture of your environment, and protects workloads across multi-cloud and hybrid environments.

At Ignite 2019, we shared our vision to create the most complete approach for securing your digital estate and integrating XDR technologies under the Microsoft Defender brand. Unifying Azure Security Center and Azure Defender under the new name **Microsoft Defender for Cloud**, reflects the integrated capabilities of our security offering and our ability to support any cloud platform.


### Native CSPM for AWS and threat protection for Amazon EKS, and AWS EC2

A new **environment settings** page provides greater visibility and control over your management groups, subscriptions, and AWS accounts. The page is designed to onboard AWS accounts at scale: connect your AWS **management account**, and you'll automatically onboard existing and future accounts. 

:::image type="content" source="media/release-notes/add-aws-account.png" alt-text="Use the new environment settings page to connect your AWS accounts.":::

When you've added your AWS accounts, Defender for Cloud protects your AWS resources with any or all of the following plans:

- **Defender for Cloud's CSPM features** extend to your AWS resources. This agentless plan assesses your AWS resources according to AWS-specific security recommendations and these are included in your secure score. The resources will also be assessed for compliance with built-in standards specific to AWS (AWS CIS, AWS PCI DSS, and AWS Foundational Security Best Practices). Defender for Cloud's [asset inventory page](asset-inventory.md) is a multi-cloud enabled feature helping you manage your AWS resources alongside your Azure resources.
- **Microsoft Defender for Kubernetes** extends its container threat detection and advanced defenses to your **Amazon EKS Linux clusters**.
- **Microsoft Defender for servers** brings threat detection and advanced defenses to your Windows and Linux EC2 instances. This plan includes the integrated license for Microsoft Defender for Endpoint, security baselines and OS level assessments, vulnerability assessment scanning, adaptive application controls (AAC), file integrity monitoring (FIM), and more.

Learn more about [connecting your AWS accounts to Microsoft Defender for Cloud](quickstart-onboard-aws.md).


### Prioritize security actions by data sensitivity (powered by Azure Purview) (in preview)
Data resources remain a popular target for threat actors. So it's crucial for security teams to identify, prioritize, and secure sensitive data resources across their cloud environments.

To address this challenge, Microsoft Defender for Cloud now integrates sensitivity information from [Azure Purview](../purview/overview.md). Azure Purview is a unified data governance service that provides rich insights into the sensitivity of your data within multi-cloud, and on-premises workloads.

The integration with Azure Purview extends your security visibility in Defender for Cloud from the infrastructure level down to the data, enabling an entirely new way to prioritize resources and security activities for your security teams.

Learn more in [Prioritize security actions by data sensitivity](information-protection.md).


### Expanded security control assessments with Azure Security Benchmark v3
Microsoft Defender for Cloud's security recommendations are enabled and supported by the Azure Security Benchmark. 

[Azure Security Benchmark](../security/benchmarks/introduction.md) is the Microsoft-authored, Azure-specific set of guidelines for security and compliance best practices based on common compliance frameworks. This widely respected benchmark builds on the controls from the [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) and the [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) with a focus on cloud-centric security.

From Ignite 2021, Azure Security Benchmark **v3** is available in [Defender for Cloud's regulatory compliance dashboard](update-regulatory-compliance-packages.md) and enabled as the new default initiative for all Azure subscriptions protected with Microsoft
Defender for Cloud. 

Enhancements for v3 include: 

- Additional mappings to industry frameworks [PCI-DSS v3.2.1](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3-2-1.pdf) and [CIS Controls v8](https://www.cisecurity.org/controls/v8/).
- More granular and actionable guidance for controls with the introduction of:
    - **Security Principles** - Providing insight into the overall security objectives that build the foundation for our recommendations.
    - **Azure Guidance** - The technical “how-to” for meeting these objectives.

- New controls include DevOps security for issues such as threat modeling and software supply chain security, as well as key and certificate management for best practices in Azure.

Learn more in [Introduction to Azure Security Benchmark](/security/benchmark/azure/introduction).


### Microsoft Sentinel connector's optional bi-directional alert synchronization released for general availability (GA)

In July, [we announced](release-notes-archive.md#azure-sentinel-connector-now-includes-optional-bi-directional-alert-synchronization-in-preview) a preview feature, **bi-directional alert synchronization**, for the built-in connector in [Microsoft Sentinel](../sentinel/index.yml) (Microsoft's cloud-native SIEM and SOAR solution). This feature is now released for general availability (GA).

When you connect Microsoft Defender for Cloud to Microsoft Sentinel, the status of security alerts is synchronized between the two services. So, for example, when an alert is closed in Defender for Cloud, that alert will display as closed in Microsoft Sentinel as well. Changing the status of an alert in Defender for Cloud won't affect the status of any Microsoft Sentinel **incidents** that contain the synchronized Microsoft Sentinel alert, only that of the synchronized alert itself.

When you enable **bi-directional alert synchronization** you'll automatically sync the status of the original Defender for Cloud alerts with Microsoft Sentinel incidents that contain the copies of those Defender for Cloud alerts. So, for example, when a Microsoft Sentinel incident containing a Defender for Cloud alert is closed, Defender for Cloud will automatically close the corresponding original alert.

Learn more in [Connect Azure Defender alerts from Azure Security Center](../sentinel/connect-azure-security-center.md) and [Stream alerts to Azure Sentinel](export-to-siem.md#stream-alerts-to-microsoft-sentinel).

### New recommendation to push Azure Kubernetes Service (AKS) logs to Sentinel

In a further enhancement to the combined value of Defender for Cloud and Microsoft Sentinel, we'll now highlight Azure Kubernetes Service instances that aren't sending log data to Microsoft Sentinel.

SecOps teams can choose the relevant Microsoft Sentinel workspace directly from the recommendation details page and immediately enable the streaming of raw logs. This seamless connection between the two products makes it easy for security teams to ensure complete logging coverage across their workloads to stay on top of their entire environment.

The new recommendation, "Diagnostic logs in Kubernetes services should be enabled" includes the 'Fix' option for faster remediation.

We've also enhanced the "Auditing on SQL server should be enabled" recommendation with the same Sentinel streaming capabilities. 


### Recommendations mapped to the MITRE ATT&CK® framework - released for general availability (GA)

We've enhanced Defender for Cloud's security recommendations to show their position on the MITRE ATT&CK® framework. This globally accessible knowledge base of threat actors' tactics and techniques based on real-world observations, provides more context to help you understand the associated risks of the recommendations for your environment.

You'll find these tactics wherever you access recommendation information:

- **Azure Resource Graph query results** for relevant recommendations include the MITRE ATT&CK® tactics and techniques.

- **Recommendation details pages** show the mapping for all relevant recommendations:

    :::image type="content" source="media/review-security-recommendations/tactics-window.png" alt-text="Screenshot of the MITRE tactics mapping for a recommendation.":::

- **The recommendations page in Defender for Cloud** has a new :::image type="icon" source="media/review-security-recommendations/tactics-filter-recommendations-page.png" border="false"::: filter to select recommendations according to their associated tactic:

Learn more in [Review your security recommendations](review-security-recommendations.md).

### Microsoft Threat and Vulnerability Management added as vulnerability assessment solution - released for general availability (GA)

In October, [we announced](#microsoft-threat-and-vulnerability-management-added-as-vulnerability-assessment-solution-in-preview) an extension to the integration between [Microsoft Defender for servers](defender-for-servers-introduction.md) and Microsoft Defender for Endpoint, to support a new vulnerability assessment provider for your machines: [Microsoft threat and vulnerability management](/microsoft-365/security/defender-endpoint/next-gen-threat-and-vuln-mgt). This feature is now released for general availability (GA).

Use **threat and vulnerability management** to discover vulnerabilities and misconfigurations in near real time with the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md) enabled, and without the need for additional agents or periodic scans. Threat and vulnerability management prioritizes vulnerabilities based on the threat landscape and detections in your organization.

Use the security recommendation "[A vulnerability assessment solution should be enabled on your virtual machines](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/ffff0522-1e88-47fc-8382-2a80ba848f5d)" to surface the vulnerabilities detected by threat and vulnerability management for your [supported machines](/microsoft-365/security/defender-endpoint/tvm-supported-os?view=o365-worldwide&preserve-view=true). 

To automatically surface the vulnerabilities, on existing and new machines, without the need to manually remediate the recommendation, see [Vulnerability assessment solutions can now be auto enabled (in preview)](#vulnerability-assessment-solutions-can-now-be-auto-enabled-in-preview).

Learn more in [Investigate weaknesses with Microsoft Defender for Endpoint's threat and vulnerability management](deploy-vulnerability-assessment-tvm.md).

### Microsoft Defender for Endpoint for Linux now supported by Microsoft Defender for servers - released for general availability (GA)

In August, [we announced](release-notes-archive.md#microsoft-defender-for-endpoint-for-linux-now-supported-by-azure-defender-for-servers-in-preview) preview support for deploying the [Defender for Endpoint for Linux](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux) sensor to supported Linux machines. This feature is now released for general availability (GA).

[Microsoft Defender for servers](defender-for-servers-introduction.md) includes an integrated license for [Microsoft Defender for Endpoint](https://www.microsoft.com/microsoft-365/security/endpoint-defender). Together, they provide comprehensive endpoint detection and response (EDR) capabilities.

When Defender for Endpoint detects a threat, it triggers an alert. The alert is shown in Defender for Cloud. From Defender for Cloud, you can also pivot to the Defender for Endpoint console, and perform a detailed investigation to uncover the scope of the attack.

Learn more in [Protect your endpoints with Security Center's integrated EDR solution: Microsoft Defender for Endpoint](integration-defender-for-endpoint.md).


### Snapshot export for recommendations and security findings (in preview)

Defender for Cloud generates detailed security alerts and recommendations. You can view them in the portal or through programmatic tools. You might also need to export some or all of this information for tracking with other monitoring tools in your environment.

Defender for Cloud's **continuous export** feature lets you fully customize *what* will be exported, and *where* it will go. Learn more in [Continuously export Microsoft Defender for Cloud data](continuous-export.md).

Even though the feature is called *continuous*, there's also an option to export weekly snapshots. Until now, these weekly snapshots were limited to secure score and regulatory compliance data. We've  added the capability to export recommendations and security findings.

### Auto provisioning of vulnerability assessment solutions released for general availability (GA)

In October, [we announced](#vulnerability-assessment-solutions-can-now-be-auto-enabled-in-preview) the addition of vulnerability assessment solutions to Defender for Cloud's auto provisioning page. This is relevant to Azure virtual machines and Azure Arc machines on subscriptions protected by [Azure Defender for servers](defender-for-servers-introduction.md). This feature is now released for general availability (GA).

If the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md) is enabled, Defender for Cloud presents a choice of vulnerability assessment solutions:

- (**NEW**) The Microsoft threat and vulnerability management module of Microsoft Defender for Endpoint (see [the release note](#microsoft-threat-and-vulnerability-management-added-as-vulnerability-assessment-solution---released-for-general-availability-ga))
- The integrated Qualys agent

Your chosen solution will be automatically enabled on supported machines.

Learn more in [Automatically configure vulnerability assessment for your machines](auto-deploy-vulnerability-assessment.md).

### Software inventory filters in asset inventory released for general availability (GA)

In October, [we announced](#software-inventory-filters-added-to-asset-inventory-in-preview) new filters for the [asset inventory](asset-inventory.md) page to select machines running specific software - and even specify the versions of interest. This feature is now released for general availability (GA).

You can query the software inventory data in **Azure Resource Graph Explorer**.

To use these features, you'll need to enable the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md). 

For full details, including sample Kusto queries for Azure Resource Graph, see [Access a software inventory](asset-inventory.md#access-a-software-inventory).

### New AKS security policy added to default initiative – for use by private preview customers only

To ensure that Kubernetes workloads are secure by default, Defender for Cloud includes Kubernetes level policies and hardening recommendations, including enforcement options with Kubernetes admission control.

As part of this project, we've added a policy and recommendation (disabled by default) for gating deployment on Kubernetes clusters. The policy is in the default initiative but is only relevant for organizations who register for the related private preview.

You can safely ignore the policies and recommendation ("Kubernetes clusters should gate deployment of vulnerable images") and there will be no impact on your environment. 

If you'd like to participate in the private preview, you'll need to be a member of the private preview ring. If you're not already a member, submit a request [here](https://aka.ms/atscale). Members will be notified when the preview begins.

### Inventory display of on-premises machines applies different template for resource name

To improve the presentation of resources in the [Asset inventory](asset-inventory.md), we've removed the "source-computer-IP" element from the template for naming on-premises machines.

- **Previous format:** ``machine-name_source-computer-id_VMUUID``
- **From this update:** ``machine-name_VMUUID``

## October 2021

Updates in October include:

- [Microsoft Threat and Vulnerability Management added as vulnerability assessment solution (in preview)](#microsoft-threat-and-vulnerability-management-added-as-vulnerability-assessment-solution-in-preview)
- [Vulnerability assessment solutions can now be auto enabled (in preview)](#vulnerability-assessment-solutions-can-now-be-auto-enabled-in-preview)
- [Software inventory filters added to asset inventory (in preview)](#software-inventory-filters-added-to-asset-inventory-in-preview)
- [Changed prefix of some alert types from "ARM_" to "VM_"](#changed-prefix-of-some-alert-types-from-arm_-to-vm_)
- [Changes to the logic of a security recommendation for Kubernetes clusters](#changes-to-the-logic-of-a-security-recommendation-for-kubernetes-clusters)
- [Recommendations details pages now show related recommendations](#recommendations-details-pages-now-show-related-recommendations)
- [New alerts for Azure Defender for Kubernetes (in preview)](#new-alerts-for-azure-defender-for-kubernetes-in-preview)


### Microsoft Threat and Vulnerability Management added as vulnerability assessment solution (in preview)

We've extended the integration between [Azure Defender for servers](defender-for-servers-introduction.md) and Microsoft Defender for Endpoint, to support a new vulnerability assessment provider for your machines: [Microsoft threat and vulnerability management](/microsoft-365/security/defender-endpoint/next-gen-threat-and-vuln-mgt). 

Use **threat and vulnerability management** to discover vulnerabilities and misconfigurations in near real time with the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md) enabled, and without the need for additional agents or periodic scans. Threat and vulnerability management prioritizes vulnerabilities based on the threat landscape and detections in your organization.

Use the security recommendation "[A vulnerability assessment solution should be enabled on your virtual machines](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/ffff0522-1e88-47fc-8382-2a80ba848f5d)" to surface the vulnerabilities detected by threat and vulnerability management for your [supported machines](/microsoft-365/security/defender-endpoint/tvm-supported-os?view=o365-worldwide&preserve-view=true). 

To automatically surface the vulnerabilities, on existing and new machines, without the need to manually remediate the recommendation, see [Vulnerability assessment solutions can now be auto enabled (in preview)](#vulnerability-assessment-solutions-can-now-be-auto-enabled-in-preview).

Learn more in [Investigate weaknesses with Microsoft Defender for Endpoint's threat and vulnerability management](deploy-vulnerability-assessment-tvm.md).

### Vulnerability assessment solutions can now be auto enabled (in preview)

Security Center's auto provisioning page now includes the option to automatically enable a vulnerability assessment solution to Azure virtual machines and Azure Arc machines on subscriptions protected by [Azure Defender for servers](defender-for-servers-introduction.md).

If the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md) is enabled, Defender for Cloud presents a choice of vulnerability assessment solutions:

- (**NEW**) The Microsoft threat and vulnerability management module of Microsoft Defender for Endpoint (see [the release note](#microsoft-threat-and-vulnerability-management-added-as-vulnerability-assessment-solution-in-preview))
- The integrated Qualys agent

:::image type="content" source="media/deploy-vulnerability-assessment-tvm/auto-provision-vulnerability-assessment-agent.png" alt-text="Configure auto provisioning of Microsoft's threat and vulnerability management from Azure Security Center.":::

Your chosen solution will be automatically enabled on supported machines.

Learn more in [Automatically configure vulnerability assessment for your machines](auto-deploy-vulnerability-assessment.md).

### Software inventory filters added to asset inventory (in preview)

The [asset inventory](asset-inventory.md) page now includes a filter to select machines running specific software - and even specify the versions of interest. 

Additionally, you can query the software inventory data in **Azure Resource Graph Explorer**.

To use these new features, you'll need to enable the [integration with Microsoft Defender for Endpoint](integration-defender-for-endpoint.md). 

For full details, including sample Kusto queries for Azure Resource Graph, see [Access a software inventory](asset-inventory.md#access-a-software-inventory).

:::image type="content" source="media/deploy-vulnerability-assessment-tvm/software-inventory.png" alt-text="If you've enabled the threat and vulnerability solution, Security Center's asset inventory offers a filter to select resources by their installed software.":::

### Changed prefix of some alert types from "ARM_" to "VM_" 

In July 2021, we announced a [logical reorganization of Azure Defender for Resource Manager alerts](release-notes-archive.md#logical-reorganization-of-azure-defender-for-resource-manager-alerts) 

As part of a logical reorganization of some of the Azure Defender plans, we moved twenty-one alerts from [Azure Defender for Resource Manager](defender-for-resource-manager-introduction.md) to [Azure Defender for servers](defender-for-servers-introduction.md).

With this update, we've changed the prefixes of these alerts to match this reassignment and replaced "ARM_" with "VM_" as shown in the following table:

| Original name                                  | From this change                              |
|------------------------------------------------|-----------------------------------------------|
| ARM_AmBroadFilesExclusion                      | VM_AmBroadFilesExclusion                      |
| ARM_AmDisablementAndCodeExecution              | VM_AmDisablementAndCodeExecution              |
| ARM_AmDisablement                              | VM_AmDisablement                              |
| ARM_AmFileExclusionAndCodeExecution            | VM_AmFileExclusionAndCodeExecution            |
| ARM_AmTempFileExclusionAndCodeExecution        | VM_AmTempFileExclusionAndCodeExecution        |
| ARM_AmTempFileExclusion                        | VM_AmTempFileExclusion                        |
| ARM_AmRealtimeProtectionDisabled               | VM_AmRealtimeProtectionDisabled               |
| ARM_AmTempRealtimeProtectionDisablement        | VM_AmTempRealtimeProtectionDisablement        |
| ARM_AmRealtimeProtectionDisablementAndCodeExec | VM_AmRealtimeProtectionDisablementAndCodeExec |
| ARM_AmMalwareCampaignRelatedExclusion          | VM_AmMalwareCampaignRelatedExclusion          |
| ARM_AmTemporarilyDisablement                   | VM_AmTemporarilyDisablement                   |
| ARM_UnusualAmFileExclusion                     | VM_UnusualAmFileExclusion                     |
| ARM_CustomScriptExtensionSuspiciousCmd         | VM_CustomScriptExtensionSuspiciousCmd         |
| ARM_CustomScriptExtensionSuspiciousEntryPoint  | VM_CustomScriptExtensionSuspiciousEntryPoint  |
| ARM_CustomScriptExtensionSuspiciousPayload     | VM_CustomScriptExtensionSuspiciousPayload     |
| ARM_CustomScriptExtensionSuspiciousFailure     | VM_CustomScriptExtensionSuspiciousFailure     |
| ARM_CustomScriptExtensionUnusualDeletion       | VM_CustomScriptExtensionUnusualDeletion       |
| ARM_CustomScriptExtensionUnusualExecution      | VM_CustomScriptExtensionUnusualExecution      |
| ARM_VMAccessUnusualConfigReset                 | VM_VMAccessUnusualConfigReset                 |
| ARM_VMAccessUnusualPasswordReset               | VM_VMAccessUnusualPasswordReset               |
| ARM_VMAccessUnusualSSHReset                    | VM_VMAccessUnusualSSHReset                    |
|||

Learn more about the [Azure Defender for Resource Manager](defender-for-resource-manager-introduction.md) and [Azure Defender for servers](defender-for-servers-introduction.md) plans.

### Changes to the logic of a security recommendation for Kubernetes clusters

The recommendation "Kubernetes clusters should not use the default namespace" prevents usage of the default namespace for a range of resource types. Two of the resource types that were included in this recommendation have been removed: ConfigMap and Secret. 

Learn more about this recommendation and hardening your Kubernetes clusters in [Understand Azure Policy for Kubernetes clusters](../governance/policy/concepts/policy-for-kubernetes.md).

### Recommendations details pages now show related recommendations

To clarify the relationships between different recommendations, we've added a **Related recommendations** area to the details pages of many recommendations. 

The three relationship types that are shown on these pages are:

- **Prerequisite** - A recommendation that must be completed before the selected recommendation
- **Alternative** - A different recommendation which provides another way of achieving the goals of the selected recommendation
- **Dependent** - A recommendation for which the selected recommendation is a prerequisite

For each related recommendation, the number of unhealthy resources is shown in the "Affected resources" column.

> [!TIP]
> If a related recommendation is grayed out, its dependency isn't yet completed and so isn't available.

An example of related recommendations:

1. Security Center checks your machines for supported vulnerability assessment solutions:<br>
    **A vulnerability assessment solution should be enabled on your virtual machines**

1. If one is found, you'll get notified about discovered vulnerabilities:<br>
    **Vulnerabilities in your virtual machines should be remediated**

Obviously, Security Center can't notify you about discovered vulnerabilities unless it finds a supported vulnerability assessment solution.

Therefore:

 - Recommendation #1 is a prerequisite for recommendation #2
 - Recommendation #2 depends upon recommendation #1

:::image type="content" source="media/release-notes/related-recommendations-solution-not-found.png" alt-text="Screenshot of recommendation to deploy vulnerability assessment solution.":::

:::image type="content" source="media/release-notes/related-recommendations-vulnerabilities-found.png" alt-text="Screenshot of recommendation to resolve discovered vulnerabilities.":::



### New alerts for Azure Defender for Kubernetes (in preview)

To expand the threat protections provided by Azure Defender for Kubernetes, we've added two preview alerts.

These alerts are generated based on a new machine learning model and Kubernetes advanced analytics, measuring multiple deployment and role assignment attributes against previous activities in the cluster and across all clusters monitored by Azure Defender.

| Alert (alert type)                                                                 | Description                                                                                                                                                                                                                                                                                                                                                      | MITRE tactic | Severity |
|------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------:|----------|
| **Anomalous pod deployment (Preview)**<br>(K8S_AnomalousPodDeployment)             | Kubernetes audit log analysis detected pod deployment that is anomalous based on previous pod deployment activity. This activity is considered an anomaly when taking into account how the different features seen in the deployment operation are in relations to one another. The features monitored by this analytics include the container image registry used, the account performing the deployment, day of the week, how often does this account performs pod deployments, user agent used in the operation, is this a namespace which is pod deployment occur to often, or other feature. Top contributing reasons for raising this alert as anomalous activity are detailed under the alert extended properties. | Execution | Medium |
| **Excessive role permissions assigned in Kubernetes cluster (Preview)**<br>(K8S_ServiceAcountPermissionAnomaly) | Analysis of the Kubernetes audit logs detected an excessive permissions role assignment to your cluster. From examining role assignments, the listed permissions are uncommon to the specific service account. This detection considers previous role assignments to the same service account across clusters monitored by Azure, volume per permission, and the impact of the specific permission. The anomaly detection model used for this alert takes into account how this permission is used across all clusters monitored by Azure Defender. | Privilege Escalation | Low |
|||

For a full list of the Kubernetes alerts, see [Alerts for Kubernetes clusters](alerts-reference.md#alerts-k8scluster).
