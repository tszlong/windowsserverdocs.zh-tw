---
title: 安裝 Server Core
description: 如何取得並安裝 Server Core 安裝在 Windows Server 2019、 Windows Server 2016 或 Windows Server （半年度管道）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 7fc7271745e40f110c54918b55624cadd0d7ff98
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991795"
---
# 安裝 Server Core

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)
  
當您第一次安裝 Windows Server 時，您會有下列安裝選項：

>[!NOTE]
> 在下列清單中，沒有「桌面體驗」的版本是 Server Core 安裝選項

-   Windows Server Standard
-   Windows Server Standard 含桌面體驗
-   Windows Server Datacenter
-   Windows Server Datacenter 含桌面體驗

當您安裝 Windows Server （半年度管道）、 包括版本 1709年、 1803、 及 1809，您會有下列安裝選項：

-   Windows Server Standard 
-   Windows Server Datacenter

[Server Core] 選項可以減少所需的磁碟空間和潛在的攻擊面。因此，除非您對 [含有桌面體驗的伺服器] 選項所包含的額外使用者介面元素與圖形化管理工具有特定需求，否則建議您選擇 Server Core 安裝。 如果您確實認為需要額外的使用者介面元素，請參閱[安裝「含有桌面體驗的伺服器」](Getting-Started-with-Server-with-Desktop-Experience.md)。 

[Server Core] 選項不會安裝標準使用者介面 (桌面體驗)；您可以使用命令列、Windows PowerShell 或透過遠端方法來管理伺服器。

>[!NOTE]
>
>與 WindowsServer 某些更早的版本不同，您無法在安裝完成後在 Server Core 與桌面體驗伺服器之間進行轉換。 如果您安裝 Server Core，並稍後決定使用桌面體驗伺服器，您應該執行全新安裝。

**使用者介面︰** 命令提示字元

**在本機安裝、設定、解除安裝伺服器角色：** 在命令提示字元使用 Windows PowerShell。

**安裝、 設定、 解除安裝伺服器角色從遠端 Windows 用戶端電腦 （或安裝含有桌面體驗的伺服器）：** 使用伺服器管理員、 遠端伺服器管理工具 (RSAT)、 Windows PowerShell 或 Windows Admin Center。

>[!NOTE]
>
>對於 RSAT，您必須使用 Windows 10 版本。
>Microsoft Management Console 在本機無法使用。

**範例中可用的伺服器角色：**

- Active Directory 憑證服務
- Active Directory 網域服務
- DHCP 伺服器
- DNS 伺服器
- 檔案服務 (包括檔案伺服器資源管理員)
- Active Directory 輕量型目錄服務 (AD LDS)
- Hyper-V
- 列印和文件服務
- 串流處理媒體服務
- 網頁伺服器 (包括 ASP.NET 子集)
- WindowsServer 更新伺服器
- Active Directory Rights Management Server
- 路由及遠端存取伺服器和下列子角色：
- 遠端桌面服務連線代理人
- 授權
- 虛擬化
- 大量啟用服務

不包含在 Server Core 中的角色，請參閱[角色、 角色服務和功能不在 Windows Server 的 Server Core](../administration/server-core/server-core-removed-roles.md)。

## 在 Windows Server 2019 或 Windows Server 2016 上安裝

如需一般安裝步驟和選項適用於 Windows Server （長期維護通道），請參閱[Windows Server 安裝與升級](installation-and-upgrade.md)。

## Windows Server （半年度管道） 上安裝

適用於 Windows Server （半年度管道） 的安裝步驟是和安裝舊版的 Windows Server 一樣 (從。ISO 映像），有下列例外：
- 不支援從先前的 Windows Server to Windows Server 版本 1709 升級。 永遠需要全新安裝。
   這表示，當您從 Windows 電腦桌面執行 setup.exe，安裝體驗不允許升級選項 （呈現灰色）。
- 適用於 Windows Server （半年度管道） 沒有評估版本
- 沒有 OEM 或零售版。 Windows Server （半年度管道） 只能透過 「 軟體保證 」 或忠誠計畫取得授權。

若要取得 Windows Server 版本 1709，請參閱[Windows Server 版本 1709 簡介](get-started-with-1709.md)。

若要取得 Windows Server 版本 1803年， [，](get-started-with-1803.md)請參閱介紹 Windows Server 版本 1803年。

什麼是新的 Windows Server 中，版本 1809，請參閱[的 Windows Server 版本 1809年中的新功能](whats-new-in-windows-server-1809.md)
