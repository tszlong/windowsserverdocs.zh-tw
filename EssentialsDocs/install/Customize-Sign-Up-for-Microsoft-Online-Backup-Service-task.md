---
title: "Microsoft Online 備份服務工作自訂上登入"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Microsoft Online 備份服務工作自訂上登入

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

根據預設，**Microsoft Online 備份 service 登入**工作上**裝置**] 索引標籤的儀表板開啟 Microsoft Online 備份 Service 的網站。 網站提供服務的相關資訊，可協助您希望服務，並下載必要的軟體。  
  
 您可以自訂**適用於 Microsoft Online 備份服務登入**工作兩種方式：  
  
-   您可以取代預設的網站 URL 的 URL，表示自訂使用者體驗。 若要取代預設的 URL，請打開作業系統、建立：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**，然後按鍵的值為指定自訂的 URL。  
  
-   您可以隱藏工作。 若要隱藏工作，請打開作業系統和建立：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)