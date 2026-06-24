# Kali Linux Setup in VirtualBox

## Objective

Create a Kali Linux virtual machine using Oracle VirtualBox.

## Prerequisites

- Oracle VirtualBox installed
- Minimum 8 GB RAM on host machine
- At least 50 GB free disk space

## Step 1: Download Kali Linux

1. Visit the Kali Linux Downloads page.
2. Download the Installer ISO.

Example:
```
kali-linux-2026.1-installer-amd64.iso
```

## Step 2: Create a New Virtual Machine

1. Open VirtualBox.
2. Click **New**.
3. Configure the VM:

| Setting | Value |
|----------|---------|
| Name | Kali Linux |
| Type | Linux |
| Version | Debian (64-bit) |
| Base Memory | 4096 MB |
| Processors | 2 |
| Virtual Hard Disk | 50 GB |

4. Click **Finish**.

## Step 3: Attach the Kali Linux ISO

1. Select the VM.
2. Click **Settings**.
3. Navigate to **Storage**.
4. Select the Optical Drive.
5. Click **Choose a Disk File**.
6. Select the Kali Linux ISO.

## Step 4: Configure Display

Navigate to:

Settings → Display

Configure:

- Graphics Controller: VMSVGA
- Video Memory: 128 MB

## Step 5: Configure Network

Navigate to:

Settings → Network

Configure:

- Adapter 1: Enabled
- Attached To: NAT

## Step 6: Start the Virtual Machine

1. Select the VM.
2. Click **Start**.
3. The Kali Linux installer will boot from the ISO.

## Step 7: Install Kali Linux

1. Select **Graphical Install**.
2. Choose Language.
3. Select Location.
4. Configure Keyboard Layout.
5. Set Hostname.
6. Create User Account.
7. Set Password.
8. Configure Disk Partitioning.
9. Install the Base System.
10. Install GRUB Bootloader.
11. Complete the installation.

## Result:
Kali Linux is successfully installed in VirtualBox and ready for cybersecurity learning and penetration testing practice.
