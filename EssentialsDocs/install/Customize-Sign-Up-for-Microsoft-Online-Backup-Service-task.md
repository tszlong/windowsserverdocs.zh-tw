---
title: 自訂註冊 Microsoft Online Backup Service 工作
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879929"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>自訂註冊 Microsoft Online Backup Service 工作

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

依照預設，儀表板的 **[裝置]** 標籤上的 **[註冊 Microsoft Online Backup Service]** 工作會開啟 Microsoft Online Backup Service 網站。 該網站提供服務相關資訊，並協助您訂閱服務及下載所需的軟體。  
  
 您可以用兩種方式自訂 **[註冊 Microsoft Online Backup Service]** 工作：  
  
-   您可以將預設網站的 URL 取代為代表自訂使用者經驗的 URL。 若要取代預設的 URL，請開啟登錄編輯程式，並建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**，然後指定自訂的 URL 做為機碼的值。  
  
-   您可以隱藏工作。 若要隱藏工作，請開啟登錄編輯程式，並建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)