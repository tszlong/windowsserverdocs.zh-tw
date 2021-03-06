---
title: 設定或自訂伺服器備份
description: 瞭解如何藉由排程每日備份來自動保護您的伺服器和其資料。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a6f31b4e9db5ce3aa2e74f42441e0de0d863a43c
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811065"
---
# <a name="set-up-or-customize-server-backup"></a>設定或自訂伺服器備份

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 在安裝期間不會自動設定伺服器備份。 您應該排定每日備份來自動保護您的伺服器及其資料。 建議您維持一個每日備份計劃，因為大多數組織皆無法承受失去數天所建立之資料的損失。

 請參閱下列各節，以設定或自訂伺服器備份：

-   [設定或變更伺服器備份設定](Set-up-or-customize-server-backup.md#BKMK_1)  &reg;

-   [伺服器備份排程](Set-up-or-customize-server-backup.md#BKMK_2)

-   [備份目標磁碟機](Set-up-or-customize-server-backup.md#BKMK_Target)

-   [要備份的項目](Set-up-or-customize-server-backup.md#BKMK_4)

##  <a name="set-up-or-change-server-backup-settings"></a><a name="BKMK_1"></a> 設定或變更伺服器備份設定

#### <a name="to-set-up-or-change-server-backup-settings"></a>設定或變更伺服器備份設定

1.  開啟 [儀表板]，然後按一下 [裝置] 索引標籤。

2.  在 [清單] 檢視中，按一下您的伺服器以選取。

3.  在 [工作] 窗格中，按一下 [設定伺服器的備份]。

    > [!NOTE]
    >  如果您要變更現有的備份設定，請按一下 [自訂伺服器的備份]。

4.  遵循精靈的指示進行。

    > [!NOTE]
    >  如果您先啟動精靈才將外部硬碟連接至伺服器，請在連接硬碟後，按一下 [選取備份目的地] 頁面上的 [重新整理清單]。

> [!NOTE]
>  在預設安裝的 Windows Server Essentials 中，伺服器會設定為每週自動執行一次磁碟重組。 如果您使用非 Microsoft 映像處理軟體，這可能會導致比一般備份還要大。 如果沒有必要定期進行伺服器磁碟重組，您可以依照下列步驟關閉磁碟重組排程：
>
> 1. 按 Windows 鍵 + W 鍵來開啟 [搜尋]。
>    2. 在 [搜尋] 文字方塊中，輸入 **Defragment**。
>    3. 在 [結果] 區段中，按一下 [重組並最佳化磁碟機]。
>    4. 在 [最佳化磁碟機] 頁面中，選取一個磁碟機，然後按一下 [變更設定]。
>    5. 在 [最佳化排程] 視窗中，取消選取 [依排程執行 (建議)] 核取方塊，然後按一下 [確定] 來儲存變更。

##  <a name="server-backup-schedule"></a><a name="BKMK_2"></a> 伺服器備份排程
 當您使用「設定伺服器備份精靈」或「自訂伺服器備份精靈」時，可以選擇在一日內多次備份伺服器資料。 因為精靈排程的是增量備份，因此備份執行地很迅速，而且不會大幅影響伺服器效能。 精靈預設會排程在每日中午 12:00 與晚上 11:00 執行備份。 不過，您可以根據您組織的需求調整備份排程。 您應該偶爾評估您備份計劃的有效性，然後視需要變更計劃。

##  <a name="backup-target-drive"></a><a name="BKMK_Target"></a> 備份目標磁片磁碟機
 您可以使用多個外部存放磁碟機進行備份，然後讓這些磁碟機在站上與離站位置之間輪替。 這樣可在站上硬體發生實體損害時協助您復原資料，藉以改善您的災害防範規劃。

 為您的伺服器備份選擇存放磁碟機時，請考慮下列各項：

-   選擇有足夠空間來儲存資料的磁碟機。 存放磁碟機的儲存容量應該至少是您想要備份之資料的 2.5 倍。 磁碟機也應該大到足以應付您伺服器資料未來的成長。

-   使用外部存放磁碟機時，請確定磁碟機是空的或者只包含您不需要的資料。

    > [!CAUTION]
    >  [設定伺服器備份精靈] 會在設定存放磁碟機來進行備份時，將這些磁碟機格式化。

-   如果備份目標磁碟機包含離線磁碟機，備份設定將不會成功。 若要完成設定，請在選取備份目標時，清除核取方塊以排除離線的磁碟機。

-   如果您選擇包含先前備份的磁碟機做為備份目標，精靈會讓您選擇是否要保留先前的備份。 如果您要保留那些備份，精靈就不會將磁碟機格式化。

-   您應該造訪外部存放磁片磁碟機製造商的網站，以確保在執行 Windows Server Essentials 的電腦上支援備份磁片磁碟機。

-   磁碟機不能包含「可延伸韌體介面 (EFI)」系統磁碟分割。 如果 USB 磁碟機上有 EFI 磁碟分割，則會假設該磁碟是開機磁碟。 如果您確定不需要磁片上的資料，您可以將磁片重新格式化，並將其用於備份。

    > [!CAUTION]
    >  當您重新格式化磁碟時，所有資料都會被刪除。

    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>移除 USB 磁碟的 EFI 系統磁碟分割

    1.  在 [控制台] 中開啟 [系統及安全性]。

    2.  在 [系統管理工具] 下，按一下 [建立及格式化硬碟磁碟分割]。

    3.  刪除 USB 磁碟上的所有磁碟區，或者只刪除 EFI 磁碟分割。 「設定伺服器備份精靈」會刪除所有磁碟區。

-   磁碟機不能包含任何共用的伺服器資料夾。 您必須先停止共用任何共用的伺服器資料夾，才能使用該磁碟做為備份目標磁碟機。 您可以從儀表板或在檔案總管中停止共用。

    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>從儀表板停止共用伺服器資料夾

    1.  在 [儀表板] 上，按一下 [存放裝置]，然後按一下 [伺服器資料夾]。

    2.  選取您想要停止共用的資料夾，然後在工作窗格中，按一下 [停止]。

> [!NOTE]
>  如果備份因為磁碟空間不足而失敗，則會從 Windows Server Essentials 資料庫中移除備份目標磁片磁碟機的磁碟機號，而儀表板不會顯示該磁片磁碟機。 如果您想要在未來的備份中使用該磁碟機，您必須使用原生工具重新指派磁碟機代號。
>
>  **重新指派現有磁碟區的磁碟機代號**
>
> 1. 在 [控制台] 中開啟 [系統及安全性]。
>    2. 在 [系統管理工具] 下，按一下 [建立及格式化硬碟磁碟分割]。
>    3. 以滑鼠右鍵按一下磁碟機，然後按一下 [變更磁碟機代號及路徑]。
>    4. 按一下 [新增] 。
>    5. 在 [新增磁碟機代號或路徑] 對話方塊中，選取要指派的磁碟機代號。  (您可以重新指派相同的磁碟機號。 ) 然後按一下 **[確定]**。
>
>    磁碟機會立即出現在儀表板上。

##  <a name="items-to-be-backed-up"></a><a name="BKMK_4"></a> 要備份的專案
 您可以選擇備份伺服器上所有的磁碟機、檔案和資料夾，或選取個別的磁碟機、檔案或資料夾進行備份。

 當您新增或移除磁碟機，或者新增或移除共用的檔案和資料夾時，您應重新檢查伺服器備份設定，以確定這些項目加入備份設定或從備份設定中移除。 若要新增或移除要備份的項目，請執行下列其中一個動作：

- 若要在伺服器備份中包含資料磁碟機，請選取相鄰的核取方塊

- 若要從伺服器備份中排除資料磁碟機，請清除相鄰的核取方塊

  > [!NOTE]
  >  如果您想要將 [作業系統] 項目從備份中排除，就必須先取消選取 [系統備份 (建議選項)] 核取方塊。

  若要減少伺服器備份所使用的伺服器儲存容量，您可以排除任何包含不太重要檔案的資料夾。

  例如，您可能有資料夾包含使用大量硬碟空間的錄製電視節目。 您可以選擇不要備份這些檔案，因為您通常在看過它們之後就會將它們刪除。 或者，您可能有一個資料夾包含您不想保留的暫存檔。

## <a name="additional-references"></a>其他參考資料

-   [管理伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md)

-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
