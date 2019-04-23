---
title: 使用 Windows Server 上的 PowerShell 自訂 RDS 標題「工作資源」
description: 提供描述如何從 Windows Server 中的預設值變更工作區名稱。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826709"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>使用 Windows Server 上的 PowerShell 自訂 RDS 標題「工作資源」

使用 Windows Server 時透過 RD 存取網路或新的遠端桌面應用程式存取的 Remoteapp 或桌上型電腦，可能已經注意到工作區的預設標題為 「 工作資源 」。  您可以使用 PowerShell cmdlet，輕鬆地變更標題。

若要變更標題，請開啟新的 PowerShell 視窗，在連線代理人伺服器上並匯入 RemoteDesktop 模組使用下列命令。

```powershell
    Import-Module RemoteDesktop
```

接下來，使用組 RDWorkspace 命令來變更工作區名稱。

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

比方說，您可以使用下列命令來將工作區名稱變更為"Contoso RemoteApps 」:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

如果您執行多個連線代理人高可用性模式中，您必須針對使用中的 broker 執行這個。 您可以使用此命令：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

如需有關設定 RDWorkspace cmdlet 的詳細資訊，請參閱[組 RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps)參考。