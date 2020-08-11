---
title: 版本資訊 - Windows Server 2016 的重要問題
description: 摘要說明需要因應措施以避免損毀、停止回應、安裝失敗、資料遺失的重要問題。
ms.date: 11/13/2018
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: b7e86b0841023548b1df1937bdf0820d59e12292
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990507"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>版本資訊：Windows Server 2016 中的重要問題

>適用於：Windows Server 2016

這些版本資訊摘要說明 Windows Server 2016 作業系統中最重要的問題，包括可避免或暫時解決問題 (如果已知) 的方式。 如需這個版本的設計變更、新功能及修正的相關資訊，請參閱 [Windows Server 2016 的新功能](whats-new-in-windows-server-2016.md)和特定功能小組的通知。 除非另有說明，否則這其中每個回報的問題都適用於 Windows Server 2016 的所有版本和安裝選項。

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。

## <a name="express-updates-available-starting-in-november-2018-new"></a>2018 年 11 月起可取得快速更新 (新)

2018 年 11 月起的「更新星期二」更新，Windows 將會再次發佈適用於 Windows Server 2016 的[快速更新](express-updates.md)。 如果您使用 WSUS 和 Configuration Manager，將會再次看到適用於 Windows Server 2016 的兩個更新套件：「完整」更新和「快速」更新。 如果您想為您的伺服器環境使用「快速」，您必須確認該伺服器自 2017 年 11 月 (KB# 4048953) 起已接受完整更新，以確保快速更新可正確地安裝。 如果您嘗試在自 2017 年 11B (KB# 4048953) 起未接受完整更新的伺服器上採用快速更新，您會看到無限迴圈取用頻寬和 CPU 資源的重複失敗。 如果您遇到此情況，請停止推送快速更新，並改為推送最新完整更新，以停止失敗迴圈。

## <a name="server-core-installation-option"></a>Server Core 安裝選項

[comment]: # (ID:370; Submitter: amason; state: 已核准)

當您使用 Server Core 安裝選項安裝 Windows Server 2016 時，預設就會安裝並啟動列印多工緩衝處理器，即使未安裝列印伺服器角色時也是一樣。

若要避免這種情況，請在第一次開機之後將列印多工緩衝處理器設定為停用。

## <a name="containers"></a>容器

[comment]: # (ID:371; Submitter: taylorb; state: 已核准)
- 使用容器之前，請先安裝 [Windows 10 1607 版的服務堆疊更新：2016 年 8 月 23 日](https://support.microsoft.com/kb/3176936)或任何可用的更新版本。 否則，可能會發生許多問題，包括建置、啟動，或執行容器失敗，以及類似下列的錯誤訊息：CreateProcess 在 Win32 中失敗:RPC 伺服器無法使用。

[comment]: # (ID:373; Submitter: plang; state: 已核准)
- NanoServerPackage OneGet 提供者無法在 Windows 容器中運作。 若要解決這個問題，請在不同的電腦 (非容器) 上使用 Find-NanoServerPackage 和 Save-NanoServerPackage，來下載所需的套件。 然後將套件複製到容器並進行安裝。

## <a name="device-guard"></a>Device Guard

[comment]: # (ID:369; Submitter: nirb; state: 已核准)
如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID:375; Submitter: wgries; state: 已核准)
如果您嘗試在 Windows Server 2016 上執行 Microsoft Exchange 2016 CU3，IIS 主機處理程序 W3WP.exe 就會發生錯誤。 目前尚無解決的方法。 您應該在有可用的支援修正程式之前，延後部署 Windows Server 2016 上的 Exchange 2016 CU3。

## <a name="remote-server-administration-tools-rsat"></a>遠端伺服器管理工具 (RSAT)

[comment]: # (ID:374; Submitter: ryanpu; state: 已核准)
如果您執行的 Windows 10 版本比 Anniversary Update 還要舊，並且使用 Hyper-V 以及已啟用虛擬可信賴平台模組的虛擬機器 (包含受防護虛擬機器)，然後安裝針對 Windows Server 2016 所提供的 RSAT 版本，則嘗試啟動這些虛擬機器將會失敗。

若要避免這個問題，請在安裝 RSAT 之前將用戶端電腦升級為 Windows 10 Anniversary Update (或更新版本)。 如果已經發生這個問題，請解除安裝 RSAT，並將用戶端升級為 Window 10 Anniversary Update，然後重新安裝 RSAT。

## <a name="shielded-virtual-machines"></a>受防護的虛擬機器

[comment]: # (ID:369; Submitter: nirb; state: 已核准)
- 請確定您在生產環境中部署受防護的虛擬機器之前，已安裝所有可用的更新。

- 如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。

## <a name="start-menu"></a>開始功能表

[comment]: # (ID:372; Submitter: samli; state: 已核准)
這個問題會影響使用 [含桌面體驗的伺服器] 選項所安裝的 Windows Server 2016。

如果您在安裝應用程式時將捷徑項目新增至 [開始]  功能表的資料夾內，則除非先登出再重新登入，否則捷徑無法運作。

回到主要 [Windows Server 2016](../index.yml) 中樞。

## <a name="storport-performance"></a>Storport 效能

相較於 Windows Server 2012 R2，執行 Windows Server 2016 的全新安裝時，部分系統可能會出現儲存空間效能降低的情形。  在 Windows Server 2016 的開發期間我們做了一些變更，以改善平台的安全性及可靠性。 其中有些變更 (像是預設啟用 Windows Defender)，會導致較長的 I/O 路徑，因此降低特定工作負載和模式中的 I/O 效能。 Microsoft 不建議停用 Windows Defender，因為它是您系統很重要的一層保護。 

## <a name="copyright"></a>著作權

本文件是以原本的形式提供。 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件供內部參照之用。

&copy; 2016 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft、Active Directory、Hyper-V、Windows 及 Windows Server 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。

1.0