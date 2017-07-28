---
title: Deploy and install Outlook add-ins for testing | Microsoft Docs
description: Learn how to test an Outlook add-in during development.
author: jasonjoh

ms.topic: article
ms.technology: office-add-ins
ms.date: 07/28/2017
ms.author: jasonjoh
---

# Deploy and install Outlook add-ins for testing

As part of the process of developing an Outlook add-in, you will probably find yourself iteratively deploying and installing the add-in for testing, which involves the following steps:

1. Creating a manifest file that describes the add-in.
1. Deploying the add-in UI file(s) to a web server.
1. Installing the add-in in your mailbox.
1. Test the add-in, making appropriate changes to the UI or manifest files, and repeating steps 2 and 3 to test the changes.

## Creating a manifest file for the add-in

Each add-in is described by an XML manifest, a document that gives the server information about the add-in, provides descriptive information about the add-in for the user, and identifies the location of the add-in UI HTML file. You can store the manifest in a local folder or server, as long as the location is accessible by the Exchange server of the mailbox that you are testing with. We'll assume that you store your manifest in a local folder. For information about how to create a manifest file, see [Outlook add-in manifests](manifests.md).

## Deploying an add-in to a web server

You can use HTML and JavaScript to create the add-in. The resulting source files are stored on a web server that can be accessed by the Exchange server that hosts the add-in. After initially deploying the source files for the add-in, you can update the add-in UI and behavior by replacing the HTML files or JavaScript files stored on the web server with a new version of the HTML file.

## Installing the add-in

After preparing the add-in manifest file and deploying the add-in UI to a web server that can be accessed, you can sideload the add-in for a mailbox on an Exchange server by using an Outlook client, or install the add-in by running remote Windows PowerShell cmdlets.

## Sideloading the add-in

You can install an add-in if your mailbox is on Exchange Online, Exchange 2013 or a later release. Sideloading add-ins requires at minimum the **My Custom Apps** role for your Exchange Server. In order to test your add-in, or install add-ins in general by specifying a URL or file name for the add-in manifest, you should request your Exchange administrator to provide the necessary permissions.

The Exchange administrator can run the following PowerShell cmdlet to assign a single user the necessary permissions. In this example, `wendyri` is the user's email alias.

```Shell
New-ManagementRoleAssignment -Role "My Custom Apps" -User "wendyri"
```

If necessary, the administrator can run the following cmdlet to assign multiple users the similar necessary permissions:

```Shell
$users = Get-Mailbox *$users | ForEach-Object { New-ManagementRoleAssignment -Role "My Custom Apps" -User $_.Alias}
```

For more information about the My Custom Apps role, see [My Custom Apps role](http://technet.microsoft.com/en-us/library/aa0321b3-2ec0-4694-875b-7a93d3d99089%28EXCHG.150%29.aspx). 

Using Office 365 or Visual Studio to develop add-ins assigns you the organization administrator role which allows you to install add-ins by file or URL in the EAC, or by Powershell cmdlets.

### Installing an add-in by using remote PowerShell

After you create a remote Windows PowerShell session on your Exchange server, you can install an Outlook add-in by using the `New-App` cmdlet with the following PowerShell command.

```Shell
New-App -URL:"http://<fully-qualified URL">
```

The fully qualified URL is the location of the add-in manifest file that you prepared for your add-in.

You can use the following additional PowerShell cmdlets to manage the add-ins for a mailbox:

-  `Get-App` - Lists the add-ins that are enabled for a mailbox.
-  `Set-App` - Enables or disables a add-in on a mailbox.
-  `Remove-App` - Removes a previously installed add-in from an Exchange server.

## Client versions

Deciding what versions of the Outlook client to test depends on your development requirements.

- If you are developing an add-in for private use, or only for members of your organization, then it is important to test the versions of Outlook that your company uses. Keep in mind that some users may use Outlook on the web, so testing your company's standard browser versions is also important.
- If you are developing an add-in to list in the Office Store, you must test the required versions as specified in the [Office Store validation policies 4.12.1](https://dev.office.com/officestore/docs/validation-policies#4-apps-and-add-ins-behave-predictably). This includes:
    - The latest version of Outlook for Windows and the version prior to the latest
    - The latest version of Outlook for Mac
    - The latest version of Outlook for iOS (if your add-in [supports mobile form factor](add-mobile-support.md))
    - The browser versions specified in Office Store validation policy 4.12.1.

> [!NOTE]
> If your add-in does not support one of the above clients due to [requesting an API requirement set](apis.md) that the client does not support, that client would be removed from the list of reqiured clients.

## Additional resources

- [Troubleshoot user errors with Office Add-ins](https://dev.office.com/docs/add-ins/testing/testing-and-troubleshooting?product=outlook)