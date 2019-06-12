---
title: 版本資訊 - Windows Server 2016 的重要問題
description: 摘要說明需要因應措施以避免損毀、停止回應、安裝失敗、資料遺失的重要問題。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: dec1ec184a147ef4fae64e9cc4384c0b6e4510b6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749533"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>版本資訊：Windows Server 2016 的重要問題

>適用於：Windows Server 2016

這些版本資訊摘要說明在 Windows Server 2016 作業系統，包括如何避免或解決問題，如果已知中最重要的問題。 如需這個版本的設計變更、新功能及修正的相關資訊，請參閱 [Windows Server 2016 的新功能](whats-new-in-windows-server-2016.md)和特定功能小組的通知。 除非另有說明，否則這其中每個回報的問題都適用於 Windows Server 2016 的所有版本和安裝選項。

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。

## <a name="express-updates-available-starting-in-november-2018-new"></a>快速開始 （新） 年 11 月 2018年中提供的更新

從 11 月 2018年"更新 Tuesday"更新，Windows 將會一次會發行[快速更新](express-updates.md)適用於 Windows Server 2016。 如果您使用 WSUS 和 System Center Configuration Manager (SCCM)，您會再次看到兩個更新套件的 Windows Server 2016： 完整的更新，以及快速更新。 如果您想要用於伺服器環境中的 Express，您需要確認伺服器已自 2017 年 11 月 (KB # 4048953) 以確保正確安裝快速更新的完整更新。 如果您嘗試快速更新自 2017年 11B update (KB # 4048953) 尚未更新的伺服器上，您會看到重複使用的頻寬和 CPU 資源在無限迴圈中的失敗。 如果您收到這種情況，停止推播 Express update，並改為推播停止失敗迴圈的最新的完整更新。

## <a name="server-core-installation-option"></a>Server Core 安裝選項

[comment]: # (識別碼：370;提交者： amason;狀態： 已核准)

當您使用 Server Core 安裝選項安裝 Windows Server 2016 時，預設就會安裝並啟動列印多工緩衝處理器，即使未安裝列印伺服器角色時也是一樣。

若要避免這種情況，請在第一次開機之後將列印多工緩衝處理器設定為停用。

## <a name="containers"></a>容器

[comment]: # (識別碼：371;提交者： taylorb;狀態： 已核准)
- 使用容器之前，先安裝[Windows 10 版本 1607年的服務堆疊更新：2016 年 8 月 23 日](https://support.microsoft.com/en-us/kb/3176936)或任何可用的更新版本更新。 否則，會發生一些問題，在建置，包括失敗啟動，或執行容器，並出現類似下列錯誤 「 CreateProcess 在 Win32 中所失敗：RPC 伺服器無法使用。 」

[comment]: # (識別碼：373;提交者： plang;狀態： 已核准)
- NanoServerPackage OneGet 提供者無法在 Windows 容器中運作。 若要解決這個問題，請在不同的電腦 (非容器) 上使用 Find-NanoServerPackage 和 Save-NanoServerPackage，來下載所需的套件。 然後將套件複製到容器並進行安裝。

## <a name="device-guard"></a>Device Guard

[comment]: # (識別碼：369;提交者： nirb;狀態： 已核准)
如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (識別碼：375;提交者： wgries;狀態： 已核准)
如果您嘗試在 Windows Server 2016 上執行 Microsoft Exchange 2016 CU3，IIS 主機處理程序 W3WP.exe 就會發生錯誤。 目前尚無解決的方法。 您應該在有可用的支援修正程式之前，延後部署 Windows Server 2016 上的 Exchange 2016 CU3。

## <a name="remote-server-administration-tools-rsat"></a>遠端伺服器管理工具 (RSAT)

[comment]: # (識別碼：374;提交者： ryanpu;狀態： 已核准)
如果您正在執行 Windows 10 年度更新版，比舊的版本並使用 HYPER-V 和虛擬機器已啟用虛擬信賴平台模組 （包括受防護的虛擬機器），然後安裝新版 Windows 提供 RSATServer 2016 中，嘗試啟動這些虛擬機器將會失敗。

若要避免這個問題，請在安裝 RSAT 之前將用戶端電腦升級為 Windows 10 Anniversary Update (或更新版本)。 如果已經發生這個問題，請解除安裝 RSAT，並將用戶端升級為 Window 10 Anniversary Update，然後重新安裝 RSAT。

## <a name="shielded-virtual-machines"></a>受防護的虛擬機器

[comment]: # (識別碼：369;提交者： nirb;狀態： 已核准)  
- 請確定您在生產環境中部署受防護的虛擬機器之前，已安裝所有可用的更新。

- 如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。

## <a name="start-menu"></a>開始功能表

[comment]: # (識別碼：372;提交者： samli;狀態： 已核准)
這個問題會影響使用 [含桌面體驗的伺服器] 選項所安裝的 Windows Server 2016。

如果您在安裝應用程式新增捷徑資料夾內的項目**啟動** 功能表捷徑將無法運作直到您登出，並記錄一次備份中。

回到主要 [Windows Server 2016](Windows-Server-2016.md) 中樞。

## <a name="storport-performance"></a>Storport 效能

相較於 Windows Server 2012 R2，執行 Windows Server 2016 的全新安裝時，部分系統可能會出現儲存空間效能降低的情形。  在 Windows Server 2016 的開發期間我們做了一些變更，以改善平台的安全性及可靠性。 其中有些變更，例如預設啟用 Windows Defender，會導致較長的 I/O 路徑，因此降低特定工作負載和模式中的 I/O 效能。 Microsoft 不建議停用 Windows Defender，因為它是您系統很重要的一層保護。  

## <a name="copyright"></a>著作權

本文件是以原本的形式提供， 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。  

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件，但僅限內部參考之用途。  

&copy;2016 Microsoft Corporation. 保留一切權利。  

Microsoft、Active Directory、Hyper-V、Windows 及 Windows Server 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。  

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。  

1.0
