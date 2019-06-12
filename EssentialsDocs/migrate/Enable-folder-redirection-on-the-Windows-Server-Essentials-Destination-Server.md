---
title: 啟用 Windows Server Essentials 目的地伺服器 1 的資料夾重新導向
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: 啟用 Windows Server Essentials 目的地伺服器上的資料夾重新導向
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 50995f0d03b400d6e44d16389afc69e5f25465ac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432977"
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>啟用 Windows Server Essentials 目的地伺服器 1 的資料夾重新導向

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如果已在來源伺服器上啟用資料夾重新導向，您可以執行這項工作。  
  
 首先，使用 Windows Server Essentials 儀表板啟用目的地伺服器上的資料夾重新導向。 然後，刪除舊的 [資料夾重新導向群組原則] 設定。  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>啟用目的地伺服器上的資料夾重新導向  
  
1.  在目的地伺服器上，開啟 Windows Server Essentials 儀表板。  
  
2.  在瀏覽列中，按一下 [裝置]  。  
  
3.  在 [裝置工作]  窗格中，按一下 [實作群組原則]  。  
  
4.  在 [啟用資料夾重新導向群組原則]  頁面上，選取要重新導向的資料夾，然後按一下 [下一步]  。  
  
5.  在 [啟用安全性原則設定]  頁面上，按一下 [完成]  。  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>刪除舊的「資料夾重新導向群組原則」設定  
  
1. 在目的地伺服器上，開啟 [群組原則管理]  系統管理工具。  
  
2. 在 **群組原則管理**，展開**樹系：** <em>YourNetworkDomainName</em>，展開**網域**，依序展開*YourNetworkDomainName*，然後展開**群組原則物件**。  
  
3. 以滑鼠右鍵按一下 [W7PVP 資料夾重新導向]  ，然後按一下 [刪除]  。  
  
4. 閱讀警告，然後按一下 [是]  。  
  
5. 關閉 [群組原則管理]  。  
  
   若要將變更套用至資料夾重新導向，網路使用者必須登出電腦，再重新登入。 這可確保將所有重新導向資料夾都傳輸到目的地伺服器。
