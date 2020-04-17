---
title: 準備您的 Windows Admin Center 環境
description: 準備您的 Windows Admin Center 環境 (專案 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7a4dacd611741942e874e831fd9598aeda5e97b3
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269275"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>準備您的 Windows Admin Center 環境

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

有一些 Server 版本需要先做額外準備，才可開始使用 Windows Admin Center 進行管理：

- [Windows Server 2012 和 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

在某些情況下，可能需要先修改[目標伺服器上的連接埠組態](#port-configuration-on-the-target-server)，才能使用 Windows Admin Center 進行管理。

## <a name="prepare-windows-server-2012-and-2012-r2"></a>準備 Windows Server 2012 和 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>安裝 WMF 5.1 版或更新版本

Windows Admin Center 需要使用預設未包含在 Windows Server 2012 和 2012 R2 中的 PowerShell 功能。 若要使用 Windows Admin Center 管理 Windows Server 2012 或 2012 R2，必須在這些伺服器上安裝 WMF 5.1 版或更新版本。

請在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF，且版本是 5.1 或更新版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure)。

## <a name="prepare-microsoft-hyper-v-server-2016"></a>準備 Microsoft Hyper-V Server 2016

若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016，必須先啟用一些伺服器角色，您才可以開始使用。

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016：

1. 啟用遠端管理。
2. 啟用檔案伺服器角色。
3. 啟用 PowerShell 的 Hyper-V 模組。

### <a name="step-1-enable-remote-management"></a>**步驟 1：** 啟用遠端管理

要在 Hyper-V 伺服器中啟用遠端管理：

1. 登入 Hyper-V 伺服器。
2. 在 \[伺服器組態\]  (SCONFIG) 工具中，鍵入 **4** 設定遠端管理。
3. 鍵入 **1** 啟用遠端管理。
4. 鍵入 **4** 回到主功能表。

### <a name="step-2-enable-file-server-role"></a>**步驟 2：** 啟用檔案伺服器角色

若要啟用基本檔案共用及遠端管理的檔案伺服器角色：

1. 按一下 [工具]  功能表中的 [角色和功能]  。
2. 在 [角色和功能]  中尋找 [檔案和存放服務]  ，並核取 [檔案和 iSCSI 服務]  和 [檔案伺服器]  ：

![顯示已選取之檔案和 iSCSI 服務角色的角色和功能螢幕擷取畫面](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**步驟 3：** 啟用適用於 PowerShell 的 Hyper-V 模組

要啟用適用於 PowerShell 的 Hyper-V 模組功能：

1. 按一下 [工具]  功能表中的 [角色和功能]  。
2. 在 [角色和功能]  中尋找 [遠端伺服器管理工具]  ，並核取 [角色管理工具]  和 [適用於 PowerShell 的 Hyper-V 模組]  ：

![顯示已選取之 Hyper-V 角色的角色和功能螢幕擷取畫面](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 現已準備好可以使用 Windows Admin Center 進行管理。

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>準備 Microsoft Hyper-V Server 2012 R2

若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，必須先啟用一些伺服器角色，您才可以開始使用。  此外，您還需要安裝 WMF 5.1 版或更新版本。

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2：

1. 安裝 Windows Management Framework (WMF) 5.1 版或更新版本
2. 啟用遠端管理
3. 啟用檔案伺服器角色
4. 啟用適用於 PowerShell 的 Hyper-V 模組

### <a name="step-1-install-windows-management-framework-51"></a>步驟 1：安裝 Windows Management Framework 5.1

Windows Admin Center 需要使用預設未包含在 Microsoft Hyper-V Server 2012 R2 中的 PowerShell 功能。 若要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，您必須安裝 WMF 5.1 或更新版本。

請在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF，且版本是 5.1 或更新版本。 

如果尚未安裝，您可以[下載 WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure)。

### <a name="step-2-enable-remote-management"></a>步驟 2：啟用遠端管理

若要啟用 Hyper-V 伺服器遠端管理：

1. 登入 Hyper-V 伺服器。
2. 在 \[伺服器組態\]  (SCONFIG) 工具中，鍵入 **4** 設定遠端管理。
3. 鍵入 **1** 啟用遠端管理。
4. 鍵入 **4** 回到主功能表。

### <a name="step-3-enable-file-server-role"></a>步驟 3：啟用檔案伺服器角色

若要啟用基本檔案共用及遠端管理的檔案伺服器角色：

1. 按一下 [工具]  功能表中的 [角色和功能]  。
2. 在 [角色和功能]  中尋找 [檔案和存放服務]  ，並核取 [檔案和 iSCSI 服務]  和 [檔案伺服器]  ：

![顯示已選取之檔案和 iSCSI 服務角色的角色和功能螢幕擷取畫面](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>步驟 4：啟用適用於 PowerShell 的 Hyper-V 模組

要啟用適用於 PowerShell 的 Hyper-V 模組功能：

1. 按一下 [工具]  功能表中的 [角色和功能]  。
2. 在 [角色和功能]  中尋找 [遠端伺服器管理工具]  ，並核取 [角色管理工具]  和 [適用於 PowerShell 的 Hyper-V 模組]  ：

![顯示已選取之 Hyper-V 遠端伺服器管理工具的角色和功能螢幕擷取畫面](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 現已準備好可以使用 Windows Admin Center 進行管理。

## <a name="port-configuration-on-the-target-server"></a>目標伺服器上的連接埠組態

Windows Admin Center 會針對某些檔案複製工作 (例如在遠端伺服器上匯入憑證) 使用 SMB 檔案共用通訊協定。 若要讓這些檔案複製作業成功，遠端伺服器上的防火牆必須允許連接埠 445 的傳入連線。  您可以使用 Windows Admin Center 的防火牆工具，確認「檔案伺服器遠端系統管理 (SMB-In)」的傳入規則已設定為允許此連接埠上的存取。

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)
