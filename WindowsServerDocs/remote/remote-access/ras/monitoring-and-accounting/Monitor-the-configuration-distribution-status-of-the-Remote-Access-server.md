---
title: 監視「遠端存取」伺服器的設定散佈狀態
description: 本主題是 Windows Server 2016 中遠端存取監視和帳戶處理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 2f38ed8abb4869cf7c9a917a27fdbfe37bf7d453
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947334"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>監視「遠端存取」伺服器的設定散佈狀態

>適用於：Windows Server (半年度管道)、Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 與遠端存取服務 (RAS) 合併為單一遠端存取角色。

「遠端存取管理主控台」會比較所有受監視伺服器的設定版本，以確認它們符合且使用最新的設定版本。 這會顯示最新的設定版本 (在「群組原則物件」或 GPO 中指定) 是否已散佈到所有伺服器，以及是否已成功套用在伺服器上。

### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>使用監視儀表板來監視設定散佈

1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。

2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。

3.  在監視儀表板上，注意中央頂端的 [設定狀態] 磚。 這個磚會顯示目前的設定散佈狀態。

下表顯示 [設定狀態] 磚所產生的訊息、它們的意義，以及必要的系統管理動作 (如果有的話)。

|嚴重性|訊息|意義|怎麼辦？|
|--|--|--|
|Success|已成功散佈設定。|已將 GPO 中的設定成功套用在伺服器上。|不需採取動作。|
|警告|未從網域控制站抓取伺服器 [伺服器名稱] 的設定。 未連結 GPO。|GPO 中的設定尚未送達伺服器。 這可能是因為 GPO 未連結到伺服器。|將 GPO 連結到套用至伺服器的管理領域，或是在暫存 GPO 案例中，手動將設定從暫存 GPO 中匯出，再匯入到實際執行 GPO 中。 如需有關暫存 Gpo 的詳細資訊，請參閱 **使用有限的版權管理遠端存取 gpo** ---------- [-----------](../../directaccess/single-server-advanced/da-adv-plan-s1-infrastructure.md) 如需 GPO 暫存步驟，請參閱在 [步驟1：設定 DirectAccess 基礎結構中設定](../../directaccess/single-server-advanced/da-adv-configure-s1-infrastructure.md)**有限許可權的遠端存取 gpo** 。|
|警告|尚未從網域控制站抓取伺服器 [伺服器名稱] 的設定。|GPO 中的設定尚未送達伺服器。<p>最多可能需要花 10 分鐘才能傳播新設定。|允許更多時間來更新伺服器上的原則。|
|錯誤|無法從網域控制站抓取伺服器 [伺服器名稱] 的設定。|GPO 中的設定未送達伺服器，並且自變更設定後已經超過 10 分鐘。|在下列其中一種情況下就可能發生這種情形：<p>-伺服器無法連線到網域來更新原則。 您可以在伺服器上執行 "gpupdate/force" 來強制執行原則更新。<br />-必須有 GPO 複寫才能取得更新的設定。<br />-遠端存取服務器的 Active Directory 網站中沒有可寫入的網域控制站。<p>等候 GPO 複寫到所有的網域控制站，然後使用 Windows PowerShell Cmdlet **Set-DAEntryPointDC**，將進入點與「遠端存取」伺服器之 Active Directory 中的可寫入網域控制站建立關聯。|
|警告|已從網域控制站抓取伺服器 [伺服器名稱] 的設定，但尚未套用。|GPO 中的設定已送達伺服器，但尚未套用。<p>最多可能需要花 15 分鐘才能套用設定。|允許更多時間來將設定完整套用到伺服器。|
|錯誤|無法套用從網域控制站抓取的伺服器 [伺服器名稱] 設定。|GPO 中的設定已送達伺服器，但是未成功套用，並且自變更設定後已經超過 15 分鐘。|在下列其中一種情況下就可能發生這種情形：<p>1. 設定目前正在套用中。 這會顯示為錯誤，因為可能已經花費很長的時間從 GPO 抓取設定。<br />    若要確認這是否為原因，請使用 [工作排程器] 並瀏覽到 Microsoft\Windows\RemoteAccess 以確認 **RAConfigTask** 目前是否正在執行。<br />2. 如果 **RAConfigTask** 目前不在執行中，它可能無法在伺服器上套用設定。<br />    檢查 [事件檢視器]  中「遠端存取」伺服器操作通道 (位於 \Applications and Services Logs\Microsoft\Windows\RemoteAccess-RemoteAccessServer) 底下是否有錯誤。<br />    檢查 [遠端存取管理主控台] 的 [操作狀態] 中是否有錯誤。 如需詳細資訊，請參閱[監視遠端存取伺服器及其元件的操作狀態](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)。|
|錯誤|已從網域控制站抓取多站台伺服器的設定。 這個設定並未在所有伺服器上都相符。|多站台部署中的伺服器 GPO 設定版本之間不一致。<p>在理想的情況下，所有進入點的所有伺服器 GPO 都會有相同的全域設定，但是因為某種原因，它們並未同步。|當變更設定失敗且未成功回復時，便可能發生這種情況。<p>您應該將 GPO 從備份狀態 (所有伺服器 GPO 皆已同步) 復原。 如需您可以使用之腳本的相關資訊，請參閱 [備份和還原遠端存取設定](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)。|