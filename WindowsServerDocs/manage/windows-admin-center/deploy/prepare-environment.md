---
title: 準備您的 Windows Admin Center 環境
description: 準備您的 Windows Admin Center 環境 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 96aced2c062717aee0d2957b751bc2c25ac8e0da
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869103"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>準備您的 Windows Admin Center 環境

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

有一些 Server 版本需要先做額外準備，才可開始使用 Windows Admin Center 進行管理：

- [Windows Server 2012 和 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-v Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

在某些情況下，可能需要先修改[目標伺服器上的通訊埠設定](#port-configuration-on-the-target-server)，才能使用 Windows 系統管理中心進行管理。

## <a name="prepare-windows-server-2012-and-2012-r2"></a>準備 Windows Server 2012 和 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>安裝 WMF 5.1 版或更新版本

Windows Admin Center 需要預設未包含在 Windows Server 2012 和 2012 R2 中的 PowerShell 功能。 若要使用 Windows Admin Center 管理 Windows Server 2012 或 2012 R2，必須在這些伺服器上安裝 WMF 5.1 版或更新版本。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure)。

## <a name="prepare-windows-server-2008-r2"></a>準備 Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>安裝 WMF 5.1 版或更新版本

Windows Admin Center 需要預設未包含在 Windows Server 2008 R2 中的 PowerShell 功能。 若要使用 Windows Admin Center 管理 Windows Server 2008 R2，必須在這些伺服器上安裝 WMF 5.1 版或更新版本。 

確定電腦上已安裝[.NET Framework 4.5.2 或更新版本](https://docs.microsoft.com/dotnet/framework/install/on-windows-7)。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure)。

在 Powershell 主控台中執行 `Enable-PSRemoting –force`，以啟用 PowerShell 遠端連線。 

### <a name="enable-remote-desktop"></a>啟用遠端桌面

若要在 Windows Admin Center 中使用遠端桌面，您必須在 Windows Server 2008 R2 伺服器上啟用遠端桌面。

從 **\[伺服器管理員\]** 移至 **\[設定遠端桌面\]** 。 啟用遠端桌面的 [允許來自執行任何版本之遠端桌面的電腦進行連線] 選項。

## <a name="prepare-microsoft-hyper-v-server-2016"></a>準備 Microsoft Hyper-V Server 2016

若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016，有一些伺服器角色必須先啟用，您才可以開始使用。

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016：

1. 啟用遠端管理。
2. 啟用檔案伺服器角色。
3. 啟用 PowerShell 的 Hyper-V 模組。

### <a name="step-1-enable-remote-management"></a>**步驟 1:** 啟用遠端管理

若要在 Hyper-V Server 中啟用遠端管理：

1. 登入 Hyper-V Server。
2. 在 **\[伺服器設定\]** (SCONFIG) 工具中，鍵入 **4** 設定遠端管理。
3. 鍵入 **1** 啟用 \[遠端管理\]。
4. 鍵入 **4** 回到主功能表。

### <a name="step-2-enable-file-server-role"></a>**步驟 2:** 啟用檔案伺服器角色

若要啟用基本檔案共用及遠端管理的檔案伺服器角色：

1. 按一下 **\[工具\]** 功能表中的 **\[角色和功能\]** 。
2. 在 **\[角色和功能\]** 中尋找 **\[檔案和存放服務\]** ，並核取 **\[檔案和 iSCSI 服務\]** 和 **\[檔案伺服器\]** ：

![顯示已選取檔案和 iSCSI 服務角色之角色和功能的螢幕擷取畫面](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**步驟 3：** 啟用 PowerShell 的 Hyper-V 模組

若要啟用 PowerShell 的 Hyper-V 模組功能：

1. 按一下 **\[工具\]** 功能表中的 **\[角色和功能\]** 。
2. 在 **\[角色和功能\]** 中尋找 **\[遠端伺服器管理工具\]** ，並核取 **\[角色管理工具\]** 和 **\[PowerShell 的 Hyper-V 模組\]** ：

![顯示已選取 Hyper-v 角色之角色和功能的螢幕擷取畫面](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 現已準備好可以使用 Windows Admin Center 進行管理。

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>準備 Microsoft Hyper-V Server 2012 R2

若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，有一些伺服器角色必須先啟用，您才可以開始使用。  此外，您還需要安裝 WMF 5.1 版或更新版本。

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2：

1. 安裝 Windows Management Framework (WMF) 5.1 版或更新版本
2. 啟用遠端管理
3. 啟用檔案伺服器角色
4. 啟用 PowerShell 的 Hyper-V 模組

### <a name="step-1-install-windows-management-framework-51"></a>步驟 1：安裝 Windows Management Framework 5。1

Windows Admin Center 需要預設未包含在 Microsoft Hyper-V Server 2012 R2 中的 PowerShell 功能。 若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，您必須安裝 WMF 5.1 或更新版本。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。 

如果尚未安裝，您可以[下載 WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure)。

### <a name="step-2-enable-remote-management"></a>步驟 2：啟用遠端管理

若要啟用 Hyper-V Server 遠端管理：

1. 登入 Hyper-V Server。
2. 在 **\[伺服器設定\]** (SCONFIG) 工具中，鍵入 **4** 設定遠端管理。
3. 鍵入 **1** 啟用遠端管理。
4. 鍵入 **4** 回到主功能表。

### <a name="step-3-enable-file-server-role"></a>步驟 3：啟用檔案伺服器角色

若要啟用基本檔案共用及遠端管理的檔案伺服器角色：

1. 按一下 **\[工具\]** 功能表中的 **\[角色和功能\]** 。
2. 在 **\[角色和功能\]** 中尋找 **\[檔案和存放服務\]** ，並核取 **\[檔案和 iSCSI 服務\]** 和 **\[檔案伺服器\]** ：

![顯示已選取檔案和 iSCSI 服務角色之角色和功能的螢幕擷取畫面](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>步驟 4：啟用 PowerShell 的 Hyper-V 模組

若要啟用 PowerShell 的 Hyper-V 模組功能：

1. 按一下 **\[工具\]** 功能表中的 **\[角色和功能\]** 。
2. 在 **\[角色和功能\]** 中尋找 **\[遠端伺服器管理工具\]** ，並核取 **\[角色管理工具\]** 和 **\[PowerShell 的 Hyper-V 模組\]** ：

![顯示已選取的 Hyper-v 遠端伺服器管理工具之角色和功能的螢幕擷取畫面](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 現已準備好可以使用 Windows Admin Center 進行管理。

## <a name="port-configuration-on-the-target-server"></a>目標伺服器上的埠設定

Windows 管理中心會針對某些檔案複製工作（例如在遠端伺服器上匯入憑證時）使用 SMB 檔案共用通訊協定。 若要讓這些檔案複製作業成功，遠端伺服器上的防火牆必須允許埠445上的輸入連線。  您可以使用 Windows 系統管理中心的防火牆工具，確認 [檔案伺服器遠端系統管理（SMB 輸入）] 的傳入規則已設定為允許此埠上的存取。

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)