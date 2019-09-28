---
title: ReFS 完整性資料流
description: ''
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: 0e41d7ae577bf7e9227ff0c02689d916f1008a3d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403041"
---
# <a name="refs-integrity-streams"></a>ReFS 完整性資料流
>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server （半年通道）、Windows 10

完整性資料流是 ReFS 的選用功能，可使用總和檢查碼驗證和維護資料的完整性。 雖然 ReFS 一律會在中繼資料上使用總和檢查碼，但 ReFS 預設不會在檔案資料上產生或驗證總和檢查碼。 完整性資料流這項功能可讓使用者針對檔案資料使用總和檢查碼。 啟用完整性資料流時，ReFS 可以明確判斷資料為有效或損毀。 此外，ReFS 與儲存空間可共同自動修正損毀的中繼資料及資料。

## <a name="how-it-works"></a>運作方式 

個別檔案、目錄或整個磁碟區可啟用完整性資料流，且完整性資料流設定可隨時切換。 此外，檔案及目錄的完整性資料流設定繼承自其父目錄。 

一旦啟用完整性資料流，ReFS 將為該檔案中繼資料夾中的特定檔案建立和維護總和檢查碼。 此總和檢查碼允許 ReFS 在存取之前先驗證該資料的完整性。 在傳回任何啟用完整性資料流的資料之前，ReFS 會先計算它的總和檢查碼：

![檔案資料的計算總和檢查碼](media/compute-checksum.gif)

然後，此總和檢查碼將會與檔案中繼資料的總和檢查碼進行比對。 如果總和檢查碼相符，則會將資料標記為有效並傳回至使用者。 如果總和檢查碼不相符，則資料已損毀。 該磁碟區的復原功能將決定 ReFS 如何回應損毀：

- 如果 ReFS 裝載在無復原功能的簡單儲存空間或空磁碟機上，ReFS 會將錯誤傳回給使用者，且不會傳回損毀資料。 
- 如果 ReFS 裝載在有復原功能的鏡像或同位空間，ReFS 將會嘗試修正損毀。 
    - 若嘗試成功，ReFS 將套用修正寫入以還原資料的完整性，且會將有效的資料傳回給應用程式。 應用程式將不會察覺到任何損毀。
    - 若嘗試失敗，ReFS 將傳回錯誤。 

ReFS 將會在系統事件記錄檔中記錄所有損毀，而該記錄檔會反映是否已修正損毀。 

![更正寫入還原資料完整性](media/corrective-write.gif)

## <a name="performance"></a>效能 

雖然完整性串流能為系統提供更多的資料完整性，但是它也會產生效能成本。 有幾個不同原因會造成此狀況：
- 如果已啟用完整性資料流，所有的寫入作業將成為寫入時配置作業。 雖然這樣會因為 ReFS 不需要讀取或修改任何現有資料而避開讀取-修改-寫入的瓶頸，但是檔案資料卻經常變得片斷不全，反而延遲讀取。 
- 取決於系統的工作負載及基礎存放裝置，計算及驗證總和檢查碼的計算成本可致使增加 IO 延遲。 

完整性資料流會背負犧牲效能的代價，因此建議您在易受效能影響的系統上停用完整性資料流。 

## <a name="integrity-scrubber"></a>完整性清除程式

如上所述，ReFS 會自動在存取任何資料之前驗證資料完整性。 ReFS 還會使用背景清除程式，讓 ReFS 可以驗證不常存取的資料。 此清除程式會定期掃描磁碟區、識別隱藏的損毀，以及主動觸發修復任何毀損資料。

  >[!NOTE]
  >資料完整性清除程式只能驗證檔案已啟用資料流完整性的檔案資料。

清除程式預設會每四週執行一次，但還是可以在 Microsoft\Windows\Data Integrity Scan 底下的 [工作排程器] 中設定此時間間隔。 

## <a name="examples"></a>範例
若要監視及變更檔案資料完整性設定，ReFS 使用 **Get-FileIntegrity** 和 **Set-FileIntegrity** Cmdlet。

### <a name="get-fileintegrity"></a>Get-FileIntegrity
若要查看是否已為檔案資料啟用完整性資料流，請使用 **Get-FileIntegrity** Cmdlet。 

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

您也可以使用 **Get-Item** Cmdlet 來取得特定目錄中所有檔案的完整性資料流設定。 

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
若要為檔案資料啟用/停用完整性資料流，請使用 **Set-FileIntegrity** Cmdlet。 

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

您也可以使用 **Get-Item** Cmdlet 來設定特定資料夾中所有檔案的完整性資料流設定。 

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True 
```

**Set-FileIntegrity** Cmdlet 也可以直接在磁碟區及目錄上使用。 

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="see-also"></a>另請參閱

-   [ReFS 總覽](refs-overview.md)
-   [ReFS 區塊複製](block-cloning.md)
-   [儲存空間直接存取總覽](../storage-spaces/storage-spaces-direct-overview.md)
