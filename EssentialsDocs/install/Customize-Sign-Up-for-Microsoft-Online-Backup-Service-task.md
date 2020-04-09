---
title: 自訂註冊 Microsoft Online Backup Service 工作
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3318ca3937cdc054889121a830bc3eafec4d6acf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818081"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>自訂註冊 Microsoft Online Backup Service 工作

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

依照預設，儀表板的 **[裝置]** 標籤上的 **[註冊 Microsoft Online Backup Service]** 工作會開啟 Microsoft Online Backup Service 網站。 該網站提供服務相關資訊，並協助您訂閱服務及下載所需的軟體。  
  
 您可以用兩種方式自訂 **[註冊 Microsoft Online Backup Service]** 工作：  
  
-   您可以將預設網站的 URL 取代為代表自訂使用者經驗的 URL。 若要取代預設 URL，請開啟登錄編輯程式，建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**，然後將自訂的 URL 指派為機碼值。  
  
-   您可以隱藏工作。 若要隱藏工作，請開啟登錄編輯程式，並建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)