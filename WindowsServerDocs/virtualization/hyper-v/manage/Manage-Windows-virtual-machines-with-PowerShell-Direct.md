---
title: 使用 PowerShell Direct 管理 Windows 虛擬機器
description: 說明如何使用 PowerShell Direct 管理虛擬機器，完全不用仰賴網路或遠端連接。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 4081a9737825d2f50f0d3b19b3bada3b9bbc76f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814699"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>使用 PowerShell Direct 管理 Windows 虛擬機器

>適用於：Windows 10、windows Server 2016 中，Windows Server 2019
  
您可以使用 PowerShell Direct，從 Windows 10，Windows Server 2016 或 Windows Server 2019 HYPER-V 主機遠端管理 Windows 10、windows Server 2016 或 Windows Server 2019 虛擬機器。 PowerShell Direct 可讓用不論網路設定或遠端管理設定，請在 HYPER-V 主機上的或虛擬機器內的 Windows PowerShell 管理。 這讓 Hyper-V 系統管理員更容易用指令碼自動化虛擬機器的管理和設定。  
  
有兩種方式可以執行 PowerShell Direct：  
  
- 建立及結束 PowerShell Direct 工作階段，使用 PSSession cmdlet
  
- 使用 Invoke-command cmdlet 執行指令碼或命令
  
如果您管理的是較舊的虛擬機器，請使用虛擬機器連線 (VMConnect)，或是[設定虛擬機器的虛擬網路](https://technet.microsoft.com/library/cc816585.aspx)。  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>建立及結束 PowerShell Direct 工作階段，使用 PSSession cmdlet  
  
1. 在 Hyper-V 主機上，以系統管理員身分開啟 Windows PowerShell。  
  
2. 使用[Enter-pssession](https://technet.microsoft.com/library/hh849707.aspx) cmdlet 來連線到虛擬機器。 執行下列命令來建立工作階段使用的虛擬機器名稱或 GUID 的其中一個：  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. 輸入您的虛擬機器的認證。   
4. 執行您需要執行的命令。 這些命令會在您用來建立工作階段的虛擬機器上執行。  
  
5.  當您完成時，使用[Exit-pssession](https://technet.microsoft.com/library/hh849743.aspx)關閉工作階段。   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>使用 Invoke-command cmdlet 執行指令碼或命令  
您可以使用 [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) Cmdlet，在虛擬機器上執行一組預先決定的命令。 以下是如何使用 Invoke-Command Cmdlet 的範例，其中 PSTest 是虛擬機器名稱，要執行的指令碼 (foo.ps1) 位在 C:/ 磁碟機的指令碼資料夾：  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
若要執行單一命令，請使用 **-ScriptBlock** 參數：  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>若要使用 PowerShell Direct 必要條件？  
若要在虛擬機器上建立 PowerShell Direct 工作階段，  
  
-   虛擬機器必須在本機主機上執行和啟動。  
  
-   您必須以 Hyper-V 系統管理員身分登入主機電腦。  
  
-   您必須提供虛擬機器的有效使用者認證。  
  
-   主機作業系統必須至少執行 Windows 10 或 Windows Server 2016。
  
-   虛擬機器必須至少執行 Windows 10 或 Windows Server 2016。  
  
您可以使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet 來檢查您正在使用的認證是否具有 HYPER-V 系統管理員角色，並取得一份虛擬機器主機上本機執行及啟動。  
  
## <a name="see-also"></a>另請參閱  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  


