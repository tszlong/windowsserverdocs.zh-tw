---
description: 深入瞭解：針對受防護的 Vm 進行疑難排解
title: 針對受防護的 Vm 進行疑難排解
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 10/3/2018
ms.openlocfilehash: 3f4c0eb81169f9b66170d3332d220233d476ba5f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049206"
---
# <a name="troubleshoot-shielded-vms"></a>針對受防護的 Vm 進行疑難排解

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

從 Windows Server 1803 版開始，虛擬機器連線的 (VMConnect) 增強的會話模式和 PS Direct 會針對完全受防護的 Vm 重新啟用。 虛擬化系統管理員仍然需要 VM 來賓認證才能存取 VM，但這讓主機服務提供者在網路設定中斷時，可以更輕鬆地針對受防護的 VM 進行疑難排解。

若要為受防護的 Vm 啟用 VMConnect 和 PS Direct，只要將其移至執行 Windows Server 1803 版或更新版本的 Hyper-v 主機。 將會自動重新啟用允許這些功能的虛擬裝置。 如果受防護的 VM 移至執行和舊版 Windows Server 的主機，VMConnect 和 PS Direct 將會再次停用。

若為需要安全性的客戶，請擔心主控者是否有 VM 的存取權，而且想要回到原始行為，請在客體作業系統中停用下列功能：

- 停用 VM 中的 PowerShell Direct 服務：

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- 只有當您的虛擬作業系統至少為 Windows Server 2019 或 Windows 10 版本1809時，才能停用 VMConnect 增強的會話模式。 在您的 VM 中新增下列登錄機碼，以停用 VMConnect 增強的會話主控台連接：

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
