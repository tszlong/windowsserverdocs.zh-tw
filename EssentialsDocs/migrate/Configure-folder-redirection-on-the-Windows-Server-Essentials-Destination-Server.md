---
title: 在 Windows Server Essentials 目的地伺服器設定資料夾重新導向
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: bc67b6bae8492d7e176ec5e46d268d171ad7c94a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622935"
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>在 Windows Server Essentials 目的地伺服器設定資料夾重新導向

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

如果已在來源伺服器上啟用資料夾重新導向，請執行這項工作。

 首先，刪除舊的「資料夾重新導向群組原則」設定。 然後使用 [Windows Server Essentials 儀表板] 在目的地伺服器上啟用資料夾重新導向。

### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>刪除舊的「資料夾重新導向群組原則」設定

1. 在目的地伺服器上，開啟 [群組原則管理]**** 系統管理工具。

2. 在 **群組原則管理**] 中，展開 [ **樹系：**<em>YourNetworkDomainName</em>]，展開 [ **網域**]，展開 [ *YourNetworkDomainName*]，然後展開 [ **群組原則物件**]。

3. 以滑鼠右鍵按一下 [SBS 群組原則資料夾重新導向]****，然後按一下 [刪除]****。

4. 以滑鼠右鍵按一下 [SBS 群組原則安全性範本]****，然後按一下 [刪除]****。

5. 閱讀警告，然後按一下 [是]****。

6. 關閉 [群組原則管理]  。

### <a name="to-enable-folder-redirection-on-the-destination-server"></a>啟用目的地伺服器上的資料夾重新導向

1. 在目的地伺服器上，開啟 [Windows Server Essentials 儀表板]。

2. 在瀏覽列中，按一下 [裝置]****。

3. 在 [裝置工作]**** 窗格中，按一下 [實作群組原則]****。

4. 在 [啟用資料夾重新導向群組原則]**** 頁面上，選取要重新導向的資料夾，然後按一下 [下一步]****。

5. 在 [啟用安全性原則設定]**** 頁面上，按一下 [完成]****。

   若要將變更套用至資料夾重新導向，網路使用者必須登出電腦，再重新登入。 這可確保將所有重新導向資料夾都傳輸到目的地伺服器。
