---
title: Troubleshoot device connections to Azure IoT Central | Microsoft Docs
description: Troubleshoot why you're not seeing data from your devices in IoT Central
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 12/21/2021
ms.topic: troubleshooting
ms.service: iot-central
ms.custom: device-developer, devx-track-azurecli

#Customer intent: As a device developer, I want to understand why data from my devices isn't showing up in IoT Central, and the steps I can take to rectify the issue.
---

# Troubleshoot why data from your devices isn't showing up in Azure IoT Central

This document helps you find out why the data your devices are sending to IoT Central may not be showing up in the application.

There are two main areas to investigate:

- Device connectivity issues
  - Authentication issues such as the device has invalid credentials
  - Network connectivity issues
  - Device isn't approved, or blocked
- Device payload shape issues

This troubleshooting guide focuses on device connectivity issues and device payload shape issues.

## Device connectivity issues

This section helps you determine if your data is reaching IoT Central.

If you haven't already done so, install the `az cli` tool and `azure-iot` extension.

To learn how to install the `az cli`, see [Install the Azure CLI](/cli/azure/install-azure-cli).

To [install](/cli/azure/azure-cli-reference-for-IoT#extension-reference-installation) the `azure-iot` extension, run the following command:

```azurecli
az extension add --name azure-iot
```

> [!NOTE]
> You may be prompted to install the `uamqp` library the first time you run an extension command.

When you've installed the `azure-iot` extension, start your device to see if the messages it's sending are making their way to IoT Central.

Use the following commands to sign in the subscription where you have your IoT Central application:

```azurecli
az login
az account set --subscription <your-subscription-id>
```

To monitor the telemetry your device is sending, use the following command:

```azurecli
az iot central diagnostics monitor-events --app-id <app-id> --device-id <device-name>
```

If the device has connected successfully to IoT Central, you see output similar to the following:

```output
Monitoring telemetry.
Filtering on device: device-001
{
    "event": {
        "origin": "device-001",
        "module": "",
        "interface": "",
        "component": "",
        "payload": {
            "temp": 65.57910343679293,
            "humid": 36.16224660107426
        }
    }
}
```

To monitor the property updates your device is exchanging with IoT Central, use the following preview command:

```azurecli
az iot central diagnostics monitor-properties --app-id <app-id> --device-id <device-name>
```

If the device successfully sends property updates, you see output similar to the following:

```output
Changes in reported properties:
version : 32
{'state': 'true', 'name': {'value': {'value': 'Contoso'}, 'status': 'completed', 'desiredVersion': 7, 'ad': 'completed', 'av': 7, 'ac
': 200}, 'brightness': {'value': {'value': 2}, 'status': 'completed', 'desiredVersion': 7, 'ad': 'completed', 'av': 7, 'ac': 200}, 'p
rocessorArchitecture': 'ARM', 'swVersion': '1.0.0'}
```

If you see data appear in your terminal, then the data is making it as far as your IoT Central application.

If you don't see any data appear after a few minutes, try pressing the `Enter` or `return` key on your keyboard, in case the output is stuck.

If you're still not seeing any data appear on your terminal, it's likely that your device is having network connectivity issues, or is not sending data correctly to IoT Central.

### Check the provisioning status of your device

If your data is not appearing on the monitor, check the provisioning status of your device by running the following command:

```azurecli
az iot central device registration-info --app-id <app-id> --device-id <device-name>
```

The following output shows an example of a device that's blocked from connecting:

```json
{
  "@device_id": "v22upeoqx6",
  "device_registration_info": {
    "device_status": "blocked",
    "display_name": "Environmental Sensor - v22upeoqx6",
    "id": "v22upeoqx6",
    "instance_of": "urn:krhsi_k0u:modelDefinition:w53jukkazs",
    "simulated": false
  },
  "dps_state": {
    "error": "Device is blocked from connecting to IoT Central application. Unblock the device in IoT Central and retry. Learn more:
https://aka.ms/iotcentral-docs-dps-SAS",
    "status": null
  }
}
```

| Device provisioning status | Description | Possible mitigation |
| - | - | - |
| Provisioned | No immediately recognizable issue. | N/A |
| Registered | The device has not yet connected to IoT Central. | Check your device logs for connectivity issues. |
| Blocked | The device is blocked from connecting to IoT Central. | Device is blocked from connecting to the IoT Central application. Unblock the device in IoT Central and retry. To learn more, see [Device status values](howto-manage-devices-individually.md#device-status-values). |
| Unapproved | The device is not approved. | Device isn't approved to connect to the IoT Central application. Approve the device in IoT Central and retry. To learn more, see [Device status values](howto-manage-devices-individually.md#device-status-values) |
| Unassigned | The device is not assigned to a device template. | Assign the device to a device template so that IoT Central knows how to parse the data. |

Learn more about [Device status values](howto-manage-devices-individually.md#device-status-values).

### Error codes

If you're still unable to diagnose why your data isn't showing up in `monitor-events`, the next step is to look for error codes reported by your device.

Start a debugging session on your device, or collect logs from your device. Check for any error codes that the device reports.

The following tables show the common error codes and possible actions to mitigate.

If you are seeing issues related to your authentication flow:

| Error code | Description | Possible Mitigation |
| - | - | - |
| 400 | The body of the request is not valid. For example, it cannot be parsed, or the object cannot be validated. | Ensure that you're sending the correct request body as part of the attestation flow, or use a device SDK. |
| 401 | The authorization token cannot be validated. For example, it has expired or doesn't apply to the request's URI. This error code is also returned to devices as part of the TPM attestation flow. | Ensure that your device has the correct credentials. |
| 404 | The Device Provisioning Service instance, or a resource such as an enrollment doesn't exist. | [File a ticket with customer support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). |
| 412 | The `ETag` in the request doesn't match the `ETag` of the existing resource, as per RFC7232. | [File a ticket with customer support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). |
| 429 | Operations are being throttled by the service. For specific service limits, see [IoT Hub Device Provisioning Service limits](../../azure-resource-manager/management/azure-subscription-service-limits.md#iot-hub-device-provisioning-service-limits). | Reduce message frequency, split responsibilities among more devices. |
| 500 | An internal error occurred. | [File a ticket with customer support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) to see if they can help you further. |

### Detailed authorization error codes

| Error | Sub error code | Notes |
| - | - | - |
| 401 Unauthorized | 401002 | The device is using invalid or expired credentials. This error is reported by DPS. |
| 401 Unauthorized | 400209 | The device is either waiting for approval by an operator or has been blocked by an operator. |
| 401 IoTHubUnauthorized |  | The device is using expired security token. This error is reported by IoT Hub. |
| 401 IoTHubUnauthorized | DEVICE_DISABLED | The device is disabled in this IoT hub and has moved to another IoT hub. Re-provision the device. |
| 401 IoTHubUnauthorized | DEVICE_BLOCKED | An operator has blocked this device. |



### File upload error codes

Here is a list of common error codes you might see when a device tries to upload a file to the cloud. Remember that before your device can upload a file, you must configure [device file uploads](howto-configure-file-uploads.md) in your application.

| Error code | Description | Possible Mitigation |
| - | - | - |
| 403006  | You've exceeded the number of concurrent file upload operations. Each device client is limited to 10 concurrent file uploads. | Ensure the device promptly notifies IoT Central that the file upload operation has completed. If that doesn't work, try reducing the request timeout. |

## Payload shape issues

When you've established that your device is sending data to IoT Central, the next step is to ensure that your device is sending data in a valid format.

There are two main categories of common issues that cause device data to not appear in IoT Central:

- Device template to device data mismatch:
  - Mismatch in naming such as typos or case-matching issues.
  - Unmodeled properties where the schema isn't defined in the device template.
  - Schema mismatch such as a type defined in the template as `boolean`, but the data is a string.
  - The same telemetry name is defined in multiple interfaces, but the device isn't IoT Plug and Play compliant.
- Data shape is invalid JSON. To learn more, see [Telemetry, property, and command payloads](concepts-telemetry-properties-commands.md).

To detect which categories your issue is in, run the most appropriate command for your scenario:

- To validate telemetry, use the preview command:

    ```azurecli
    az iot central diagnostics validate-messages --app-id <app-id> --device-id <device-name>
    ```

- To validate property updates, use the preview command

    ```azurecli
    az iot central diagnostics validate-properties --app-id <app-id> --device-id <device-name>
    ```

You may be prompted to install the `uamqp` library the first time you run a `validate` command.

The following output shows example error and warning messages from the validate command:

```output
Validating telemetry.
Filtering on device: v22upeoqx6.
Exiting after 300 second(s), or 10 message(s) have been parsed (whichever happens first).
[WARNING] [DeviceId: v22upeoqx6] No encoding found. Expected encoding 'utf-8' to be present in message header.

[WARNING] [DeviceId: v22upeoqx6] Content type '' is not supported. Expected Content type is 'application/json'.

[ERROR] [DeviceId: v22upeoqx6] [TemplateId: urn:krhsi_k0u:modelDefinition:w53jukkazs] Datatype of field 'humid' does not match the da
tatype 'double'. Data '56'. All dates/times/datetimes/durations must be ISO 8601 compliant.
```

If you prefer to use a GUI, use the IoT Central **Raw data** view to see if something isn't being modeled. The **Raw data** view doesn't detect if the device is sending malformed JSON.

:::image type="content" source="media/troubleshoot-connection/raw-data-view.png" alt-text="Screenshot of Raw Data view":::

When you've detected the issue, you may need to update device firmware, or create a new device template that models previously unmodeled data.

If you chose to create a new template that models the data correctly, migrate devices from your old template to the new template. To learn more, see [Manage devices in your Azure IoT Central application](howto-manage-devices-individually.md).

## Next steps

If you need more help, you can contact the Azure experts on the [Microsoft Q&A and Stack Overflow forums](https://azure.microsoft.com/support/community/). Alternatively, you can file an [Azure support ticket](https://portal.azure.com/#create/Microsoft.Support).

For more information, see [Azure IoT support and help options](../../iot-fundamentals/iot-support-help.md).
