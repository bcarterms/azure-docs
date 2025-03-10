---
title: Create an Azure VM with Accelerated Networking using Azure CLI
description: Learn how to create a Linux virtual machine with Accelerated Networking enabled.
services: virtual-network
documentationcenter: na
author: steveesp
manager: gedegrac
editor: ''
tags: azure-resource-manager

ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/15/2022
ms.author: steveesp
ms.custom: 

---
# Create a Linux virtual machine with Accelerated Networking using Azure CLI

## Portal creation

Though this article provides steps to create a virtual machine with accelerated networking using the Azure CLI, you can also [create a virtual machine with accelerated networking using the Azure portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). When creating a virtual machine in the portal, in the **Create a virtual machine** blade, choose the **Networking** tab.  In this tab, there is an option for **Accelerated networking**.  If you have chosen a [supported operating system](./accelerated-networking-overview.md#supported-operating-systems) and [VM size](./accelerated-networking-overview.md#supported-vm-instances), this option will automatically populate to "On."  If not, it will populate the "Off" option for Accelerated Networking and give the user a reason why it isn't enabled.   
You can also enable or disable accelerated networking through the portal after VM creation by navigating to the network interface and clicking the button at the top of the **Overview** blade.

>[!NOTE]
> Only supported operating systems can be enabled through the portal. If you're using a custom image, and your image supports Accelerated Networking, create your VM using CLI or PowerShell. 

After the VM is created, you can confirm that Accelerated Networking is enabled by following the [confirmation instructions](#confirm-that-accelerated-networking-is-enabled).

## CLI creation
### Create a virtual network

Install the latest [Azure CLI](/cli/azure/install-azure-cli) and log in to an Azure account using [az login](/cli/azure/reference-index). In the following examples, replace example parameter names with your own values. Example parameter names included *myResourceGroup*, *myNic*, and *myVm*.

Create a resource group with [az group create](/cli/azure/group). The following example creates a resource group named *myResourceGroup* in the *centralus* location:

```azurecli
az group create --name myResourceGroup --location centralus
```

Select a supported Linux region listed in [Linux Accelerated Networking](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

Create a virtual network with [az network vnet create](/cli/azure/network/vnet). The following example creates a virtual network named *myVnet* with one subnet:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

### Create a network security group
Create a network security group with [az network nsg create](/cli/azure/network/nsg). The following example creates a network security group named *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

The network security group contains several default rules, one of which disables all inbound access from the Internet. Open a port to allow SSH access to the virtual machine with [az network nsg rule create](/cli/azure/network/nsg/rule):

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

### Create a network interface with Accelerated Networking

Create a public IP address with [az network public-ip create](/cli/azure/network/public-ip). A public IP address isn't required if you don't plan to access the VM from the Internet. However, it's required to complete the steps in this article.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Create a network interface with [az network nic create](/cli/azure/network/nic) with Accelerated Networking enabled. The following example creates a network interface named *myNic* in the *mySubnet* subnet of the *myVnet* virtual network and associates the *myNetworkSecurityGroup* network security group to the network interface:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

### Create a VM and attach the NIC
When you create the VM, specify the NIC you created with `--nics`. Select a size and distribution listed in [Linux accelerated networking](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

Create a VM with [az vm create](/cli/azure/vm). The following example creates a VM named *myVM* with the UbuntuLTS image and a size that supports Accelerated Networking (*Standard_DS4_v2*):

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

For a list of all VM sizes and characteristics, see [Linux VM sizes](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Once the VM is created, output similar to the following example output is returned. Take note of the **publicIpAddress**. This address is used to access the VM in subsequent steps.

```output
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

### Confirm that accelerated networking is enabled

Use the following command to create an SSH session with the VM. Replace `<your-public-ip-address>` with the public IP address assigned to the virtual machine that you created, and replace *azureuser* if you used a different value for `--admin-username` when you created the VM.

```bash
ssh azureuser@<your-public-ip-address>
```

From the Bash shell, enter `uname -r` and confirm that the kernel version is one of the following versions, or greater:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 3.10.0-693
* **CentOS**: 3.10.0-693


Confirm that the Mellanox VF device is exposed to the VM with the `lspci` command. The returned output is similar to the following output:

```output
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Check for activity on the VF (virtual function) with the `ethtool -S eth0 | grep vf_` command. If you receive output similar to the following sample output, accelerated networking is enabled and active.

```output
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Accelerated Networking is now enabled for your VM.

## Handle dynamic binding and revocation of virtual function 
Applications must run over the synthetic NIC that is exposed in VM. If the application runs directly over the VF NIC, it doesn't receive **all** packets that are destined to the VM, since some packets show up over the synthetic interface. If you run an application over the synthetic NIC, it guarantees that the application receives **all** packets that are destined to it. It also makes sure that the application keeps running, even if the VF is revoked during host servicing. Applications binding to the synthetic NIC is a **mandatory** requirement for all applications taking advantage of **Accelerated Networking**.

For more details on application binding requirements, see [How Accelerated Networking works in Linux and FreeBSD VMs](/azure/virtual-network/accelerated-networking-how-it-works#application-usage).

## Enable Accelerated Networking on existing VMs
If you've created a VM without Accelerated Networking, it's possible to enable this feature on an existing VM. The VM must support Accelerated Networking by meeting the following prerequisites that are also outlined:

* The VM must be a supported size for Accelerated Networking
* The VM must be a supported Azure Gallery image (and kernel version for Linux)
* All VMs in an availability set or VMSS must be stopped/deallocated before enabling Accelerated Networking on any NIC

### Individual VMs & VMs in an availability set
First stop/deallocate the VM or, if an Availability Set, all the VMs in the Set:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

If your VM was created individually without an availability set, you only must stop or deallocate the individual VM to enable Accelerated Networking. If your VM was created with an availability set, all VMs contained in the set must be stopped or deallocated before enabling Accelerated Networking on any of the NICs. 

Once stopped, enable Accelerated Networking on the NIC of your VM:

```azurecli
az network nic update \
    --name myNic \
    --resource-group myResourceGroup \
    --accelerated-networking true
```

Restart your VM or, if in an Availability Set, all the VMs in the Set and confirm that Accelerated Networking is enabled: 

```azurecli
az vm start --resource-group myResourceGroup \
    --name myVM
```

### VMSS
VMSS is slightly different but follows the same workflow. First, stop the VMs:

```azurecli
az vmss deallocate \
    --name myvmss \
    --resource-group myrg
```

Once the VMs are stopped, update the Accelerated Networking property under the network interface:

```azurecli
az vmss update --name myvmss \
    --resource-group myrg \
    --set virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].enableAcceleratedNetworking=true
```

>[!NOTE]
> A VMSS has VM upgrades that apply updates using three different settings, automatic, rolling, and manual. In these instructions, the policy is set to automatic so that the VMSS will pick up the changes immediately after reboot. To set it to automatic so that the changes are immediately picked up: 

```azurecli
az vmss update \
    --name myvmss \
    --resource-group myrg \
    --set upgradePolicy.mode="automatic"
```

Finally, restart the VMSS:

```azurecli
az vmss start \
    --name myvmss \
    --resource-group myrg
```

Once you restart, wait for the upgrades to finish but once completed, the VF appears inside the VM. (Make sure you're using a supported OS and VM size.)

### Resizing existing VMs with Accelerated Networking

VMs with Accelerated Networking enabled can only be resized to VMs that support Accelerated Networking.  

A VM with Accelerated Networking enabled can't be resized to a VM instance that doesn't support Accelerated Networking using the resize operation. Instead, to resize one of these VMs: 

* Stop/Deallocate the VM or if in an availability set/VMSS, stop/deallocate all the VMs in the set/VMSS.
* Accelerated Networking must be disabled on the NIC of the VM or if in an availability set/VMSS, all VMs in the set/VMSS.
* Once Accelerated Networking is disabled, the VM/availability set/VMSS can be moved to a new size that doesn't support Accelerated Networking and restarted.

## Next steps
* Learn [how Accelerated Networking works](./accelerated-networking-how-it-works.md)
* Learn how to [create a VM with Accelerated Networking in PowerShell](../virtual-network/create-vm-accelerated-networking-powershell.md)
* Improve latency with an [Azure proximity placement group](../virtual-machines/co-location.md)

