---
title: 更新 Nano Server
description: 深入了解：更新 Nano Server
manager: DonGill
ms.date: 09/06/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 124f8d4ddd406b108588c82652ea3f3577ebf9c9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045166"
---
# <a name="updating-nano-server"></a>更新 Nano Server

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。

Nano Server 提供各種方法讓您保持在最新狀態。 相較於 Windows Server 其他安裝選項，Nano Server 遵循更為主動的服務模型，類似 Windows 10。 這些定期版本稱為 **最新商務分支 (CBB)** 版本。 這種方式可支援想要更迅速創新，並以雲端發佈頻率跟上快速開發週期的客戶。 您可以從 [Windows Server 部落格](https://cloudblogs.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/)上取得更多 CBB 的相關資訊。

**在這些 CBB 版本之間**，Nano Server 會以一系列的 *累積更新* 保持在最新狀態。 例如，Nano Server 的第一個累積更新發佈於 2016 年 9 月 26 日，搭配 [KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)。 我們提供在 Nano Server 上安裝此一更新和後續累積更新的各種選項。 在本文中，我們將使用 KB3192366 更新做為範例，說明如何取得並將累積更新套用到 Nano Server。 如需有關累積更新模型的詳細資訊，請參閱 [Microsoft Update 部落格](/archive/blogs/mu/patching-with-windows-server-2016)。

> [!NOTE]
> 如果您是從媒體或線上存放庫安裝選用的 Nano Server 套件，其並不會包含最新的安全性問題修正。 若要避免選用套件和基本作業系統之間的版本不符，您應該在安裝任何選用套件之後和重新啟動伺服器 **之前**，立即安裝最新的累積更新。

在 Windows Server 2016：2016 年 9 月 26 日 ([KB3192366](https://support.microsoft.com/kb/3192366)) 的累計更新案例中，您應該先針對 Windows 10 版本 1607：2016 年 8 月 23 日安裝最新的服務堆疊更新，以作為先決條件 ([KB3176936](https://support.microsoft.com/kb/3176936))。 下方大部分選項，您都需要包含 .cab 更新套件的.msu 檔案。 請造訪 Microsoft Update Catalog 下載每個更新套件︰
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

從 Microsoft Update Catalog 下載 .msu 檔案後，請將其儲存到網路共用或本機目錄，例如 C:\ServicingPackages。 您可以根據 KB 號碼來重新命名 .msu 檔案，如下方所示，將可更容易識別這些檔案。 然後使用 Expand 公用程式將 .msu 檔案中的 .cab 檔案解壓縮進獨立的目錄，再將這些 .cab 檔案複製到單一資料夾中。

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

現在您可以根據需求，使用數個不同的方式透過已解壓縮的 .cab 檔案將更新套用至 Nano Server 映像。 下列選項並非以特定的偏好順序顯示，請使用最適合您環境的選項。

> [!NOTE]
> 使用 DISM 工具服務 Nano Server 時，您必須使用與正在服務的 Nano Server 版本相同或較新版本的 DISM 版本。 您可以藉由從相同版本的 Windows 執行 DISM、安裝相同版本的 [Windows 評定及部署套件 (ADK)](https://developer.microsoft.comwindows/hardware/windows-assessment-deployment-kit)，或在 Nano Server 本身執行 DISM 來達到此目的。

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>選項 1：整合累積更新至新的映像
如果您正在建立新的 Nano Server 映像，您可以直接整合最新的累積更新到映像中，讓其在第一次開機時就已經完全修補。

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>選項 2：整合累積更新至現有的映像
如果您已有作為建立 Nano Server 特定執行個體基準的 Nano Server 映像，您可以直接整合最新的累積更新到基準映像中，讓使用該映像建立的機器在第一次開機時就已經完全修補。

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>選項 3：套用累積更新至現有的離線 VHD 或 VHDX
如果您已有虛擬硬碟 (VHD 或 VHDX)，您可以使用 DISM 工具將更新套用到虛擬硬碟。 您需要確定該磁碟並非正在使用中，請關閉任何正在使用該磁碟的 VM 或卸載虛擬硬碟檔案。

- 使用 PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- 使用 dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>選項 4：套用累積更新至正在執行的 Nano Server
如果您擁有正在執行的 Nano Server VM 或實體主機，且您已下載作為更新的 .cab 檔案，您可以使用 DISM 工具以讓作業系統仍在線上時進行更新。 您將會需要從本機複製 .cab 檔案至 Nano Server 或至可存取的網路位置。 如果您正在套用服務堆疊更新，請確定在套用服務堆疊更新後重新啟動此伺服器，再套用其他更新。

> [!NOTE]
> 如果您已使用 NanoServerImage Cmdlet 建立 Nano Server VHD 或 VHDX 映像，但並未為虛擬硬碟檔案指定 MaxSize，預設 4GB 的大小將因為空間不足而無法套用累積更新。 在安裝更新前，請使用 Hyper-V 管理員、磁碟管理、PowerShell 或其他工具擴展虛擬硬碟和系統磁碟區的大小至最少 10GB，或使用 DISM 工具的 ScratchDir 參數將暫存目錄設至擁有至少 10GB 可用空間的磁碟區。

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- 使用 PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- 使用 dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab

   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>選項 5：下載並安裝累積更新至正在執行的 Nano Server

如果您擁有正在執行的 Nano Server VM 或實體主機，您可以使用 Windows Update WMI 提供者讓作業系統仍在線上時下載和安裝更新。 若使用此方法，您不需要從 Microsoft Update Catalog 分別下載 .msu 檔案。 WMI 提供者會同時偵測、下載和安裝所有可用的更新。

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- 掃描所有可用的更新
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- 安裝所有可用的更新
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- 取得已安裝更新的清單
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```

## <a name="additional-options"></a>其他選項
更新 Nano Server 的其他方法可能會與上述選項重疊或相輔相成。 這些選項包括使用 Windows Server Update Services (WSUS)、System Center Virtual Machine Manager (VMM)、工作排程器或非 Microsoft 的解決方案。
- 設定下列登錄機碼，[為 WSUS 設定 Windows Update](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939844(v=ws.10))：
  - WUServer
  - WUStatusServer (通常會使用和 WUServer 相同的值)
  - UseWUServer
  - AUOptions
- [在 VMM 中管理結構更新](/previous-versions/system-center/system-center-2012-R2/gg675084(v=sc.12))
- [註冊已排定的工作](/previous-versions/system-center/system-center-2012-R2/gg675084(v=sc.12))
