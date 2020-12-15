---
title: 變更遠端桌面中的接聽連接埠
description: 了解如何變更遠端桌面用戶端的接聽連接埠。
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2ace6e153b50ccf366359c70c63cf73676975ec
ms.sourcegitcommit: f86366371ed566526da211daee4e5c83eb6e37b3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96843086"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>變更電腦上的遠端桌面接聽連接埠

> 適用於：Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2008 R2

當您透過遠端桌面用戶端連線至電腦 (Windows 用戶端或 Windows Server) 時，電腦上遠端桌面功能會透過定義的接聽連接埠 (預設為 3389) 「聽到」連線要求。 您可以修改登錄來變更 Windows 電腦上的接聽連接埠。

1. 啟動登錄編輯程式。 (在搜尋方塊中鍵入 regedit。)
2. 巡覽至下列登錄子機碼：**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp**
3. 尋找 **PortNumber**
4. 依序按一下 [編輯] > [修改] 和 [十進位]。
5. 鍵入新的連接埠號碼，然後按一下 [確認]。 
6. 關閉登錄編輯程式，並重新啟動您的電腦。

下次您使用遠端桌面連線連線到此電腦時，必須鍵入新的連接埠。 如果您使用防火牆，請務必設定防火牆以允許連線至新的連接埠號碼。


您可以透過執行下列 PowerShell 命令來檢查目前的連接埠：

```powershell
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber"
```

例如：

```powershell
PortNumber   : 3389
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations
PSChildName  : RDP-Tcp
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
```

您也可以透過執行下列 PowerShell 命令來變更 RDP 連接埠。 在此命令中，我們將新的 RDP 連接埠指定為 **3390**。


若要將新的 RDP 連接埠新增至登錄：

```powershell
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "PortNumber" -Value 3390
New-NetFirewallRule -DisplayName 'RDPPORTLatest' -Profile 'Public' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 3390
```
