---
title: 版本資訊 - WindowsServer 2016 的重要問題
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
ms.localizationpriority: medium
ms.openlocfilehash: 5a25a9152298a38ad77a377a87b71917ee586947
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507711"
---
# 版本資訊：WindowsServer 2016 的重要問題

>適用於︰WindowsServer 2016

這些版本資訊摘要說明 WindowsServer&reg; 2016 作業系統中最重要的問題，包括可避免或暫時解決問題 (如果已知) 的方式。 如需這個版本的設計變更、新功能及修正的相關資訊，請參閱 [WindowsServer 2016 的新功能](what-s-new-in-windows-server-2016.md)和特定功能小組的通知。 除非另有說明，否則這其中每個回報的問題都適用於 WindowsServer 2016 的所有版本和安裝選項。  

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。  

## 快速在年 11 月 2018 （新） 中開始提供的更新

從開始年 11 月 2018年 「 更新星期二 」 更新，Windows 會再次發行[快速更新](express-updates.md)Windows Server 2016。 如果您使用 WSUS 和 System Center Configuration Manager (SCCM)，您會再次看到兩個套件的 Windows Server 2016 更新： 完整更新和快速版更新。 如果您想要使用您的伺服器環境的快速版，您需要確認伺服器已採取自年 11 月 2017 (KB # 4048953) 以確保正確地安裝快速更新的完整更新。 如果您嘗試在尚未自 2017年 11B 更新 (KB # 4048953) 已更新的伺服器上快速更新，您會看到取用頻寬和 CPU 資源在無限迴圈中的重複的失敗。 如果您收到到這個案例中，停止推播快速更新之後，並改為推播最近的完整更新，以停止失敗迴圈。  

## Server Core 安裝選項
[comment]: # (識別碼：370；提交者：amason；狀態：已簽核)  
當您使用 Server Core 安裝選項安裝 WindowsServer 2016 時，預設就會安裝並啟動列印多工緩衝處理器，即使未安裝列印伺服器角色時也是一樣。

若要避免這種情況，請在第一次開機之後將列印多工緩衝處理器設定為停用。


## 容器  

[comment]: # (識別碼：371；提交者：taylorb；狀態：已簽核)  
- 使用容器之前，請先安裝 [Servicing stack update for Windows 10 Version 1607: August 23, 2016](https://support.microsoft.com/en-us/kb/3176936) (Windows 10 1607 版的服務堆疊更新：2016 年 8 月 23 日) 或任何可用的更新版本。 否則，可能會發生許多問題，包括建置失敗、啟動，或執行容器，以及與「CreateProcess 在 Win32 中失敗: RPC 伺服器無法使用」類似的錯誤。

[comment]: # (識別碼：373；提交者：plang；狀態：已簽核)  
- NanoServerPackage OneGet 提供者無法在 Windows 容器中運作。 若要解決這個問題，請在不同的電腦 (非容器) 上使用 Find-NanoServerPackage 和 Save-NanoServerPackage，來下載所需的套件。 然後將套件複製到容器並進行安裝。

## Device Guard
[comment]: # (識別碼：369；提交者：nirb；狀態：已簽核)
如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。

## Microsoft Exchange
[comment]: # (識別碼：375；提交者：wgries；狀態：已簽核)
如果您嘗試在 WindowsServer 2016 上執行 Microsoft Exchange 2016 CU3，IIS 主機處理程序 W3WP.exe 就會發生錯誤。 目前尚無解決的方法。 您應該在有可用的支援修正程式之前，延後部署 WindowsServer 2016 上的 Exchange 2016 CU3。

## 遠端伺服器管理工具 (RSAT)
[comment]: # (識別碼：374；提交者：ryanpu；狀態：已簽核)
如果您執行的 Windows10 版本比 Anniversary Update 還要舊，並且使用 Hyper-V 以及已啟用虛擬可信賴平台模組的虛擬機器 (包含受防護虛擬機器)，然後安裝針對 WindowsServer 2016 所提供的 RSAT 版本，則嘗試啟動這些虛擬機器將會失敗。

若要避免這個問題，請在安裝 RSAT 之前將用戶端電腦升級為 Windows10 Anniversary Update (或更新版本)。 如果已經發生這個問題，請解除安裝 RSAT，並將用戶端升級為 Window 10 Anniversary Update，然後重新安裝 RSAT。


## 受防護的虛擬機器
[comment]: # (識別碼：369；提交者：nirb；狀態：已簽核)  
- 請確定您在生產環境中部署受防護的虛擬機器之前，已安裝所有可用的更新。

- 如果您使用虛擬化型的程式碼完整性保護或受防護的虛擬機器 (其使用虛擬化型的程式碼完整性保護)，您應該注意，這些技術可能會與某些裝置和應用程式不相容。 您應該先在實驗室測試此類設定，才在生產系統上啟用這些功能。 若沒有這麼做，將可能導致未預期的資料遺失或停止回應錯誤。


## [開始] 功能表
[comment]: # (識別碼：372；提交者：samli；狀態：已簽核)
這個問題會影響使用 [含桌面體驗的伺服器] 選項所安裝的 Windows Server 2016。

如果您在安裝應用程式時將捷徑項目新增至 [開始] 功能表的資料夾內，則除非先登出再重新登入，否則捷徑無法運作。



回到主要 [WindowsServer 2016](Windows-Server-2016.md) 中樞。

## Storport 效能
相較於 Windows Server 2012 R2，執行 Windows Server 2016 的全新安裝時，部分系統可能會出現儲存空間效能降低的情形。在 Windows Server 2016 的開發期間我們做了一些變更，以改善平台的安全性及可靠性。 其中有些變更，例如預設啟用 Windows Defender，會導致較長的 I/O 路徑，因此降低特定工作負載和模式中的 I/O 效能。 Microsoft 不建議停用 Windows Defender，因為它是您系統很重要的一層保護。  

## 著作權  
本文件是以原本的形式提供， 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。  

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件供內部參照之用。  

&copy;2016 Microsoft Corporation. 著作權所有，並保留一切權利。  

Microsoft、Active Directory、Hyper-V、Windows 及 WindowsServer 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。  

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。  


1.0  
