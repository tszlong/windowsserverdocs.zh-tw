---
title: 受防護的 Vm 進行疑難排解
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850029"
---
# <a name="troubleshoot-shielded-vms"></a>受防護的 Vm 進行疑難排解

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

從 Windows Server 版本 1803年，虛擬機器連線 (VMConnect) 增強的工作階段模式和 PS 直接會重新啟用完整的受防護的 vm。 虛擬化系統管理員仍然需要 VM 客體的認證來存取 VM，但這可以讓主機服務提供者中斷其網路設定時，針對受防護的 VM 進行疑難排解更容易。

若要啟用 VMConnect 和 PS 直接受防護的 Vm，只要將它們移至執行 Windows Server 版本 1803年或更新版本的 HYPER-V 主機。 以這些功能的虛擬裝置將會自動重新啟用。 如果受防護的 VM 移到執行主機和舊版的 Windows Server，VMConnect 和 PS 直接將會停用一次。

對於機密客戶擔心如果主機服務提供者有任何 VM 的存取權，而且想要返回原始行為，應該客體 OS 中停用下列功能：

- 停用 PowerShell Direct 服務在 VM 中：

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- VMConnect 增強的工作階段模式可以只停用將客體作業系統至少為 Windows Server 2019 或 Windows 10 版本 1809年。 若要停用 VMConnect 增強的工作階段主控台連線在 VM 中新增下列登錄機碼：

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
