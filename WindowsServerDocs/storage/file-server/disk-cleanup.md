---
title: Windows Server 上使用磁碟清理
description: 了解如何使用命令列選項來設定 「 磁碟清理 」 工具 (Cleanmgr.exe) 可自動清除某些檔案。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fbec7cd2b8312f03998cfb27b739d0866d3a47c5
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407661"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Windows Server 上使用磁碟清理

> 適用於：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

磁碟清理工具清除不必要的檔案，在 Windows Server 環境中。 此工具是以 Windows Server 2019 和 Windows Server 2016 中，預設可以使用，但您可能必須採取幾個手動步驟，舊版的 Windows Server 上啟用此功能。

若要啟動 磁碟清理工具，請執行 Cleanmgr.exe 命令，或選取**開始**，選取**Windows 系統管理工具**，然後選取**磁碟清理**。

您也可以使用 執行磁碟清理[[cleanmgr] Windows 命令](../../administration/windows-commands/cleanmgr.md)然後使用命令列選項來指定磁碟清理會清除特定檔案。

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>在舊版的 Windows Server 上啟用清理磁碟，藉由安裝桌面體驗

請遵循以下步驟，使用 [新增角色及功能精靈] 來執行 Windows Server 2012 R2 的伺服器上安裝桌面體驗或更早版本，這也會安裝磁碟清理。

1. 如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。

   - 在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員]  來啟動 [伺服器管理員]。

   - 移至**啟動**，然後選取 [伺服器管理員] 磚。

1. 在 **管理**功能表上，選取 新增**角色及功能**。

1. 在 **在您開始之前**頁面上，確認您想要安裝的功能已準備好目的地伺服器和網路環境。 選取 **\[下一步\]** 。

1. 在 **選取安裝類型**頁面上，選取**角色型或功能型安裝**在單一伺服器上安裝所有的部分功能。 選取 **\[下一步\]** 。

1. 在 [選取目的地伺服器]  頁面上，從伺服器集區選取一部伺服器，或者選取一個離線 VHD。 選取 **\[下一步\]** 。

1. 在 [**選取伺服器角色**頁面上，選取**下一步]** 。

1. 在 **選取的功能**頁面上，選取**使用者介面和基礎結構**，然後選取**桌面體驗**。

1. 在 **新增所需的桌面體驗的功能嗎？** ，選取**新增功能**。

1. 繼續進行安裝，並再重新啟動系統。

1. 確認**磁碟清理**選項按鈕會出現在 [屬性] 對話方塊中。

   ![磁碟使用磁碟清理選項的 [屬性] 對話方塊](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>手動新增至舊版的 Windows Server 的 磁碟清理

「 磁碟清理 」 工具 (cleanmgr.exe) 不存在的 Windows Server 2012 R2 或更早版本除非您已安裝桌面體驗功能。

若要使用 cleanmgr.exe，如先前所述，先安裝桌面體驗，或已在伺服器、 cleanmgr.exe 和 cleanmgr.exe.mui 上出現的兩個檔案複製。 您可以使用下表來尋找適用於您作業系統的檔案。

| 作業系統  | Architecture  | 檔案位置  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

找出 cleanmgr.exe，並將檔案移至 **%systemroot%\System32**。

找出 cleanmgr.exe.mui，並將檔案移至 **%systemroot%\System32\en-US**。

您現在可以啟動 [磁碟清理] 工具，從命令提示字元中執行 Cleanmgr.exe 或依序按一下**開始**，然後輸入 **[cleanmgr]** 在搜尋列。

若要讓磁碟清理 按鈕出現在磁碟的 內容 對話方塊，您也必須安裝 「 桌面體驗 」 功能。

## <a name="additional-references"></a>其他參考資料

[釋出磁碟空間，在 Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
