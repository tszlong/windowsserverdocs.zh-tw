---
title: "建立執行文章初始設定工作 PostIC.cmd 檔案"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>建立執行文章初始設定工作 PostIC.cmd 檔案

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以新增後初始設定自訂項目撰寫您的程式碼，並從指令碼檔名為 PostIC.cmd 呼叫該驗證碼。 使用 PostIC.cmd 檔案時，您必須遵循下列指導方針：  
  
-   自訂您的驗證碼必須以無訊息方式執行（不顯示使用者介面）。  
  
-   自訂您的驗證碼不起始伺服器的重新開機。 初次設定將會重新伺服器的最後一個工作。  
  
-   自訂您的驗證碼必須執行或較少三分鐘的時間。  
  
 定義您返回 0 如果能順利執行程式碼的 PostIC.cmd 檔案。 如果任何其他值，作業系統會尋找檔名為[SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure)，其中包含應如果未成功執行 PostIC.cmd 檔案中的程式碼執行的程式碼。 PostIC.cmd 檔案和 SetupFailure.cmd 檔案都必須位於的 C:\Windows\Setup\Scripts。  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>若要定義後初始設定自訂項目  
  
1.  撰寫稱為從 PostIC.cmd 指令碼的程式碼。  
  
2.  使用「記事本」建立稱為 PostIC.cmd 檔案，並新增至您在步驟 1 中建立的程式碼通話。 請確定您的驗證碼傳回成功值。  
  
3.  儲存 PostIC.cmd C:\Windows\Setup\Scripts 中。  
  
4.  （選擇性）建立 SetupFailure.cmd 如果 PostIC.cmd 傳回 0 以外的任何項目，請執行程式碼的檔案。  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 您可以使用 SetupFailure.cmd 提供通知的初始設定中的問題。 SetupFailure.cmd 檔案中包含您想要執行的問題發生的驗證碼。 SetupFailure.cmd 檔案位於 C:\Windows\Setup\Scripts 並安裝任務或 PostIC.cmd 檔案傳回一個值以外 0 時，就會發生問題時執行。  
  
##### <a name="to-define-notifications"></a>若要定義的通知  
  
1.  撰寫稱為從 SetupFailure.cmd 指令碼的程式碼。  
  
2.  使用「記事本」建立稱為 SetupFailure.cmd 檔案，並新增至您在步驟 1 中建立的程式碼通話。 請確定您的驗證碼傳回成功值。  
  
3.  儲存 SetupFailure.cmd C:\Windows\Setup\Scripts 中。  
  
## <a name="see-also"></a>也了  
 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)