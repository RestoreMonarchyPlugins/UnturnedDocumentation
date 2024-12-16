---
position: 1
title: Development Environment
authors: MCrow
category: developers
published: true
description: Learn what you need to get started with Unturned plugin development.
---

Before you can make any plugins for Unturned, you need to set up your development environment. This guide will help you set up Visual Studio and install project templates for creating plugins.

## Visual Studio
![](assets/visual_studio.png)
Visual Studio is an integrated development environment (IDE) from Microsoft that is used for developing applications in C# and other languages. It is the recommended IDE for developing Unturned plugins.
1. Go to [visualstudio.microsoft.com](https://visualstudio.microsoft.com/) and download the latest version of Visual Studio.
2. Run the installer and follow the instructions.
3. In the installer, select the following workloads:
   - **.NET desktop development**
   - **Game development with Unity** (optional)
4. Open Visual Studio to verify the installation.

## Unturned Templates
We have created a custom project template for creating Unturned plugins. This template will help you get started with plugin development quickly and let you more easily create new plugins in the future.

### Install .NET SDK
Before you can install the templates, you need to install the .NET SDK. The .NET SDK contains the .NET CLI, which is required to install custom project templates for Visual Studio.

1. Go to [dotnet.microsoft.com](https://dotnet.microsoft.com/download) and download the latest .NET SDK.
2. Run the installer and follow the instructions.
3. Open a terminal or PowerShell window and run `dotnet --version` to verify the installation. You should see the version of the SDK you installed.

![](assets/dotnet_version.png)

### Install Unturned Templates
Now that you have the .NET SDK installed, you can install the Unturned project templates.

1. Close Visual Studio if it is open.
2. Open a terminal or PowerShell window and run the following command:
   ```shell
   dotnet new -i RestoreMonarchy.UnturnedTemplates
   ```
   You should see output similar to the following:

   ![](assets/unturned_templates.png)
3. Open Visual Studio and click on `Create a new project`.
4. Search for `Unturned`. You should see the list of Unturned templates.

   ![](assets/unturned_templates_list.png)


### Conclusion
You have now set up your development environment for creating Unturned plugins. In the next guide, we will create a new plugin using the project template we installed.