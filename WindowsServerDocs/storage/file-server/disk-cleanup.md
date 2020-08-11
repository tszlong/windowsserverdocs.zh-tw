---
title: 在 Windows Server 上使用磁碟清理
description: 了解如何使用命令列選項來設定磁碟清理工具 (Cleanmgr.exe)，以自動清除特定檔案。
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.date: 06/20/2019
ms.openlocfilehash: cd7aa3dbb648e5083894f4344ea96ca1c1684aab
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971165"
---
# <a name="using-disk-cleanup-on-windows-server"></a>在 Windows Server 上使用磁碟清理

> 適用於：Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

磁碟清理工具會在 Windows Server 環境中清除不必要的檔案。 Windows Server 2019 和 Windows Server 2016 上預設會提供這項工具，但您可能必須採取幾個手動步驟，才能在舊版的 Windows Server 上啟用此工具。

若要啟動磁碟清理工具，請執行 Cleanmgr.exe 命令，或依序選取 [開始]  、[Windows 系統管理工具]  及 [磁碟清理]  。

您也可以使用 [cleanmgr Windows 命令](../../administration/windows-commands/cleanmgr.md)執行磁碟清理，並使用命令列選項指定磁碟清理來清除特定檔案。

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>安裝桌面體驗以在舊版 Windows Server 上啟用磁碟清理

請遵循下列步驟，使用 [新增角色及功能] 精靈在執行 Windows Server 2012 R2 或更早版本的伺服器上安裝桌面體驗，而這也會一併安裝磁碟清理。

1. 如果已經開啟伺服器管理員，請移至下一個步驟。 如果尚未開啟伺服器管理員，請執行下列其中一項動作來將它開啟。

   - 在 Windows 桌面上，按一下 Windows 工作列中的 [伺服器管理員]  來啟動 [伺服器管理員]。

   - 移至 [開始]  ，然後選取 [伺服器管理員] 磚。

1. 在 [管理]  功能表上，選取 [新增角色及功能]  。

1. 在 [在您開始前]  頁面上，確認您的目的地伺服器與網路環境已經可以安裝您想要的功能。 選取 [下一步]  。

1. 在 [選取安裝類型]  頁面上選取 [角色型或功能型安裝]  ，以在單一伺服器上安裝所有功能。 選取 [下一步]  。

1. 在 [選取目的地伺服器]  頁面上，從伺服器集區選取一部伺服器，或者選取一個離線 VHD。 選取 [下一步]  。

1. 在 [選取伺服器角色]  頁面上，選取 [下一步]  。

1. 在 [選取功能]  頁面上選取 [使用者介面和基礎結構]  ，然後選取 [桌面體驗]  。

1. 在 [新增桌面體驗所需的功能？]  中，選取 [新增功能]  。

1. 繼續進行安裝，然後重新啟動系統。

1. 確認 [磁碟清理]  選項按鈕已出現在 [內容] 對話方塊中。

   ![具有磁碟清理選項的磁碟內容對話方塊](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>以手動方式將磁碟清理新增至舊版的 Windows Server

除非您已安裝桌面體驗功能，否則 Windows Server 2012 R2 或更早版本上不會顯示「磁碟清理」工具 (cleanmgr.exe)。

若要使用 cleanmgr.exe，請安裝先前所述的桌面體驗，或複製伺服器上已存在的兩個檔案：cleanmgr.exe 和 cleanmgr.exe.mui。 使用下表來找出作業系統的檔案。

| 作業系統  | 架構  | 檔案位置  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe
| Windows Server 2008 R2 | 64 位元 | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

找出 ccleanmgr.exe，並將檔案移至 **%systemroot%\System32**。

找出 cleanmgr.exe.mui，並將檔案移至 **%systemroot%\System32\en-US**。

現在，您若要啟動磁碟清理工具，可以從命令提示字元中執行 Cleanmgr.exe，或按一下 [開始]  ，並在搜尋列中輸入 **Cleanmgr**。

若要讓 [磁碟清理] 按鈕顯示在磁碟的 [內容] 對話方塊上，您也必須安裝 [桌面體驗] 功能。

## <a name="additional-references"></a>其他參考資料

[在 Windows 10 中釋放磁碟機空間](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
