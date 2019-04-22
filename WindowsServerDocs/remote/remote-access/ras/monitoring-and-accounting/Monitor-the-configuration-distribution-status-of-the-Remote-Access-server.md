---
title: 監視「遠端存取」伺服器的設定發佈狀態
description: 本主題是適用於遠端存取 」 監視和指南 Windows Server 2016 中的帳戶處理的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3171eb9e9c90d0688fa413b80d9dbbf162e77fe8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818419"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>監視「遠端存取」伺服器的設定發佈狀態

>適用於：Windows Server （半年通道），Windows Server 2016

**注意：** Windows Server 2012 將 DirectAccess 和遠端存取服務 (RAS) 結合成單一的遠端存取角色中。  
  
「遠端存取管理主控台」會比較所有受監視伺服器的設定版本，以確認它們符合且使用最新的設定版本。 這會顯示最新的設定版本 (在「群組原則物件」或 GPO 中指定) 是否已散佈到所有伺服器，以及是否已成功套用在伺服器上。  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>使用監視儀表板來監視設定散佈  
  
1.  在 [伺服器管理員] 中，按一下 [工具]，然後按一下 [遠端存取管理]。  
  
2.  按一下 [儀表板] 以瀏覽到 [遠端存取管理主控台] 中的 [遠端存取儀表板]。  
  
3.  在監視儀表板上，注意中央頂端的 [設定狀態] 磚。 這個磚會顯示目前的設定散佈狀態。  
  
下表顯示 [設定狀態] 磚所產生的訊息、它們的意義，以及必要的系統管理動作 (如果有的話)。  
  
|||||  
|-|-|-|-|  
|嚴重性|訊息|意義|該怎麼辦？|  
|成功|已成功散佈設定。|已將 GPO 中的設定成功套用在伺服器上。|不需要採取任何動作。|  
|警告|未從網域控制站抓取伺服器 [伺服器名稱] 的設定。 未連結 GPO。|GPO 中的設定尚未送達伺服器。 這可能是因為 GPO 未連結到伺服器。|將 GPO 連結到套用至伺服器的管理領域，或是在暫存 GPO 案例中，手動將設定從暫存 GPO 中匯出，再匯入到實際執行 GPO 中。 如需有關暫存 Gpo 的詳細資訊，請參閱 <<c0>  **有限權限管理遠端存取 Gpo**中[Step-1-Plan-the-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md)。 如需 GPO 暫存步驟，請參閱**有限權限設定遠端存取 Gpo**在[步驟 1:設定 DirectAccess 基礎結構](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md)。|  
|警告|尚未從網域控制站抓取伺服器 [伺服器名稱] 的設定。|GPO 中的設定尚未送達伺服器。<br /><br />最多可能需要花 10 分鐘才能傳播新設定。|允許更多時間來更新伺服器上的原則。|  
|錯誤|無法從網域控制站抓取伺服器 [伺服器名稱] 的設定。|GPO 中的設定未送達伺服器，並且自變更設定後已經超過 10 分鐘。|在下列其中一種情況下就可能發生這種情形：<br /><br />-伺服器有沒有連線到網域來更新此原則。 您可以強制執行原則的更新伺服器上執行"gpupdate /force"。<br />-GPO 複寫，可能必須擷取更新的組態。<br />-沒有可寫入網域控制站中遠端存取伺服器的 Active Directory 站台。<br /><br />等候 GPO 複寫到所有的網域控制站，然後使用 Windows PowerShell Cmdlet **Set-DAEntryPointDC**，將進入點與「遠端存取」伺服器之 Active Directory 中的可寫入網域控制站建立關聯。|  
|警告|已從網域控制站抓取伺服器 [伺服器名稱] 的設定，但尚未套用。|GPO 中的設定已送達伺服器，但尚未套用。<br /><br />最多可能需要花 15 分鐘才能套用設定。|允許更多時間來將設定完整套用到伺服器。|  
|錯誤|無法套用從網域控制站抓取的伺服器 [伺服器名稱] 設定。|GPO 中的設定已送達伺服器，但是未成功套用，並且自變更設定後已經超過 15 分鐘。|在下列其中一種情況下就可能發生這種情形：<br /><br />1.目前正在套用設定。 這會顯示為錯誤，因為可能已經花費很長的時間從 GPO 抓取設定。<br />    若要確認這是否為原因，請使用 [工作排程器] 並瀏覽到 Microsoft\Windows\RemoteAccess 以確認 **RAConfigTask** 目前是否正在執行。<br />2.如果 **RAConfigTask** 目前未執行，表示它可能無法將設定套用在伺服器上。<br />    檢查 [事件檢視器]  中「遠端存取」伺服器操作通道 (位於 \Applications and Services Logs\Microsoft\Windows\RemoteAccess-RemoteAccessServer) 底下是否有錯誤。<br />    檢查 [遠端存取管理主控台] 的 [操作狀態] 中是否有錯誤。 如需詳細資訊，請參閱[監視遠端存取伺服器及其元件的操作狀態](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)。|  
|錯誤|已從網域控制站抓取多站台伺服器的設定。 這個設定並未在所有伺服器上都相符。|多站台部署中的伺服器 GPO 設定版本之間不一致。<br /><br />在理想的情況下，所有進入點的所有伺服器 GPO 都會有相同的全域設定，但是因為某種原因，它們並未同步。|當變更設定失敗且未成功回復時，便可能發生這種情況。<br /><br />您應該將 GPO 從備份狀態 (所有伺服器 GPO 皆已同步) 復原。 您可以使用指令碼的相關資訊，請參閱[備份和還原遠端存取設定](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)。|  
  


