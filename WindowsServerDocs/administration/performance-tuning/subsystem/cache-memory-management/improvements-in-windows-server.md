---
title: 快取和記憶體管理員的改進
description: 快取和記憶體管理員在 Windows Server 2016 中的改進
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868579"
---
# <a name="cache-and-memory-manager-improvements"></a>快取和記憶體管理員的改進

本主題說明在 Windows Server 2012 和 2016年中的快取管理員和記憶體管理員增強功能。

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Windows Server 2016 中的快取管理員增強功能
快取管理員也會新增支援，則為 true 非同步快取讀取。
如果它依賴非同步快取的讀取，這可能無法改善應用程式的效能。  雖然大部分的內建檔案系統有支援非同步快取讀取一段時間，但已通常因為處理執行緒集區和檔案系統的內部工作佇列的相關的各種設計選擇的效能限制。  從核心適當的支援，快取管理員現在會隱藏所有執行緒集區並從檔案系統，讓它更有效率地處理非同步工作佇列管理複雜性快取讀取。快取管理員有一組控制項 datastructures （系統支援最多） 的每個 VHD 巢狀層級以最大化平行處理原則。


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的快取管理員增強功能
除了讀取預先邏輯的循序工作負載，新的 API 的快取管理員增強功能[CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx)加入讓檔案系統驅動程式，例如 SMB，變更其讀取的預先參數。 它可以傳送多個小型預先讀取要求而不是傳送單一大型讀取預先要求，以允許遠端檔案的案例更佳的輸送量。 只有核心元件，例如檔案系統驅動程式，以程式設計的方式可以在每個檔案上設定這些值。

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的記憶體管理員增強功能
啟用頁面合併可能會降低有很多的私用的可分頁的頁面，具有相同內容的伺服器上的記憶體使用量。 比方說，執行相同的記憶體密集應用程式，或適用於高度重複的資料，在單一應用程式的多個執行個體的伺服器可能會很好的候選項目嘗試結合的頁面。 啟用頁面合併的缺點是增加的 CPU 使用量。

以下是一些範例伺服器角色的情況下，結合的頁面不太可能讓很多優點的其中：

-   檔案伺服器 （大部分的記憶體供不是私用，因此沒有可結合的檔案頁面）

-   設定為使用 AWE 或大型分頁 （大部分的記憶體是私用，但不可分頁） 的 Microsoft SQL Server

預設會停用合併的頁面，但可以藉由啟用[啟用 MMAgent](https://technet.microsoft.com/library/jj658954.aspx) Windows PowerShell cmdlet。 Windows Server 2012 中新增合併的頁面。
