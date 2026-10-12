🚀 Windows Deployment Services (WDS) Configuration Lab

A comprehensive and professional guide for installing, configuring, and deploying Windows Deployment Services (WDS) alongside Active Directory Domain Services (AD DS) and DHCP for Network OS Deployment.

📋 Prerequisites

Before installing the WDS role, ensure that the following requirements are met on your server:

Active Directory: The server must be a member of an Active Directory Domain Services (AD DS) domain or configured as a Domain Controller.

DHCP Server: An active DHCP server must be present on the network (WDS uses PXE booting, which requires DHCP for IP address allocation).

DNS Server: An active DNS server must be available on the network.

NTFS Partition: An NTFS-formatted volume/partition (e.g., C: or D:) to store operating system images and deployment files.

🛠️ Step 1: Install WDS Role via PowerShell

Instead of using the Server Manager GUI, open PowerShell as Administrator and run:

# Install Windows Deployment Services and all Management Tools
Install-WindowsFeature -Name WDS -IncludeManagementTools -IncludeAllSubFeature


⚙️ Step 2: Automated WDS Server Configuration

Run the following commands using Command Prompt (Run as Administrator) or PowerShell to configure the server, path, and PXE response settings automatically:

:: Initialize and configure WDS server with storage path
wdsutil /Initialize-Server /reminInst:"C:\RemoteInstall"

:: Configure PXE Settings to respond to all clients (known and unknown)
wdsutil /Set-Server /AnswerClients:All


Note: Replace C:\RemoteInstall with your target drive if you prefer another volume (e.g., D:\RemoteInstall).

💿 Step 3: Add Boot & Install Images via Command Line

Assuming your Windows Server ISO / DVD is mounted at drive E::

1. Add Boot Image (boot.wim)

wdsutil /Add-Image /ImageFile:"E:\sources\boot.wim" /ImageType:Boot


2. Create Image Group & Add Install Image (install.wim)

:: Create Image Group
wdsutil /Add-ImageGroup /ImageGroup:"Windows Server Images"

:: Import install.wim into the created group
wdsutil /Add-Image /ImageFile:"E:\sources\install.wim" /ImageType:Install /ImageGroup:"Windows Server Images"


⚡ Step 4: Manage & Restart WDS Service

To restart and verify the status of the WDS service using PowerShell:

# Restart WDS Service
Restart-Service -Name WDSServer

# Check Service Status
Get-Service -Name WDSServer


🖥️ Step 5: Client Deployment Test

Boot a target client computer or virtual machine via network boot (PXE / Network Boot).

The client will obtain an IP address from the DHCP server and load boot.wim over the network.

Enter authorized domain credentials (Domain Credentials / Administrator) when prompted.

Select the desired OS image from the list to begin the network installation process.

👨‍💻 Author & Documentation

Abdullah Mohamed Naeem

Network & Security Engineer
