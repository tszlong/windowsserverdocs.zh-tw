---
title: 疑難排解 Web 探查 URL
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 6dfffd1e-f4f4-43b6-9e3c-49015ce34338
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4ef394d50a3a2e5e09f83f34e695f291e6db4517
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958446"
---
# <a name="troubleshooting-web-probe-urls"></a>疑難排解 Web 探查 URL

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含與 `Set-DAEntryPointDC` 命令問題有關的疑難排解資訊。 若要確認您所收到的錯誤與設定進入點網域控制站有關，請檢查 Windows 事件記錄檔中的事件識別碼 10065。

## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>儲存伺服器 GPO 設定
**收到錯誤**。 將遠端存取設定儲存至 GPO <GPO_name> 時發生錯誤。

若要疑難排解這個錯誤，請參閱儲存伺服器 GPO 設定。

## <a name="remote-access-is-not-configured"></a>未設定遠端存取
**收到錯誤**。 <server_name> 上未設定遠端存取。 請指定屬於多站台部署之伺服器的名稱。

或者

伺服器 <server_name> 上未設定遠端存取。 請指定已啟用 DirectAccess 的電腦。

**原因**

*ComputerName* 參數指定的電腦上未設定遠端存取。

`Set-DaEntryPointDC` Cmdlet 只能在屬於已設定之多站台部署的伺服器上使用。

**方案**

執行命令，並確定將 *ComputerName* 參數指定成已設定為屬於多站台部署之伺服器的名稱。

## <a name="multisite-is-not-enabled"></a>未啟用多站台
**收到錯誤**。 您必須先啟用多網站部署，才能執行此作業。 請使用 `Enable-DAMultiSite` Cmdlet 進行啟用。

**原因**

*ComputerName* 參數指定的伺服器未啟用多站台。

`Set-DaEntryPointDC` Cmdlet 只能在屬於已設定之多站台部署的伺服器上使用。

**方案**

執行命令，並確定將 *ComputerName* 參數指定成已設定為屬於多站台部署之伺服器的名稱。

## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>Cmdlet 中未提供進入點和網域控制站
`Set-DaEntryPointDC` Cmdlet 可讓您變更與不同進入點相關的網域控制站，例如，如果已無法再使用某個特定的網域控制站。 您可以更新特定的進入點以使用不同的網域控制站，也可以更新使用特定網域控制站的所有進入點以使用新的網域控制站。 在第一種情況下，您應該使用 *EntryPointName* 參數指定應該更新的進入點。 在第二種情況下，您應該使用 *ExistingDC* 參數指定應該取代的網域控制站。 您只能指定其中一個參數。

**收到錯誤**。 未指定任何必要的參數。 請提供進入點或現有網域控制站的名稱。

或者

Cmdlet `Set-DaEntryPointDC` 遺失所有必要的參數。

**原因**

*EntryPointName* 或 *ExistingDC* 參數尚未指定，或 `Set-DaEntryPointDC` Cmdlet 同時指定這兩個參數。

**方案**

執行命令，並確認指定 *EntryPointName* 參數或 *ExistingDC* 參數。

## <a name="could-not-locate-domain-controller"></a>找不到網域控制站
**收到錯誤**。 無法自動找到新的網域控制站。 請稍後重試，或確認網域控制站設定是否正確。

**原因**

以 *ComputerName* 參數指定的電腦無法透過 RPC 連線，或網域不包含任何可用的可寫入網域控制站。

**方案**

確認可透過 RPC 存取遠端電腦，而且網域有可用的可寫入網域控制站。 如果網域有可用的可寫入網域控制站，您也可以使用 *NewDC* 參數明確指定其名稱。

## <a name="could-not-connect-to-domain-controller"></a>無法連線到網域控制站

-   **問題 1**

    **收到錯誤**。 無法連線到網域控制站 <domain_controller>。 檢查網路連線能力和伺服器可用性。

    **原因**

    無法連線到網域控制站。 這只會在系統管理員在 *NewDC* 或 *ExistingDC* 參數中指定網域控制站時發生。

    **方案**

    確認網域控制站的名稱拼寫正確。 如果您是使用簡短名稱指定名稱，請使用 FQDN，然後再試一次。

-   **問題 2**

    **收到錯誤**。 無法連絡網域控制站 <domain_controller>。

    **原因**

    可能有網路方面的問題，表示無法連線到 *NewDC* 參數中指定的網域控制站或設定中的任何其他現有網域控制站。

    **方案**

    確認網域控制站的名稱拼寫正確，確認網域控制站確實存在、正在執行、可寫入，而且網域控制站和網域之間存在信任的關係。

-   **問題3**

    **收到錯誤**。 無法到達 %2！ s！的網域控制站 <domain_controller>。

    **原因**

    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 當管理進入點之伺服器 GPO 的網域控制站無法使用時，便無法讀取或修改遠端存取設定。

    **方案**

    遵循2.4 中所述的「變更管理伺服器 Gpo 的網域控制站」程式[。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。

-   **問題4**

    **收到錯誤**。 無法連線到網域 <domain_name> 中的主域控制站。

    **原因**

    為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 用戶端 GPO 是在網域主控站上管理。 如果網域主控站無法使用，便無法讀取或修改遠端存取組態設定。

    **方案**

    遵循2.4 中所述的「傳送 PDC 模擬器角色」程式[。設定 Gpo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs)。

## <a name="read-only-domain-controller"></a>唯讀網域控制站
**收到錯誤**。 <domain_controller> 的網域控制站是唯讀的。 請指定不是唯讀的網域控制站。

**原因**

以 *NewDC* 參數指定的網域控制站是唯讀的網域控制站。

**方案**

使用 `Set-DAEntryPointDC` 時，*NewDC* 參數是用來更新與特定進入點相關的網域控制站，或用來更新與某個網域控制站相關的所有進入點。 因此，新的網域控制站必須是可寫入的網域控制站。 請在 *NewDC* 參數中指定可寫入的網域控制站，然後再試一次。

## <a name="cannot-retrieve-gpo"></a>無法擷取 GPO

-   **問題 1**

    **收到錯誤**。 無法從網域控制站 previous_domain_controller> <取得網域控制站上的 GPO <GPO_name> <replacement_domain_controller>，因為它們不在相同的網域中。

    **原因**

    遠端存取伺服器和網域控制站位於不同的網域，所以無法擷取 GPO。

    **方案**

    如果您嘗試更新特定的進入點，請確認新的網域控制站與進入點伺服器位於相同的網域。 如果您嘗試更新特定的網域控制站，請確認新的網域控制站與您要取代的網域控制站位於相同的網域。

-   **問題 2**

    **收到錯誤**。 無法從網域控制站 previous_domain_controller> <取得網域控制站上的 GPO <GPO_name> <replacement_domain_controller>。 請等候網域複寫完成，然後再試一次。

    **原因**

    嘗試更新進入點網域控制站時，Cmdlet 會嘗試從新的網域控制站讀取伺服器 GPO，但是在新的網域控制站上找不到 GPO，因為尚未複寫。

    **方案**

    新的網域控制站上沒有伺服器 GPO。 請確認 GPO 已經成功複寫到新的網域控制站，然後再試一次。

-   **問題3**

    **收到錯誤**。 您沒有許可權可存取 GPO <GPO_name>。

    **原因**

    嘗試更新進入點網域控制站時，Cmdlet 會嘗試從新的網域控制站讀取伺服器 GPO，但是無法在新的網域控制站上讀取 GPO，因為您沒有適當的權限。

    **方案**

    網域控制站上有 GPO，但無法讀取。 請確認您具有必要的權限，然後再試一次。

## <a name="entry-point-not-part-of-multisite-deployment"></a>進入點不屬於多站台部署
**收到錯誤**。 進入點 <entry_point_name> 不是多網站部署的一部分。 請指定其他值。

**原因**

找不到您指定的進入點名稱。

**方案**

確認進入點名稱拼寫正確，而且 GPO 複寫到必要的網域控制站，然後再試一次。 若要檢視為每個進入點指派的網域控制站，請使用 `Get-DAEntryPointDC`。

## <a name="remote-access-server-settings"></a>遠端存取伺服器設定

-   **問題 1**

    **收到錯誤**。 無法存取進入點 <entry_point_name> 中的伺服器 <server_name>。

    **原因**

    嘗試更新進入點網域控制站時，Cmdlet 會嘗試從所有相關的遠端存取伺服器讀取和寫入進入點網域控制站。 Cmdlet 無法從一或多部遠端存取伺服器讀取資料。

    **方案**

    確認所有相關的遠端存取伺服器都在執行中，而且您具有這些伺服器的本機系統管理員權限，然後再試一次。

-   **問題 2**

    **收到錯誤**。 無法將設定儲存到伺服器 <上的登錄，server_name> 進入點 <entry_point_name>。

    **原因**

    嘗試更新進入點網域控制站時，Cmdlet 會嘗試從所有相關的遠端存取伺服器讀取和寫入進入點網域控制站。 Cmdlet 無法將資料寫入一或多部遠端存取伺服器。

    **方案**

    確認所有相關的遠端存取伺服器都在執行中，而且您具有這些伺服器的本機系統管理員權限，然後再試一次。

-   **問題3**

    **收到錯誤**。 無法在 <server_name> 上套用 GPO 更新。 直到下次重新整理原則之前，變更都不會生效。

    **原因**

    使用 Cmdlet `Set-DAEntryPointDC` 時，指定的 *ComputerName* 參數不是最後一個新增到多站台部署之進入點的遠端存取伺服器。

    **方案**

    使用遠端存取管理主控台的 [儀表板]**** 中的 [設定狀態]**** 可以看到所有未更新的伺服器。 這不會造成任何功能上的問題；但是您可以在任何未更新的伺服器上執行 `gpupdate /force`，立即更新設定狀態。

## <a name="problem-resolving-fqdn"></a>解析 FQDN 時發生問題
**收到錯誤**。 無法存取進入點 <entry_point_name> 中的伺服器 <server_name>。

**原因**

取得要修改的 DirectAccess 伺服器清單時，Cmdlet 無法從其電腦 SID 解析其中一部伺服器的完整網域名稱 (FQDN)。

**方案**

錯誤訊息中指定的進入點與網域控制站相關。 請確認進入點可以使用網域控制站。 如果從網域移除指定的 SID 所屬的電腦，請忽略這個訊息，然後從多站台部署移除伺服器。

## <a name="no-entry-points-to-update"></a>沒有可更新的進入點
**收到警告**。 未修改網域控制站設定。 如果您認為有必要變更，請確定正確設定了 Cmdlet 參數，而且 GPO 也複寫到必要的網域控制站。

**原因**

以 `Set-DaEntryPointDC` 參數呼叫 ** Cmdlet 時，DirectAccess 會檢查所有進入點，並更新與指定的網域控制站相關聯的進入點。 不過，沒有任何進入點使用指定的 *ExistingDC*。

**方案**

若要查看進入點及其相關聯網域控制站的清單，請使用 `Get-DAEntryPointDC` Cmdlet。 如果應該進行變更，請確認 Cmdlet 參數拼寫正確，而且 GPO 複寫到必要的網域控制站，然後再試一次。



