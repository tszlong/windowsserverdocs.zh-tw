---
title: 建立 Hyper-v VHD 設定檔案
description: 在 Hyper-v 2016 上建立 VHDset 檔案的步驟
author: jiwool
ms.author: jiwool
ms.date: 01/26/2017
ms.topic: article
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
ms.openlocfilehash: 7e2c287666115b9a81c6ab5585187f26dad7a5e3
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746463"
---
# <a name="create-hyper-v-vhd-set-files"></a>建立 Hyper-v VHD 設定檔案
VHD 設定檔案是 Windows Server 2016 中來賓叢集的新共用虛擬磁片模型。 VHD 設定檔支援線上調整共用虛擬磁片大小、支援 Hyper-v 複本，而且可以包含在應用程式一致的檢查點中。

VHD 設定檔案會使用新的 VHD 檔案類型。某個. VHD 設定檔案會以中繼資料的形式儲存在來賓叢集中使用之群組虛擬磁片的檢查點資訊。

Hyper-v 會處理管理檢查點鏈和合併共用 VHD 集的所有層面。 管理軟體可以像執行相同的方式，執行磁片作業，例如在 VHD 設定檔上進行線上調整大小。VHDX 檔案。 這表示管理軟體不需要知道 VHD 設定檔案格式。

> [!NOTE]
> 在部署到生產環境之前，請務必先評估 VHD 設定檔的影響。 請確定您的環境中沒有效能或功能降低，例如磁片等待時間。

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>從 Hyper-v 管理員建立 VHD 設定檔案

1.  開啟 Hyper-V 管理員。 按一下 [開始]****，指向 [系統管理工具]****，然後按一下 [Hyper-V 管理員]****。
2.  在 [動作] 窗格中，按一下 [ **新增**]，然後按一下 [ **硬碟**]。
3.  在 [ **選擇磁片格式** ] 頁面上，選取 [ **VHD 設定** 為虛擬硬碟的格式]。
4.  繼續流覽嚮導的頁面，以自訂虛擬硬碟。 您可以按 [ **下一步** ]，在嚮導的每個頁面上移動，也可以按一下左窗格中的頁面名稱，直接移至該頁面。
5.  完成虛擬硬碟的設定之後，請按一下 **[完成]**。

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>從 Windows PowerShell 建立 VHD 設定檔案

使用具有檔案類型的 [新 VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps) Cmdlet。檔案路徑中的 VHD。 此範例會建立名為 base .vhd 的 VHD 集檔案，其大小為 10 Gb。

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>將共用 VHDX 檔案遷移至 VHD 設定檔案

將現有的共用 VHDX 遷移至 VHD 需要讓 VM 離線。 這是使用 Windows PowerShell 的建議程式：

1. 從 VM 移除 VHDX。 例如，執行：
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```

2. 將 VHDX 轉換為 VHD。 例如，執行：
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```

3. 將 VHD 新增至 VM。 例如，執行：
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```