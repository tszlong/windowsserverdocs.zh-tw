---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: ReFS 上的區塊複製
description: ''
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-file-systems
ms.openlocfilehash: 54165700209320eee50fc63d98d78cbf4a92d053
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838109"
---
# <a name="block-cloning-on-refs"></a>ReFS 上的區塊複製

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

區塊複製會指示檔案系統代表應用程式複製一個範圍的檔案位元組，其中目的檔案可能與來源檔案相同或不同。 然而，複製作業因為會觸發基礎實體資料昂貴的讀取與寫入，所以成本高昂。 

不過，ReFS 上的區塊複製能作為低成本中繼資料作業執行複製，而無須讀取或寫入檔案資料。 因為 ReFS 可讓多個檔案共用相同的邏輯叢集 (磁碟區上的實體位置)，所以複製作業只須重相對應檔案區域來分隔實體位置，進而將昂貴的實體作業轉換為快速且具邏輯的作業。 這能更快完成複製，並產生較少的存放裝置 I/O。 這項改善同時使虛擬工作負載受益，有鑑於使用區塊複製作業時 .vhdx 檢查點合併作業會大幅加速。 此外，因為多個檔案可共用相同的邏輯叢集，所以相同的資料便不會以實體方式多次儲存，進而改善儲存容量。 
  
## <a name="how-it-works"></a>運作方式 

ReFS 上的區塊複製將檔案資料作業轉換為中繼資料作業。 為了達成此項最佳化，ReFS 為複製的區域將參考計數引入其中繼資料。 此參考計數會記錄參考相同實體區域的相異檔案區域數。 這能讓多個檔案共用相同的實體資料：

![在多個檔案參考相同區域時顯示參考計數更新](media/ref-count-example.gif)

ReFS 透過保留各個邏輯叢集的參考計數，讓檔案間的隔離不中斷：寫入共用區域會觸發寫入時配置機制，其中 ReFS 會為傳入的寫入配置新的區域。 此項機制保留了共用邏輯叢集的完整性。 

### <a name="example"></a>範例
假設有 X 和 Y 兩個檔案，其中兩個檔案都分別由三個區域組成，且各個區域都對應至不同的邏輯叢集。

![這兩個檔案各擁有三個相異區域且全對應至參考計數為 1 的區域](media/block-clone-1.png)

現在假設應用程式發出從檔案 X 至檔案 Y 的區塊複製作業，使區域 A 與 B 於區域 E 的位移進行複製。下列檔案系統狀態將導致：

![封鎖之複製區域的參考計數顯示為 2](media/block-clone-2.png)

此檔案系統狀態顯示區塊複製區域複製成功。 因為 ReFS 僅透過將 VCN 更新至 LCN 對應來執行此複製作業，所以未讀取任何實體資料，且未覆寫檔案 Y 中的任何實體資料。 檔案 X 與 Y 現在共用邏輯叢集，由資料表中的參考計數所反映。 因為未以實體方式複製任何資料，所以 ReFS 得以降低磁碟區上的容量消耗。 

現在假設應用程式嘗試覆寫檔案 X 中的區域 A。ReFS 將複製共用區域、適度更新參考計數，並執行新複製區域的傳入寫入。 如此可確保維持檔案間的隔離。   

![透過寫入新區域 G 及更新參考計數來保留隔離](media/block-clone-3.png)

修改寫入後，區域 B 仍由這兩個檔案所共用。 注意，若區域 A 大於叢集，便僅會複製修改的叢集，且剩餘的部分會維持為共用。


## <a name="functionality-restrictions-and-remarks"></a>功能限制與備註
- 來源與目的地區域必須以叢集界限作為開頭與結尾。 
- 複製的區域長度必須小於 4GB。 
- 可對應至相同實體區域的檔案區域數上限為 8175。
- 目的地區域不得延伸超過檔案結尾。 若應用程式要使用複製的資料延伸目的地，其必須先呼叫 [SetEndOfFile](https://msdn.microsoft.com/library/windows/desktop/aa365531(v=vs.85).aspx)。 
- 若來源與目的地區域位於相同檔案，其不得重疊。 (應用程式可繼續，方法是將區塊複製作業分割為多個不再重疊的多個區塊複製)。
- 來源與目的檔案必須位於相同的 ReFS 磁碟區。 
- 來源與目的檔案必須具有相同的[完整性資料流](https://msdn.microsoft.com/library/windows/desktop/gg258117(v=vs.85).aspx)設定。 
- 若來源檔案為疏鬆，目的檔案也必須為疏鬆。 
- 區塊複製作業會中斷「共用伺服器用戶端檔案鎖」(也稱為[層級 2 伺服器用戶端檔案鎖](https://msdn.microsoft.com/library/windows/desktop/aa365713(v=vs.85).aspx))。
- ReFS 磁碟區必須使用 Windows Server 2016 格式化，且若正在使用容錯移轉叢集，叢集功能等級的格式時間必須為 Windows Server 2016 或更新版本。 

## <a name="see-also"></a>另請參閱

-   [ReFS 概觀](refs-overview.md)
-   [ReFS 的完整性資料流](integrity-streams.md)
-   [儲存空間直接存取概觀](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](https://msdn.microsoft.com/library/windows/desktop/mt590821(v=vs.85).aspx)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](https://msdn.microsoft.com/library/windows/desktop/mt590823(v=vs.85).aspx)
