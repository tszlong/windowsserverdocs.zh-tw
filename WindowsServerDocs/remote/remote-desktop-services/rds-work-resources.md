---
title: 在 Windows Server 上使用 PowerShell 自訂 RDS 標題「工作資源」
description: 提供關於如何在 Windows Server 中變更預設工作區名稱的說明。
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 5124ce691793570f6ffa11a43975719addb89e67
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970115"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>在 Windows Server 上使用 PowerShell 自訂 RDS 標題「工作資源」

使用 Windows Server 透過 RD WebAccess 或新的遠端桌面應用程式存取 Remoteapp 或桌面時，您可能會發現工作區的預設標題為「工作資源」。  您可以使用 PowerShell Cmdlet 輕鬆變更標題。

若要變更標題，請在連線代理人伺服器上開啟新的 PowerShell 視窗，並使用下列命令匯入 RemoteDesktop 模組。

```powershell
    Import-Module RemoteDesktop
```

接著，請使用 Set-RDWorkspace 命令變更工作區名稱。

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```

例如，您可以使用下列命令將工作區名稱變更為 "Contoso RemoteApps"：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

如果您在高可用性模式中執行多個連線代理人，您必須對主動代理人執行此作業。 您可以使用下列命令：

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

如需 Set-RDWorkspace Cmdlet 的詳細資訊，請參閱 [Set-RDSWorkspace](/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) 參考。
