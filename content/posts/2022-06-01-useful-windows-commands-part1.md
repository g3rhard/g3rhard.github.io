---
layout: post
title:  "Useful Windows commands - Part 1"
date:   2022-06-01 09:00:00 +0800
categories: windows cli powershell
---

*1*. Get powershell history:

  ```powershell
  notepad (Get-PSReadlineOption).HistorySavePath
  ```

*2*. Work with APPX packages:

  ```powershell
  # Get list of packages
  Get-AppxPackage | Select Name, PackageFullNamer
  # Add package from file
  Add-AppxPackage -Path "path_to_the_file.msix"
  # Remove package
  Get-AppxPackage *PACKAGE_NAME* | Remove-AppxPackage
  ```

*3*. Get available parameters for the powershell command:

  ```powershell
  (Get-Command COMMAND_NAME).Parameters
  ```

*4*. Work with Powershell modules:

  ```powershell
  # Get modules list
  Get-Module -ListAvailable
  # Save module to another place (PATH_TO_NEW_MODULES_FOLDER)
  Save-Module -Name Module_name -Path PATH_TO_NEW_MODULES_FOLDER -Repository PSGallery
  ```

*5*. Work with Powershell profile:

  ```powershell
  # Create profile if it not exist
  if ((Test-Path $profile) -eq $false) {
    New-Item -path $profile -type file –force
  }
  # Edit profile
  notepad $profile
  ```

*6*. Add custom path for modules in Powershell $profile:

  ```powershell
  $env:PSModulePath = ((@("PATH_TO_NEW_MODULES_FOLDER") + ($env:PSModulePath -split ";")) -join ";")
  ```

That's all.

## Additional links

1. [Dan Tsekhanskiy - My PowerShell Profile](https://tseknet.com/blog/psprofile)
2. [mikemaccana/powershell-profile](https://github.com/mikemaccana/powershell-profile)
3. [how-to geek - Customizing your PowerShell Profile](https://www.howtogeek.com/50236/customizing-your-powershell-profile)
