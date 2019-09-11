---
title: 在 Windows Server 上使用磁片清理
description: 瞭解如何使用命令列選項來設定磁片清理工具（Cleanmgr.exe 釋放），以自動清除特定檔案。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 4bf32520dc6fa2be36d44fbd66a7efc885a8f5d7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867407"
---
# <a name="using-disk-cleanup-on-windows-server"></a>在 Windows Server 上使用磁片清理

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

[磁片清理] 工具會在 Windows Server 環境中清除不必要的檔案。 Windows Server 2019 和 Windows Server 2016 上預設會提供這項工具，但您可能必須採取幾個手動步驟，才能在舊版的 Windows Server 上啟用它。

若要啟動 [磁片清理] 工具，請執行 Cleanmgr.exe 釋放命令，或選取 [**開始**]，選取 [ **Windows 系統管理工具**]，然後選取 [**磁片清理**]。

您也可以使用[Cleanmgr.exe 釋放 Windows 命令](../../administration/windows-commands/cleanmgr.md)來執行磁片清理，並使用命令列選項來指定磁片清理清除特定檔案。

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>藉由安裝桌面體驗，在舊版 Windows Server 上啟用磁片清理

請遵循下列步驟，使用 [新增角色及功能] Wizard 在執行 Windows Server 2012 R2 或更早版本的伺服器上安裝桌面體驗，這也會安裝磁片清理。

1. 如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。

   - 在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員] 來啟動 [伺服器管理員]。

   - 移至 [**開始**]，然後選取 [伺服器管理員] 磚。

1. 在 [**管理**] 功能表上，選取 [新增**角色及功能**]。

1. 在 [**開始之前**] 頁面上，確認您的目的地伺服器和網路環境已準備好要安裝的功能。 選取 [下一步]。

1. 在 [**選取安裝類型**] 頁面上，選取 [**角色型或功能型安裝**]，以在單一伺服器上安裝所有元件功能。 選取 [下一步]。

1. 在 [選取目的地伺服器] 頁面上，從伺服器集區選取一部伺服器，或者選取一個離線 VHD。 選取 [下一步]。

1. 在 [**選取伺服器角色**] 頁面上，選取 **[下一步]** 。

1. 在 [**選取功能**] 頁面上，選取 [**使用者介面和基礎結構**]，然後選取 [**桌面體驗**]。

1. 在 [**新增桌面體驗所需的功能？** ] 中，選取 [**新增功能**]。

1. 繼續進行安裝，然後重新開機系統。

1. 確認 [**磁片清理**選項] 按鈕出現在 [內容] 對話方塊中。

   ![具有磁片清理選項的磁片屬性對話方塊](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>手動將磁片清理新增至舊版的 Windows Server

除非您已安裝桌面體驗功能，否則 Windows Server 2012 R2 或更早版本上不會顯示「磁片清理工具」（cleanmgr.exe 釋放 .exe）。

若要使用 cleanmgr.exe 釋放，請安裝先前所述的桌面體驗，或複製伺服器上已存在的兩個檔案 cleanmgr.exe 釋放和 cleanmgr.exe 釋放。 使用下表來找出作業系統的檔案。

| 作業系統  | 架構  | 檔案位置  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

找出 cleanmgr.exe 釋放，並將檔案移至 **%systemroot%\System32**。

找出 cleanmgr.exe 釋放，並將檔案移至 **%systemroot%\System32\en-US**。

您現在可以從命令提示字元執行 Cleanmgr.exe 釋放，或按一下 [**開始**]，然後在搜尋列中輸入**cleanmgr.exe 釋放**，以啟動 [磁片清理] 工具。

若要讓 [磁片清理] 按鈕顯示在磁片的 [內容] 對話方塊上，您也必須安裝 [桌面體驗] 功能。

## <a name="additional-references"></a>其他參考資料

[釋放 Windows 10 中的磁片磁碟機空間](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
