---
title: "Install and configure development tools (preview)"
description: Set up required finance and operations app development tools on your local computer.
author: pvillads
ms.date: 05/20/2024
ms.topic: how-to
ms.reviewer: pehecke
ms.author: pathaku
ms.subservice: developer
contributors:
 - Pankaj-Thakur
---

# Install and configure development tools (preview)

[!INCLUDE [cc-beta-prerelease-disclaimer](../../includes/cc-beta-prerelease-disclaimer.md)]

This article describes how to install and configure required Visual Studio finance and operations tools into your local development environment. The instructions in each section should be followed in the order presented in this article.

> [!IMPORTANT]
> As a prerequisite, you need access to a [provisioned developer-focused sandbox environment](../../admin/unified-experience/tutorial-deploy-new-environment-with-ERP-template.md).
>
> The user account you'll be using for development in the sandbox environment must be assigned the System Administrator role.
> 
> The development machine running Microsoft Windows must have Visual Studio 2022 installed with at least the .NET desktop development workload and the Modeling SDK. This SDK can be selected under individual components in the Visual Studio installer. Refer to [required Visual Studio components](/dynamics365/fin-ops-core/dev-itpro/dev-tools/developer-tools-vs2017#required-visual-studio-components). The development machine also must have Microsoft SQL Server or SQL Server Express LocalDB installed.

## Install the Power Platform extension for Visual Studio

Follow the instructions in this article to install the extension from the marketplace: [What is Power Platform Tools for Visual Studio](../devtools-vs.md). You don't need to install the plug-in profiler as directed in that article.

### Connect to your online environment

After installing the Power Platform Tools extension for Visual Studio, follow these instructions to connect Visual Studio to your unified developer-focused environment.

1. Launch Visual Studio if not already running.
1. In the **Tools** menu, select **Connect to Dataverse** or **Download Dynamics 365 FinOps assets**.

   - **Connect to Dataverse** only downloads assets if not downloaded earlier.
   - **Download Dynamics 365 FinOps assets** always downloads.

1. Select the desired options in the dialog and select **Login**.  
    
   If you need to use another account or a different online environment, don't choose to use the signed-in user. Enter the credentials for your developer user account with access to the online environment.

1. Select an available solution in the dialog, and then choose **Done**.
1. Find the sandbox environment created for online development in the list of Dataverse environments and connect to it.

   > [!NOTE]
   > If you have only one online development environment, the organization list dialog may not be shown as you would automatically connect to the environment.
   > :::image type="content" source="../media/unified-experience/UnoVSConnect.png" alt-text="Menu for connecting to unified environment from Visual Studio.":::
   > 
   > :::image type="content" source="../media/unified-experience/UnoLogin.png" alt-text="Login dialog when connecting to unified environment from Visual Studio with Show Advanced selected.":::
   >
   > If your account requires multi factor authentication, uncheck all checkboxes on login screen.
   >
   > If you are logging in to an online development environment in a tenant different from your own, where your user principal name (UPN) is added as a guest, update **Visual Studio** > **Tools** > **Options** > **Power Platform Tools** and select **Skip Discovery when connecting to Dataverse** and uncheck all the boxes on the login pane. You will be prompted to provide the Dataverse uri.
   >
   > :::image type="content" source="../media/unified-experience/UnoOption2.png" alt-text="Visual Studio> Tools> Options> Power Platform Tools to skip Discovery":::
   >
   > :::image type="content" source="../media/unified-experience/UnoLogin2.png" alt-text="Login dialog when connecting to unified environment from Visual Studio.":::
   > 
   > :::image type="content" source="../media/unified-experience/UnoEnvironmentUri.png" alt-text="Dialog to obtain the Dataverse uri to the unified development environment.":::

## Download and install the finance and operations extension and metadata

After installing the Power Platform Tools extension and connecting to the online Dataverse sandbox environment, you'll be presented with a dialog to download the finance and operations Visual Studio extension and metadata.

:::image type="content" source="../media/devexp-fno-download.png" alt-text="Extension and metadata download dialog.":::

> [!IMPORTANT]
> You can configure settings using the Visual Studio setting **Tools** > **Options** > **Power Platform Tools**.
>
> The option **Auto setup for Dynamics 365...** is enabled by default. This ensures you do not have to manually extract system metadata or create the metadata configuration. The tool will do this for you. Do look into the Visual Studio output pane for any user action needed.
>
> Consider selecting the option **Do not display Power Platform Explorer...** to speed up connecting to your environment and **Download logs...** to get logs for deployment and DBSync operations.
>
> :::image type="content" source="../media/unified-experience/D365FinOpsToolsOptions.png" alt-text="Tools options":::
>
> You need at least 12 GB of free space on the local system drive to download the extension and metadata.

Choose **OK** in the dialog to start the download. Visual Studio keeps you updated on the progress in the Visual Studio output pane.

### Extract system metadata

> [!NOTE]
> This is not required when auto setup is enabled

Downloaded assets can be found in the following location:  
`C:\Users\<User>\AppData\Local\Microsoft\Dynamics365\<ApplicationVersion>`

The following files are downloaded:

- `DYNAMICSXREFDB.bak` is a backup of the cross reference database.
- `Microsoft.Dynamics.Framework.Tools.Installer.vsix` is the Finance and operations extension.
- `PackagesLocalDirectory.zip` is the System metadata.
- `TraceParser.msi` [Learn more about Trace Parser](/dynamics365/fin-ops-core/dev-itpro/perf-test/trace-parser).

Unblock any of the three files that need unblocking.

:::image type="content" source="../media/unified-experience/D365FinOpsAsetsUnblock.png" alt-text="Unblock downloaded asset files":::

Unpack the `PackagesLocalDirectory.zip` file as `PackagesLocalDirectory`. We recommend using the [7zip](https://www.7-zip.org/download.html) utility to speed up extraction.

### Install the finance and operations extension

> [!NOTE]
> This is only required when the Visual Studio output pane indicates it is.

Using Windows File Explorer, from the folder specified below, open the `Microsoft.Dynamics.Framework.Tools.Installer.vsix` file to install the extension.

`C:\Users\<User>\AppData\Local\Microsoft\Dynamics365\<ApplicationVersion>`

### Configure the finance and operations extension 

> [!NOTE]
> This is not required when auto setup is enabled.

To configure the extension, follow these instructions.

1. Start Visual Studio, and then select **Continue without code** from the dialog.
1. Navigate to **Extensions** > **Dynamics 365** > **Configure Metadata** for the first time.  
   
   If you don't see the **Configure Metadata** option, open **Extensions** > **Dynamics 365** > **Infolog** and retry.
   
   :::image type="content" source="../media/unified-experience/D365FinOpsConfigureMetadataMenu.png" alt-text="Configure Metadata menu":::

1. On the configuration form, select **New** and then create a new configuration. Refer to the field descriptions provided in the [Configuration form field descriptions](#configuration-form-field-descriptions) table that follows this procedure.

   :::image type="content" source="../media/unified-experience/D365FinOpsConfigureMetadata.png" alt-text="Configure Metadata dialog":::

1. Select **Save**.

#### Configuration form field descriptions

| Form field | Description |
|---|---|
|Name| Name of the configuration you're creating now.|
|Description| Description of the configuration you're creating now.|
|Cross reference database server| Set to `(localdb)\.` if you're using LocalDB, or `localhost` if you're using Microsoft SQL Server.|
|Cross reference database name| The name for the cross reference database. The Visual Studio extension creates one if it doesn't exist.|
|Application version to restore cross reference database from| Select this. It's populated from the assets downloaded when you connected to the unified developer environment.|
|Folder for your own custom metadata| Folder where your own code is (or will be).|
|Folders for reference metadata| Should contain at least the path where you have the extracted the PackagesLocalDirectory folder mentioned in [Extract system metadata](#extract-system-metadata).|

#### Troubleshooting configuration

If the **Save** button is still grayed out after filling in all the fields, you'll see error messages with a red border in tooltips on fields with invalid values.

:::image type="content" source="../media/unified-experience/D365FinOpsConfigureMetadataValidation.png" alt-text="Configure Metadata Validation":::

Ensure that you entered the correct values for fields with errors. For example, use `(localdb)\.` if you're using LocalDB.
Also, if you're using LocalDB, you might need to first configure and test it. Consider issuing the following command from a Command prompt:

`sqllocaldb create MSSQLLocalDB -s`.

More information: [SQL Server Express LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb).

> [!NOTE]
> If the cross reference database does not already exist, the tool will restore the database from the backup downloaded to C:\Users\\`<User>`\AppData\Local\Microsoft\Dynamics365\\`<ApplicationVersion>`. You'll see notifications in the Visual Studio **Output** pane. If you need to restore it again by yourself you can do so. For instructions to do a restore refer to [Restore to SQL Server](/sql/samples/adventureworks-install-configure?tabs=ssms#restore-to-sql-server).

#### Completeing configuration

After the configuration is saved, you might get a prompt for elevation as administrator to register the protocol handler and to extract compiler files. Go ahead and accept these prompts.

Once the configuration completes, navigate to **View** > **Application Explorer** . You should see the **Application Explorer** window open. You might have to restart Visual Studio first.

:::image type="content" source="../media/unified-experience/D365FinOpsApplicationExplorer.png" alt-text="Application Explorer":::

You can create several configurations, but must select one to be current. The chosen configuration takes effect on new instances of Visual Studio.

## Summary

Congratulations! You're now ready to build, deploy, debug, and test your X++ modules working with the Power Platform online development environment. Explore the possibilities with the power of finance and operations integrated with Power Platform.

## Next steps

Start writing code, deploy, and debug.

> [!div class="nextstepaction"]
> [Write, deploy, and debug X++ code](finance-operations-debug.md)

### See also

[Unified developer experience for finance and operations apps (preview)](finance-operations-dev-overview.md)

[!INCLUDE [footer-banner](../../includes/footer-banner.md)]
