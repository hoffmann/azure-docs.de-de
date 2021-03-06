---
title: Connect to Azure Stack with PowerShell | Microsoft Docs
description: Learn how to manage Azure Stack with PowerShell
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: de418dfa-6f33-407e-b001-42166cf71014
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: helaw
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8b2761f89c4505237d69338743bf45a20928ac0a


---
# <a name="install-powershell-and-connect-to-azure-stack"></a>Install PowerShell and connect to Azure Stack
In this guide, we walk through the steps for connecting to Azure Stack with PowerShell. Once completed, these steps can also help you manage and deploy resources.

## <a name="install-azure-stack-powershell-cmdlets"></a>Install Azure Stack PowerShell cmdlets
1. AzureRM cmdlets are installed from the PowerShell Gallery. To begin, open a PowerShell Console on MAS-CON01 and run the following command to return a list of PowerShell repositories available:
   
       Get-PSRepository
   
     ![Screenshot result of running 4Get-PSRepository with PSGallery listed](./media/azure-stack-connect-powershell/image1.png)
2. Run the following command to install the AzureRM module:
   
       Install-Module -Name AzureRM -RequiredVersion 1.2.6 -Scope CurrentUser
   
   > [!NOTE]
   > *-Scope CurrentUser* is optional. If you want more than the current user to have access to the modules, use an elevated command prompt and leave off the *Scope* parameter.
   > 
   > 
3. To confirm the installation of AzureRM modules, execute the following commands:
   
       Get-Command -Module AzureRM.AzureStackAdmin

## <a name="connect-to-azure-stack"></a>Connect to Azure Stack
A module is available for download that handles configuring the PowerShell connection to Azure Stack for you.  Visit [Azure Stack Tools](http://aka.ms/ConnectToAzureStackPS) for the module and additional steps. 

## <a name="retrieve-a-list-of-subscriptions"></a>Retrieve a list of subscriptions
In this section, you verify PowerShell cmdlets are running against Azure Stack by retrieving and selecting a subscription for use.

Run the following command to retrieve a list of Azure Stack subscriptions associated with your account:

    Get-AzureRmSubscription


## <a name="next-steps"></a>Next steps
[Deploy templates with PowerShell](azure-stack-deploy-template-powershell.md)

[Connect with Azure CLI](azure-stack-connect-cli.md)

[Deploy templates with Visual Studio](azure-stack-deploy-template-visual-studio.md)




<!--HONumber=Nov16_HO3-->


