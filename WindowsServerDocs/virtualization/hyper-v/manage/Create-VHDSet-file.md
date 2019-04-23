---
title: 建立 HYPER-V VHD 設定檔案
description: 若要建立 hyper-v 2016 VHDset 檔案的步驟
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: 61f2450857cbeaffd7f75f7b259e9f9de06ba5c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870399"
---
# <a name="create-hyper-v-vhd-set-files"></a>建立 HYPER-V VHD 設定檔案
設定 VHD 檔案是 Windows Server 2016 中的客體叢集的新共用虛擬磁碟模型。 設定 VHD 檔案支援線上調整大小的共用虛擬硬碟、 支援 HYPER-V 複本，並可以包含在應用程式一致檢查點。 

設定 VHD 檔案會使用新的 VHD 檔案類型。VHD。 將 VHD 檔案會儲存用於客體叢集，中繼資料的表單中的群組虛擬磁碟的檢查點資訊。

HYPER-V 會處理管理檢查點鏈結的所有層面，並合併共用的 VHD 設定。 管理軟體可以執行磁碟作業，例如線上調整大小的 VHD 設定檔案上對的方式相同。VHDX 檔案。 這表示管理軟體，不需要知道的 VHD 設定檔案格式。

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>從 HYPER-V 管理員建立的 VHD 設定的檔案

1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。
2.  在 [動作] 窗格中，按一下**的新**，然後按一下**硬碟**。
3.  在 **選擇磁碟格式**頁面上，選取**VHD 設定**做為虛擬硬碟格式。
4.  繼續執行的自訂虛擬硬碟精靈 頁面。 您可以按一下**下一步**移動每個頁面的精靈中，或您可以按一下來直接移至該頁面的左窗格中的頁面名稱。
5.  您已完成設定虛擬硬碟之後，請按一下**完成**。

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>從 Windows PowerShell 中建立的 VHD 設定的檔案

使用[NEW-VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet，與檔案類型。檔案路徑中的 VHD。 這個範例會建立一個名為 base.vhds 大小為 10 gb 為單位的 VHD 設定檔案。

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>將共用的 VHDX 檔案移轉至 VHD 設定的檔案

移轉現有的共用的 VHDX 的 vhd，需要使 VM 離線。 這是使用 Windows PowerShell 的建議程序：

1.  從 VM 移除 VHDX。 例如，執行： 
  ``` PowerShell
  PS c:\>Remove-VMHardDiskDrive existing.vhdx
  ```
  
2.  將 VHDX 轉換為 VHD。 例如，執行：
  ``` PowerShell
  PS c:\>Convert-VHD existing.vhdx new.vhds
  ```
  
3.  將 VHD 新增至 VM。 例如，執行：
  ``` PowerShell
  PS c:\>Add-VMHardDiskDrive new.vhds
  ```
  



