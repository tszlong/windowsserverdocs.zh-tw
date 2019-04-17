---
title: "讓 Windows Server Essentials 目的地 Server1 重新導向資料夾"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: "讓 Windows Server Essentials 目的伺服器上的資料夾重新導向"
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f93d7b28177f96725f2e62c40f9c81cbf186ee6d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>讓 Windows Server Essentials 目的地 Server1 重新導向資料夾

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

如果功能來源伺服器上的資料夾重新導向，您可以執行此工作。  
  
 首先，使用 Windows Server Essentials 儀表板，讓資料夾重新導向目的伺服器上。 然後，delete 舊的資料夾重新導向群組原則設定。  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>若要讓目的伺服器上的資料夾重新導向  
  
1.  在目的伺服器，開放 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [**裝置**。  
  
3.  在**裝置工作**窗格中，按**實作群組原則**。  
  
4.  在**讓資料夾重新導向群組原則**頁面上，選取 [資料夾重新導向，然後按一下 [**下**。  
  
5.  在**支援的安全性原則設定**頁面上，按**完成]**。  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>若要 delete 舊的資料夾重新導向群組原則設定  
  
1.  在目的伺服器，請打開**群組原則管理**系統管理工具。  
  
2.  在**群組原則管理**，展開 [**樹系：***YourNetworkDomainName*，展開 [**網域**，展開 [ *YourNetworkDomainName*，然後展開**群組原則物件**。  
  
3.  以滑鼠右鍵按一下**W7PVP 資料夾重新導向**，然後按**Delete**。  
  
4.  朗讀警告，然後按一下**[是]**。  
  
5.  關閉**群組原則管理**。  
  
 若要套用變更資料夾重新導向，網路使用者必須登入他們的電腦，然後重新登入。 這樣可確保所有重新導向資料夾傳輸到目的伺服器。
