---
title: Nano Server 的開發作業
description: PowerShell 遠端處理及 CIM 工作階段
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8d793dde9c41bc99b55eeb0da3a5ee4b025f08d6
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443647"
---
# <a name="developing-for-nano-server"></a>Nano Server 的開發作業

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

這些主題說明 Nano Server 上之 PowerShell 的重要差異，並同時提供相關指引，讓您開發自己的 PowerShell Cmdlet 以用於 Nano Server。

- [Nano 伺服器上的 PowerShell](PowerShell-on-Nano-Server.md)
- [開發 Nano 伺服器的 PowerShell Cmdlet](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>使用 Windows PowerShell 遠端執行功能  
若要使用 Windows PowerShell 遠端執行功能管理 Nano Server，您需要將 Nano Server 的 IP 位址新增至管理電腦的信任主機清單、將您使用的帳戶新增為 Nano Server 的系統管理員，並啟用 CredSSP (如果想要使用該功能)。  

> [!NOTE]
> 如果目標 Nano Server 和您的管理電腦位於相同的 AD DS 樹系 (或具有信任關係的樹系) 中，則不應該將 Nano Server 新增至信任主機清單；您可以使用 Nano Server 的完整網域名稱連線到 Nano Server，例如︰PS C:\>Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
若要將 Nano Server 新增至信任主機清單，請在提升權限的 Windows PowerShell 命令提示字元中執行下列命令：  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
若要啟動遠端 Windows PowerShell 工作階段，請啟動提升權限的本機 Windows PowerShell 工作階段，然後執行下列命令：  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
您現在可在 Nano Server 上正常執行 Windows PowerShell 命令。  
  
> [!NOTE]  
> 並非所有 Windows PowerShell 命令都可用於此版本的 Nano Server。 若要查看有哪些命令可用，請執行 `Get-Command -CommandType Cmdlet`  
  
透過命令 `Exit-PSSession` 停止遠端工作階段  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>透過 WinRM 使用 Windows PowerShell CIM 工作階段  
您可以在 Windows PowerShell 中使用 CIM 工作階段和執行個體，透過 Windows 遠端管理 (WinRM) 執行 WMI 命令。  
  
在 Windows PowerShell 命令提示字元中執行下列命令來啟動 CIM 工作階段：  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
建立工作階段之後，您可以執行各種 WMI 命令，例如：  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  