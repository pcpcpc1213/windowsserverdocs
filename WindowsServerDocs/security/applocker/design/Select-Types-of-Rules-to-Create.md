---
title: Select Types of Rules to Create
description: "Windows Server Security"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-applocker
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a2a4fa2-e11a-4329-8522-cb2a99726450
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# Select Types of Rules to Create

>Applies To: Windows Server&reg; 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012

This topic lists resources you can use when selecting your application control policy rules by using AppLocker.

When determining what types of rules to create for each of your groups, you should also determine what enforcement setting to use for each group. Different rule types are more applicable for some applications, depending on the way that the applications are deployed in a specific business group.

The following topics provide additional information about AppLocker rules that can help you decide what rules to use for your applications:

-   [Understanding AppLocker Rule Behavior](../get-started/how-applocker-works/Understanding-AppLocker-Rule-Behavior.md)

-   [Understanding AppLocker Rule Exceptions](../get-started/how-applocker-works/Understanding-AppLocker-Rule-Exceptions.md)

-   [Understanding AppLocker Rule Collections](../get-started/how-applocker-works/Understanding-AppLocker-Rule-Collections.md)

-   [Understanding AppLocker Allow and Deny Actions on Rules](../get-started/how-applocker-works/Understanding-AppLocker-Allow-and-Deny-Actions-on-Rules.md)

-   [Understanding AppLocker Rule Condition Types](../get-started/how-applocker-works/Understanding-AppLocker-Rule-Condition-Types.md)Understanding AppLocker Rule Condition Types

-   [Understanding AppLocker Default Rules](../get-started/how-applocker-works/Understanding-AppLocker-Default-Rules.md)

### Select the rule collection
The rules you create will be in one of the following rule collections:

-   Executable files: .exe and .com

-   Windows Installer files: .msi, .msp, and .mst

-   Scripts: .ps1, .bat, .cmd, .vbs, and .js

-   Packaged apps and packaged app installers: .appx

-   DLLs: .dll and .ocx

> [!NOTE]
> .appx and .mst file types are not applicable to AppLocker running on Windows Server 2008 R2 and Windows 7.

By default, the rules will allow a file to run based upon user or group privilege. If you use DLL rules, a DLL allow rule has to be created for each DLL that is used by all of the allowed applications. The DLL rule collection is not enabled by default.

In the Woodgrove Bank example, the line-of-business application for the Bank Tellers business group is C:\Program Files\Woodgrove\Teller.exe, and this application needs to be included in a rule. In addition, because this rule is part of a list of allowed applications, all the Windows files under C:\Windows must be included as well.

### Determine the rule condition
A rule condition is criteria upon which an AppLocker rule is based and can only be one of the rule conditions in the following table.

|Rule condition|Usage scenario|Resources|
|------------------|------------------|-------------|
|Publisher|To use a publisher condition, the files must be digitally signed by the software publisher, or you must do so by using an internal certificate. Rules that are specified to the version level might have to be updated when a new version of the file is released.|For more information about this rule condition, see [Understanding the Publisher Rule Condition in AppLocker](../get-started/how-applocker-works/Understanding-the-Publisher-Rule-Condition-in-AppLocker.md).|
|Path|Any file can be assigned this rule condition; however, because path rules specify locations within the file system, any subdirectory will also be affected by the rule (unless explicitly exempted).|For more information about this rule condition, see [Understanding the Path Rule Condition in AppLocker](../get-started/how-applocker-works/Understanding-the-Path-Rule-Condition-in-AppLocker.md).|
|File hash|Any file can be assigned this rule condition; however, the rule must be updated each time a new version of the file is released because the hash value is based in part upon the version.|For more information about this rule condition, see [Understanding the File Hash Rule Condition in AppLocker](../get-started/how-applocker-works/Understanding-the-File-Hash-Rule-Condition-in-AppLocker.md).|

In the Woodgrove Bank example, the line-of-business application for the Bank Tellers business group is signed and is located at C:\Program Files\Woodgrove\Teller.exe. Therefore, the rule can be defined with a publisher condition. If the rule is defined to a specific version and above (for example, Teller.exe version 8.0 and above), then this will allow any updates to this application to occur without interruption of access to the users if the application's name and signed attributes stay the same.

### Determine how to allow system files to run
Because AppLocker rules build a list of allowed applications, a rule or rules must be created to allow all Windows files to run. AppLocker provides a means to ensure system files are properly considered in your rule collection by generating the default rules for each rule collection. You can use the default rules as a template when creating your own rules. However, these rules are only meant to function as a starter policy when you are first testing AppLocker rules so that the system files in the Windows folders will be allowed to run. When a default rule is created, it is denoted with "(Default rule)" in its name as it appears in the rule collection.

You can also create a rule for the system files based on the path condition. In the preceding example, for the Bank Tellers group, all Windows files reside under C:\Windows and can be defined with the path rule condition type. This will permit access to these files whenever updates are applied and the files change. If you require additional application security, you might need to modify the rules created from the built-in default rule collection. For example, the default rule to allow all users to run .exe files in the Windows folder is based on a path condition that allows all files within the Windows folder to run. The Windows folder contains a Temp subfolder to which the Users group is given the following permissions:

-   Traverse Folder/Execute File

-   Create Files/Write Data

-   Create Folders/Append Data

These permissions settings are applied to this folder for application compatibility. However, because any user can create files in this location, allowing applications to be run from this location might conflict with your organization's security policy.

## Next steps
After you have selected the types of rules to create, record your findings as explained in [Document Your AppLocker Rules](Document-Your-AppLocker-Rules.md).

After recording your findings for the AppLocker rules to create, you will need to consider how to enforce the rules. For information about how to do this, see [Determine Group Policy Structure and Rule Enforcement](Determine-Group-Policy-Structure-and-Rule-Enforcement.md).

