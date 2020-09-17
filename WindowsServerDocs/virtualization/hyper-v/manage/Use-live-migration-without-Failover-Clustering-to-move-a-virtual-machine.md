---
title: 在不使用容錯移轉叢集的情況下，使用即時移轉來移動虛擬機器
description: 提供在獨立環境中進行即時移轉的必要條件和指示。
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/17/2017
ms.openlocfilehash: b02c30a612ef6aa1ed56e1c26e86c21a48b5b138
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746633"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>在不使用容錯移轉叢集的情況下，使用即時移轉來移動虛擬機器

>適用於：Windows Server 2016

本文說明如何在不使用容錯移轉叢集的情況下，進行即時移轉來移動虛擬機器。 即時移轉會在 Hyper-v 主機之間移動執行中的虛擬機器，而不會有任何明顯的停機時間。

若要這樣做，您需要：

- 屬於本機 Hyper-v 系統管理員群組或來源和目的地電腦上 Administrators 群組成員的使用者帳戶。

- Windows Server 2016 或 Windows Server 2012 R2 中的 Hyper-v 角色已安裝在來源和目的地伺服器上，並設定為進行即時移轉。 如果虛擬機器至少為第5版，您可以在執行 Windows Server 2016 和 Windows Server 2012 R2 的主機之間進行即時移轉。

    如需版本升級的指示，請參閱 [Windows 10 或 Windows Server 2016 上的 Hyper-v 升級虛擬機器版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需安裝指示，請參閱 [設定主機以進行即時移轉](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。

- Hyper-v 管理工具安裝在執行 Windows Server 2016 或 Windows 10 的電腦上，除非這些工具安裝在來源或目的地伺服器上，而且您將從該處執行這些工具。

## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>使用 Hyper-v 管理員移動正在執行的虛擬機器

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >> **hyper-v 管理員**]。 ) 

2.  在流覽窗格中，選取其中一個伺服器。  (如果未列出，請以滑鼠右鍵按一下 [ **Hyper-v 管理員**]，按一下 **[連線到伺服器]**，輸入伺服器名稱，然後按一下 **[確定]**。 重複加入更多伺服器。 ) 

3.  在 [ **虛擬機器** ] 窗格中，以滑鼠右鍵按一下虛擬機器，然後按一下 [ **移動**]。 這會開啟 [移動向導]。

4.  使用 [wizard] 頁面選擇移動、目的地伺服器和選項的類型。

5.  在 [摘要]**** 頁面上檢視您的選擇，然後按一下 [完成]****。

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>使用 Windows PowerShell 來移動執行中的虛擬機器

下列範例會使用移動 VM Cmdlet，將名為 *LMTest* 的虛擬機器移至名為 *TestServer02* 的目的地伺服器，並將虛擬硬碟和其他檔案（例如檢查點和智慧分頁檔）移至目的地伺服器上的 *D:\LMTest* 目錄。

```
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest
```

## <a name="troubleshooting"></a>疑難排解

### <a name="failed-to-establish-a-connection"></a>無法建立連接

如果您尚未設定限制委派，您必須先登入來源伺服器，才能移動虛擬機器。 如果您沒有這麼做，驗證嘗試就會失敗，並出現錯誤，並顯示此訊息：

「在遷移來源的虛擬機器遷移作業失敗。
無法建立與主 *電腦名稱稱*的連接：安全性封裝0x8009030E 中沒有可用的認證。」

 若要修正此問題，請登入來源伺服器，然後再次嘗試移動。 若要避免在進行即時移轉之前，必須先登入來源伺服器，請設定限制委派。 您將需要網域系統管理員認證來設定限制委派。 如需相關指示，請參閱 [設定主機以進行即時移轉](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。

 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>因為主機硬體不相容而失敗

 如果虛擬機器未開啟處理器相容性，且有一或多個快照集，則如果主機具有不同的處理器版本，移動就會失敗。 發生錯誤並顯示此訊息：

**虛擬機器無法移至目的地電腦。目的地電腦上的硬體與此虛擬機器的硬體需求不相容。**

 若要修正此問題，請關閉虛擬機器，並開啟處理器相容性設定。

1. 在 [Hyper-v 管理員] 的 [ **虛擬機器** ] 窗格中，以滑鼠右鍵按一下虛擬機器，然後按一下 [設定]。
2. 在流覽窗格中，展開 [ **處理器** ]，然後按一下 [ **相容性**]。
3. 檢查 **遷移至具有不同處理器版本的電腦**。
4. 按一下 [確定]。

   若要使用 Windows PowerShell，請使用 [get-vmprocessor](/powershell/module/hyper-v/set-vmprocessor?view=win10-ps) Cmdlet：

   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```