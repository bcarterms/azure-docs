---
title: FAQs about Azure Health Data Services
description: This document provides answers to the frequently asked questions about Azure Health Data Services.
services: healthcare-apis
author: ginalee-dotcom
ms.custom: references_regions
ms.service: healthcare-apis
ms.topic: reference
ms.date: 03/02/2022
ms.author: ginle
---

# Frequently asked questions about Azure Health Data Services

These are some of the frequently asked questions for the Azure Health Data Services.

## Azure Health Data Services: The basics

### What is Azure Health Data Services?
Azure Health Data Services is a fully managed health data platform that enables the rapid exchange and persistence of Protected Health Information (PHI) and health data through interoperable open industry standards like Fast Healthcare Interoperability Resources (FHIR®) and Digital Imaging and Communications in Medicine (DICOM®).

### What does Azure Health Data Services enable you to do?
Azure Health Data Services enables you to: 

* Quickly connect disparate health data sources and formats such as structured, imaging, and device data and normalize it to be persisted in the cloud.

* Transform and ingest data into FHIR. For example, you can transform health data from legacy formats, such as HL7v2 or CDA, or from high frequency IoT data in device proprietary formats to FHIR.

* Connect your data stored in Healthcare APIs with services across the Azure ecosystem, like Synapse, and products across Microsoft, like Teams, to derive new insights through analytics and machine learning and to enable new workflows as well as connection to SMART on FHIR applications.

* Manage advanced workloads with enterprise features that offer reliability, scalability, and security to ensure that your data is protected, meets privacy and compliance certifications required for the healthcare industry.

### Can I migrate my existing production workload from Azure API for FHIR to Azure Health Data Services?
No, unfortunately we don't offer migration capabilities at this time. 

### What is the pricing of Azure Health Data Services?
At this time, Azure Health Data Services is available for you to use at no charge. 

### What regions are Azure Health Data Services available?
Refer to the [Products by region](https://azure.microsoft.com/global-infrastructure/services/?products=azure-api-for-fhir) page for the most current information. 
          
### What are the subscription quota limits for Azure Health Data Services?
For more information, see [Azure Health Data Services service limits](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-healthcare-apis) for the most current information.

### What is the backup and recovery policy for Azure Health Data Services?
Data for the managed service is automatically backed up every 12 hours, and the backups are kept for seven days. Data can be restored by the support team. Customers can make a request to restore the data, or change the default data backup policy, through a support ticket.

## More frequently asked questions
[FAQs about Azure Health Data Services FHIR service](./fhir/fhir-faq.md)

[FAQs about Azure Health Data Services DICOM service](./dicom/dicom-services-faqs.yml)

[FAQs about Azure Health Data Services IoT connector](./iot/iot-connector-faqs.md)

(FHIR&#174;) is a registered trademark of [HL7](https://hl7.org/fhir/) and is used with the permission of HL7.