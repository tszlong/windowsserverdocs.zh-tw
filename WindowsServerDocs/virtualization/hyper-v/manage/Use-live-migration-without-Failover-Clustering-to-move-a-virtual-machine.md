---
title: 使用沒有容錯移轉叢集的即時移轉來移動虛擬機器
description: 必要條件和指示，以執行即時移轉獨立環境中的提供。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 9be61fbc860e9d8c5cbc020d6dd4082722e32509
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812094"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>使用沒有容錯移轉叢集的即時移轉來移動虛擬機器

>適用於：Windows Server 2016

這篇文章會示範如何不使用容錯移轉叢集執行即時移轉移動虛擬機器。 即時移轉而不需要任何顯著的停機時間的 HYPER-V 主機之間移動執行中虛擬機器。   
  
若要能夠執行這項操作，您將需要：   

- 使用者帳戶是本機 HYPER-V Administrators 群組或在來源和目的地電腦上的 Administrators 群組的成員。 
  
- 在 Windows Server 2016 或 Windows Server 2012 R2 中的 HYPER-V 角色安裝在來源和目的地伺服器上，並設定為即時移轉。 您可以執行即時移轉虛擬機器至少為執行 Windows Server 2016 和 Windows Server 2012 R2 的主機之間第 5 版。

    版本升級的指示，請參閱[更新虛擬機器版本，在 Windows 10 或 Windows Server 2016 的 HYPER-V 中](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需安裝指示，請參閱[設定即時移轉的主機](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。

- 除非來源或目的地伺服器上安裝工具，在執行 Windows Server 2016 或 Windows 10 的電腦上安裝 HYPER-V 管理工具會將它們從該處執行。  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>使用 HYPER-V 管理員，移動執行中虛擬機器  
  
1.  開啟 \[Hyper-V 管理員\]。 (從 [伺服器管理員] 中，按一下**工具** >>**HYPER-V 管理員**。)  
  
2.  在 [導覽] 窗格中，選取其中一個伺服器。 (如果未列出，以滑鼠右鍵按一下**HYPER-V 管理員**，按一下**連接到伺服器**，輸入伺服器名稱，然後按一下**確定**。 若要新增更多伺服器重複。）  
  
3.  從**虛擬機器**窗格中，以滑鼠右鍵按一下 虛擬機器，然後按一下**移動**。 這會開啟 [移動精靈]。 
  
4.  您可以使用精靈頁面來選擇移動、 目的地伺服器和選項的類型。
  
5.  在 [摘要]  頁面上檢視您的選擇，然後按一下 [完成]  。  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>使用 Windows PowerShell 來移動執行中虛擬機器
  
下列範例會使用 MOVE-VM cmdlet，將名為虛擬機器移*LMTest*到目的地伺服器命名*TestServer02*並將虛擬硬碟及其他檔案，這類的檢查點和為智慧型分頁檔*D:\LMTest*目的地伺服器上的目錄。  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>疑難排解

### <a name="failed-to-establish-a-connection"></a>無法建立連線 

如果您尚未設定限制委派，您必須登入來源伺服器之前可以移動虛擬機器。 如果您不這樣做，驗證嘗試失敗，發生錯誤，並會顯示此訊息：  
  
「 無法位於移轉來源的虛擬機器移轉作業。  
無法建立與主機的連線*電腦名稱*:無認證可用於安全性套件 0x8009030E。 」
  
 若要修正此問題，請在登入來源伺服器，然後再試移動一次。 若要避免登入來源伺服器進行即時移轉之前，設定限制委派。 您必須設定限制委派的網域系統管理員認證。 如需相關指示，請參閱 <<c0> [ 設定即時移轉的主機](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>失敗，因為主機硬體不相容
 
 如果虛擬機器沒有開啟的處理器相容性，而且具有一或多個快照集，如果主機具有不同處理器版本，也將會失敗的移動。 發生錯誤，而且會顯示此訊息：
 
**虛擬機器無法移至目的地電腦。在目的地電腦上的硬體不相容，此虛擬機器的硬體需求。**
 
 若要修正此問題，請關閉虛擬機器並開啟處理器相容性設定。
 
1. 從 HYPER-V 管理員] 中，在**虛擬機器**] 窗格中，以滑鼠右鍵按一下 [虛擬機器，然後按一下 [設定。
2. 在 [導覽] 窗格中，依序展開**處理器**然後按一下**相容性**。
3. 請檢查**移轉至具有不同處理器版本的電腦**。
4. 按一下 [確定]  。
 
   若要使用 Windows PowerShell，使用[Set-vmprocessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet:
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```