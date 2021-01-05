---
title: 步驟 5：在進行 Windows Server Essentials 移轉的目的地伺服器上啟用資料夾重新導向
description: 瞭解如何在目的地伺服器上啟用資料夾重新導向，以進行 Windows Server Essentials 遷移。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4cbb8ae2fed9398d21e128cb8f14646ff7b898f2
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810425"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>步驟 5：在進行 Windows Server Essentials 移轉的目的地伺服器上啟用資料夾重新導向

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials

如果來源伺服器上已啟用資料夾重新導向，您可以在目的地伺服器啟用資料夾重新導向，然後刪除舊的資料夾重新導向群組原則設定。

 首先，使用 [Windows Server Essentials 儀表板] 在目的地伺服器上啟用資料夾重新導向。 然後，刪除舊的 [資料夾重新導向群組原則] 設定。

### <a name="to-enable-folder-redirection-on-the-destination-server"></a>啟用目的地伺服器上的資料夾重新導向

1.  在目的地伺服器上，開啟 [Windows Server Essentials 儀表板]。

2.  在瀏覽列中，按一下 [裝置]。

3.  在 [裝置工作] 窗格中，按一下 [實作群組原則]。

4.  在 [啟用資料夾重新導向群組原則] 頁面上，選取要重新導向的資料夾，然後按一下 [下一步]。

5.  在 [啟用安全性原則設定] 頁面上，按一下 [完成]。

### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>刪除舊的「資料夾重新導向群組原則」設定

1. 在目的地伺服器上，開啟 [群組原則管理] 系統管理工具。

2. 在 **群組原則管理**] 中，展開 [ **樹系：**<em>YourNetworkDomainName</em>]，展開 [ **網域**]，展開 [ *YourNetworkDomainName*]，然後展開 [ **群組原則物件**]。

3. 以滑鼠右鍵按一下您想要刪除的原則，然後按一下 [刪除]。

4. 閱讀警告，然後按一下 [是]。

5. 關閉 [群組原則管理]  。

   若要套用資料夾重新導向的變更，網路使用者必須登出電腦，然後再重新登入。 這可確保將所有重新導向資料夾都傳輸到目的地伺服器。

## <a name="next-steps"></a>後續步驟
 您已經啟用目的地伺服器上的資料夾重新導向。 現在移至 [步驟6：從新的 Windows Server Essentials 網路降級和移除來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。


若要查看所有步驟，請參閱 [遷移至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

