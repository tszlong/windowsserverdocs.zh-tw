---
title: 安裝 Server Core
description: 如何在 Windows Server 2019、Windows Server 2016 或 Windows Server (半年通道) 上取得和安裝 Server Core 安裝。
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e6264a59a837003e49e82529750cfb153cc37b92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360345"
---
# <a name="install-server-core"></a>安裝 Server Core

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)
  
第一次安裝 Windows Server 時，您會有下列安裝選項：

>[!NOTE]
> 在下列清單中，沒有「桌面體驗」的版本是 Server Core 安裝選項

-   Windows Server Standard
-   Windows Server Standard 含桌面體驗
-   Windows Server Datacenter
-   Windows Server Datacenter 含桌面體驗

安裝 Windows Server (半年通道) 時，您會有下列安裝選項：

-   Windows Server Standard 
-   Windows Server Datacenter

[Server Core] 選項可以減少所需磁碟空間和潛在的受攻擊面；因此，除非您對 [含有桌面體驗的伺服器] 選項所包含的額外使用者介面項目與圖形化管理工具有特定需求，否則建議您選擇 Server Core 安裝。 如果您確實認為需要額外的使用者介面項目，請參閱[安裝含有桌面體驗的伺服器](Getting-Started-with-Server-with-Desktop-Experience.md)。 

[Server Core] 選項不會安裝標準使用者介面 (桌面體驗)；您可以使用命令列、Windows PowerShell 或透過遠端方法來管理伺服器。

>[!NOTE]
>
>與 Windows Server 某些更早的版本不同，您無法在安裝完成後在 Server Core 與桌面體驗伺服器之間進行轉換。 如果您安裝 Server Core，並稍後決定使用桌面體驗伺服器，您應該執行全新安裝。

**使用者介面︰** 命令提示字元

**在本機安裝、設定、解除安裝伺服器角色：** 在命令提示字元使用 Windows PowerShell。

**從遠端 Windows 用戶端電腦 (或已安裝含有桌面體驗的伺服器) 安裝、設定、解除安裝伺服器角色：** 使用伺服器管理員、遠端伺服器管理工具 (RSAT)、Windows PowerShell 或 Windows Admin Center。

>[!NOTE]
>
>對於 RSAT，您必須使用 Windows 10 版本。
>無法在本機使用 Microsoft Management Console。

**可用的範例伺服器角色：**

- Active Directory 憑證服務
- Active Directory Domain Services
- DHCP 伺服器
- DNS 伺服器
- 檔案服務 (包括檔案伺服器資源管理員)
- Active Directory 輕量型目錄服務 (AD LDS)
- Hyper-V
- 列印和文件服務
- 串流處理媒體服務
- 網頁伺服器 (包括 ASP.NET 子集)
- Windows Server 更新伺服器
- Active Directory Rights Management Server
- 路由及遠端存取伺服器和下列子角色：
   - 遠端桌面服務連線代理人
   - 授權
   - 虛擬化
   - 大量啟用服務

如需 Server Core 中未包含的角色，請參閱[不在 Windows Server - Server Core 中的角色、角色服務和功能](../administration/server-core/server-core-removed-roles.md)。

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>安裝於 Windows Server 2019 或 Windows Server 2016

如需 Windows Server (長期維護通道) 的一般安裝步驟和選項，請參閱 [Windows Server 安裝與升級](installation-and-upgrade.md)。

## <a name="installing-on-windows-server-semi-annual-channel"></a>安裝於 Windows Server (半年通道)

Windows Server (半年通道) 的安裝步驟和安裝舊版 Windows Server 一樣 (從 .ISO 映像)，但有下列例外：

- 不支援從舊版的 Windows Server 升級到 Windows Server 1709 版。 一律需要全新安裝。
   這表示當您從 Windows 電腦桌面執行 setup.exe 時，安裝體驗不允許升級選項 (呈現灰色)。
- Windows Server (半年通道) 沒有評估版
- 沒有 OEM 或零售版。 Windows Server (半年通道) 只能透過「軟體保證」或忠誠度方案取得授權。

如需半年通道的詳細資訊，請參閱[維護通道比較](../get-started-19/servicing-channels-19.md)。

若要了解 Windows Server 半年通道的新功能，請參閱 [Windows Server 的新功能](whats-new-in-windows-server.md)
