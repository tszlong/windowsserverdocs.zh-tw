---
title: 針對受防護的 Vm 進行疑難排解
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: b0d4338d20238eb528c19221151f380cc154a2db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386371"
---
# <a name="troubleshoot-shielded-vms"></a>針對受防護的 Vm 進行疑難排解

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

從 Windows Server 1803 版開始，已針對完全受防護的 Vm 重新啟用虛擬機器連線（VMConnect）增強的會話模式和 PS Direct。 虛擬化系統管理員仍然需要 VM 來賓認證來取得 VM 的存取權，但這讓主機服務提供者可以在網路設定中斷時，更輕鬆地對受防護的 VM 進行疑難排解。

若要啟用受防護 Vm 的 VMConnect 和 PS Direct，只需要將它們移至執行 Windows Server 1803 版或更新版本的 Hyper-v 主機。 允許這些功能的虛擬裝置會自動重新啟用。 如果受防護的 VM 移至執行和舊版 Windows Server 的主機，則會再次停用 VMConnect 和 PS Direct。

對於需要擔心主控者是否可存取 VM 並想要回到原始行為的安全性敏感客戶，應在虛擬作業系統中停用下列功能：

- 停用 VM 中的 PowerShell Direct 服務：

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- 只有當您的虛擬作業系統至少為 Windows Server 2019 或 Windows 10 版本1809時，才可以停用 VMConnect 增強的會話模式。 在您的 VM 中新增下列登錄機碼，以停用 VMConnect 增強的會話主控台連線：

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
