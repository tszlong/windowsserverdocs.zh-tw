---
title: 快取與記憶體管理員的改良功能
description: Windows Server 2016 中快取與記憶體管理員的改良功能
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c2fdceb7ff3743890c73ee4108ffcf8e00fe437f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992060"
---
# <a name="cache-and-memory-manager-improvements"></a>快取與記憶體管理員的改良功能

本主題說明 Windows Server 2012 和2016中的快取管理員和記憶體管理員改良功能。

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Windows Server 2016 中的快取管理員改良功能
快取管理員也新增了真正非同步快取讀取的支援。
如果應用程式高度依賴非同步快取讀取，這可能會改善其效能。雖然大部分的內建檔案系統都支援非同步快取讀取一段時間，但通常是因為各種與處理執行緒集區和檔案系統的內部工作佇列相關的設計選項而產生的效能限制。透過內核適當的支援，快取管理員現在會從檔案系統隱藏所有線程集區和工作佇列管理的複雜性，使其更有效率地處理非同步快取讀取。「快取管理員」有一組控制項 datastructures，適用于每個 (系統支援的) VHD 嵌套層級上限，以最大化平行處理原則。


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的快取管理員改良功能
除了快取管理員的增強功能之外，還會針對連續的工作負載讀取先前的邏輯，並新增了新的 API [CcSetReadAheadGranularityEx](/windows-hardware/drivers/ifs/ccsetreadaheadgranularityex) ，讓檔案系統驅動程式（例如 SMB）變更其讀取先行參數。 它藉由傳送多個小型讀取預先要求，而不是傳送單一的大量讀取要求，來為遠端檔案案例提供更好的輸送量。 只有核心元件（例如檔案系統驅動程式）可以透過程式設計方式，針對每個檔案來設定這些值。

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的記憶體管理員改良功能
啟用頁面結合可能會減少伺服器上的記憶體使用量，其具有許多具有相同內容的私用分頁頁面。 例如，執行相同記憶體密集型應用程式之多個實例的伺服器，或使用高度重復資料的單一應用程式，可能是嘗試合併頁面的絕佳候選項目。 啟用頁面結合的缺點是增加 CPU 使用量。

以下是一些伺服器角色的範例，其中的頁面結合不太可能提供太多好處：

-   檔案伺服器 (大部分的記憶體都是由不是私用，因此無法組合的檔案頁面所耗用) 

-   設定為使用 AWE 或大型頁面的 Microsoft SQL Server (大部分的記憶體都是私用的，但無法分頁) 

預設會停用頁面結合，但可以使用[Mmagent.ps1](/powershell/module/mmagent/enable-mmagent?view=win10-ps) Windows PowerShell Cmdlet 來啟用。 已在 Windows Server 2012 中新增頁面結合。