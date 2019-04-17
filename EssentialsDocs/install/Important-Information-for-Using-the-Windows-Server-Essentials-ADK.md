---
title: "使用 Windows Server Essentials ADK 重要資訊"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26cb2992-1250-4672-98ee-8b870baa45d5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4dec1fdf01538ca119b991675f932d2d8ec1e097
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="important-information-for-using-the-windows-server-essentials-adk"></a>使用 Windows Server Essentials ADK 重要資訊

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

建立和自訂 Windows Server Essentials 的映像，您使用工具] 中的許多[Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248647)，但在 Windows 8 ADK，以及 Windows Server Essentials ADK 一些重要不同。  
  
 您應該下列重要不同注意：  
  
-   在 [已變更部分設定**%windir%\setup\script\SetupComplete.cmd**。 如果您想要使用這個命令時，您可以新增額外的 cmdlines，但不要移除現有的行。  
  
## <a name="working-with-passwords"></a>使用密碼  
  
-   若要設定的系統管理員密碼Admin@123和自動登入在 Install.wim\unattend.xml 支援。 因此，您不需要重新輸入密碼許多次期間伺服器的初次設定。 如果有自訂的 unattend.xml\ 根卸除式媒體，此設定將會被覆寫，且您將需要設定密碼，以及登入期間開機...  
  
-   在初始設定，使用者會建立新 account 和密碼提示。 這個新帳號變成作業系統的網路系統管理員負責。 系統管理員帳號，並自動登入然後已停用。 您可以使用的品質保證測試的 cfg.ini 檔案自動執行此程序。  
  
-   請參考[Windows 8 ADK](https://go.microsoft.com/fwlink/?LinkId=248694)文件，以建立 unattend.xml\ 檔案的詳細資訊。  
  
## <a name="see-also"></a>也了  

 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [開始使用 Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

