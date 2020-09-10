---
title: 自訂註冊 Microsoft Online Backup Service 工作
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c424be2d0815fbcb217e0453b27f4b7aceb07ee6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623632"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>自訂註冊 Microsoft Online Backup Service 工作

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

依照預設，儀表板的 **[裝置]** 標籤上的 **[註冊 Microsoft Online Backup Service]** 工作會開啟 Microsoft Online Backup Service 網站。 該網站提供服務相關資訊，並協助您訂閱服務及下載所需的軟體。

 您可以用兩種方式自訂 **[註冊 Microsoft Online Backup Service]** 工作：

-   您可以將預設網站的 URL 取代為代表自訂使用者經驗的 URL。 若要取代預設 URL，請開啟登錄編輯程式，建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**，然後將自訂的 URL 指派為機碼值。

-   您可以隱藏工作。 若要隱藏工作，請開啟登錄編輯程式，並建立登錄機碼：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**。

## <a name="see-also"></a>另請參閱
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)