---
title: 使用 PowerShell Direct 管理 Windows 虛擬機器
description: 提供指示，說明如何使用 PowerShell Direct 來管理虛擬機器，而不需依賴網路或遠端連線。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: c4a051de2d8f62c38ae0c44b1a62d5bf9df339e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859431"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>使用 PowerShell Direct 管理 Windows 虛擬機器

>適用于： Windows 10、Windows Server 2016、Windows Server 2019
  
您可以使用 PowerShell Direct，從 Windows 10、Windows Server 2016 或 Windows Server 2019 Hyper-v 主機遠端系統管理 Windows 10、Windows Server 2016 或 Windows Server 2019 虛擬機器。 PowerShell Direct 可在虛擬機器內進行 Windows PowerShell 管理，而不論 Hyper-v 主機或虛擬機器上的網路設定或遠端系統管理設定為何。 這讓 Hyper-V 系統管理員更容易用指令碼自動化虛擬機器的管理和設定。  
  
有兩種方式可以執行 PowerShell Direct：  
  
- 使用 PSSession Cmdlet 建立及結束 PowerShell Direct 會話
  
- 使用 Invoke 命令 Cmdlet 執行腳本或命令
  
如果您管理的是較舊的虛擬機器，請使用虛擬機器連線 (VMConnect)，或是[設定虛擬機器的虛擬網路](https://technet.microsoft.com/library/cc816585.aspx)。  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>使用 PSSession Cmdlet 建立及結束 PowerShell Direct 會話  
  
1. 在 Hyper-V 主機上，以系統管理員身分開啟 Windows PowerShell。  
  
2. 使用[Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx) Cmdlet 來連接到虛擬機器。 執行下列其中一個命令，使用虛擬機器名稱或 GUID 來建立會話：  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. 輸入虛擬機器的認證。   
4. 執行您需要執行的命令。 這些命令會在您用來建立工作階段的虛擬機器上執行。  
  
5.  當您完成時，請使用[Exit-PSSession](https://technet.microsoft.com/library/hh849743.aspx)來關閉會話。   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>使用調用命令 Cmdlet 執行腳本或命令  
您可以使用 [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) Cmdlet，在虛擬機器上執行一組預先決定的命令。 以下是如何使用 Invoke-Command Cmdlet 的範例，其中 PSTest 是虛擬機器名稱，要執行的指令碼 (foo.ps1) 位在 C:/ 磁碟機的指令碼資料夾：  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
若要執行單一命令，請使用 **-ScriptBlock** 參數：  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>使用 PowerShell Direct 的必要條件為何？  
若要在虛擬機器上建立 PowerShell Direct 工作階段，  
  
-   虛擬機器必須在本機主機上執行和啟動。  
  
-   您必須以 Hyper-V 系統管理員身分登入主機電腦。  
  
-   您必須提供虛擬機器的有效使用者認證。  
  
-   主機作業系統必須至少執行 Windows 10 或 Windows Server 2016。
  
-   虛擬機器必須至少執行 Windows 10 或 Windows Server 2016。  
  
您可以使用「[取得 VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) 」 Cmdlet 來檢查您所使用的認證是否具有「hyper-v 系統管理員」角色，並取得在主機本機上執行並開機的虛擬機器清單。  
  
## <a name="see-also"></a>另請參閱  
[輸入-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-命令](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  


