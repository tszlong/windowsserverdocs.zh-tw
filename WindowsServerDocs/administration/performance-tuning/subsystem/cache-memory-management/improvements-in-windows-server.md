---
title: 快取和記憶體管理員改進
description: Windows Server 2016 中快取與記憶體管理員的改良功能
ms.topic: article
ms.author: pavel
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 799598223812f5992db0354780424f7da13033ea
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078005"
---
# <a name="cache-and-memory-manager-improvements"></a>快取和記憶體管理員改進

本主題說明 Windows Server 2012 和2016中的快取管理員和記憶體管理員改進。

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Windows Server 2016 中的快取管理員改進
快取管理員也加入了對真正非同步快取讀取的支援。
如果應用程式高度依賴非同步快取讀取，這可能會改善應用程式的效能。雖然大部分的內建檔案系統都支援非同步快取讀取一段時間，但由於與處理執行緒集區和檔案系統內部工作佇列相關的各種設計選項，通常會有效能上的限制。快取管理員可透過核心適當的支援，從檔案系統中隱藏所有線程集區和工作佇列管理的複雜度，使其更有效率地處理非同步快取的讀取。快取管理員針對每個 (系統所支援的一組控制項 >datastructures，支援最大) VHD 的嵌套層級，以最大化平行處理原則。


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的快取管理員改進
除了快取管理員增強功能，可針對連續工作負載預先讀取邏輯之外，還新增了一個新的 API [CcSetReadAheadGranularityEx](/windows-hardware/drivers/ifs/ccsetreadaheadgranularityex) ，讓檔案系統驅動程式（例如 SMB）變更其預先讀取參數。 它會藉由傳送多個小型的預先讀取要求，而不是傳送單一的大量讀取要求，讓遠端檔案案例更能提供更佳的輸送量。 只有核心元件（例如檔案系統驅動程式）可以用程式設計方式，以每個檔案為基礎來設定這些值。

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的記憶體管理員改進
啟用頁面結合可能會降低具有許多私用、可分頁頁面且內容相同的伺服器上的記憶體使用量。 例如，執行多個相同記憶體密集型應用程式之多個實例的伺服器，或是使用高度重復資料的單一應用程式，可能是嘗試頁面組合的絕佳候選項目。 啟用頁面組合的缺點是會增加 CPU 使用量。

以下是一些伺服器角色範例，其中的頁面組合不太可能提供許多優點：

-   檔案伺服器 (大部分的記憶體都是由非私用且無法組合的檔案頁面所使用) 

-   設定為使用 AWE 或大型頁面的 Microsoft SQL Server (大部分的記憶體都是私用的，但無法分頁) 

預設會停用頁面合併，但可使用 [mmagent.ps1](/powershell/module/mmagent/enable-mmagent?view=win10-ps) Windows PowerShell Cmdlet 來啟用。 Windows Server 2012 中已新增頁面合併。