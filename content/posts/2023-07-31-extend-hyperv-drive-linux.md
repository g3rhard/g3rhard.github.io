---
layout: post
title:  "Expand Hyper-V disk after creation in Linux guest system"
date:   2023-07-31 09:00:00 +0800
categories: work hyperv
---

Just short note about it, as from time to time I need to increase volume size.

## Part 1 - In host system

* Get list of VMs

  ```powershell
  Get-VM
  ```

* Turn off the VM.

  ```powershell
  $vmName = "YourVirtualMachineName"
  Stop-VM -Name "$vmName"
  ```

* Expand the disk

  ```powershell
  Get-VHD -VMName $vmName
  Resize-VHD -Path "VHD_PATH" -SizeBytes 100GB
  ```

* Check the changes

  ```poweshell
  Get-VHD -VMName $vmName
  ```

* Start the VM & connect to VM

  ```poweshell
  Start-VM -Name "$vmName"
  Connect-VM -VMName "$vmName"
  ```

## Part 2 - Inside VM

* Install guest tools for VM (if it wasn't done before)

  ```sh
  sudo apt install cloud-guest-utils
  ```

* Work with virtual disk

  ```sh
  lsblk # to find the correct disk name and partition
  sudo growpart /dev/sda 1 # choose the partition of hard drive to increase, in this example sda and 1 partition
  sudo resize2fs /dev/sda1 # resize filesystem in the partition, in this example sda and 1 partition
  ```

* Check the changes

  ```sh
  df -h
  ```

That's all.

## Additional links

1. [Expand Ubuntu disk after Hyper-V Quick Create](https://linguist.is/2020/08/12/expand-ubuntu-disk-after-hyper-v-quick-create/)
2. [Supported Ubuntu virtual machines on Hyper-V](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v)
