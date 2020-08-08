---
title: 如何判斷複寫資料夾的最小暫存區域 DFSR 需求
description: 本文是快速參考指南，說明如何計算 DFSR 正常運作所需的最小臨時區域。
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 581b485f219e960ecd467baa1f7dff7742c3acf8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965785"
---
# <a name="how-to-determine-the-minimum-staging-area-dfsr-needs-for-a-replicated-folder"></a>如何判斷複寫資料夾的最小暫存區域 DFSR 需求

本文是快速參考指南，說明如何計算 DFSR 正常運作所需的最小臨時區域。 低於這些值可能會導致複寫速度變慢或完全停止。

請記住，這些*只是最低*的。 考慮臨時區域大小時，暫存區域愈大，最高可達複寫資料夾的大小。 請參閱本文結尾的「如何判斷您是否有臨時區域問題」一節，以取得更多有關為何要有適當大小的臨時區域的詳細資訊。

> [!Note]
> 我們也有可協助您計算預備大小的修復程式。 [DFS 複寫 (DFSR) 管理介面的更新可供使用](https://support.microsoft.com/kb/2607047)

## <a name="rules-of-thumb"></a>經驗法則

**Windows Server 2003 R2** –臨時區域配額必須和複寫資料夾中9個最大檔案的大小相同

**Windows Server 2008 和 2008 R2** –臨時區域配額必須和複寫資料夾中32個最大檔案的大小相同

初始複寫會比日常複寫更能使用臨時區域。 如果您有可用的磁片磁碟機空間，強烈建議您在初始複寫期間將暫存區域設定為高於最小值

## <a name="where-do-i-get-powershell"></a>哪裡可以取得 PowerShell？

PowerShell 包含在 Windows 2008 和更新版本中。 您必須在 Windows Server 2003 上安裝 PowerShell。 您可以在[這裡](https://support.microsoft.com/kb/968930)下載適用于 Windows 2003 的 PowerShell。

## <a name="how-do-you-find-these-x-largest-files"></a>您要如何找到這些 X 個最大的檔案？

您可以使用 PowerShell 腳本來尋找32或9個最大的檔案，並判斷它們加到 (的 gb 數，因為) PowerShell 命令的 Ned Pyle。 我實際上會提供三個 PowerShell 腳本。 每個都有其專屬的功能;不過，數位3最有用。

1. 執行下列命令：
   ```Powershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | ft name,length -wrap –auto
   ```

   此命令會傳回檔案名和檔案大小（以位元組為單位）。 如果您想要知道複寫資料夾中最大的32檔案，讓您可以「造訪」其擁有者，這就很有用。

2. 執行下列命令：
   ```Poswershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   ```
   此命令會傳回資料夾中32最大檔案的總位元組數，而不列出檔案名。

3. 執行下列命令：
   ```Poswershell
   $big32 = Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum

   $big32.sum /1gb
   ```
   此命令會取得資料夾中32個最大檔案的總位元組數，並執行數學以將位元組轉換成 gb。 此命令是兩個不同的行。 您可以一次將這兩個專案貼入 PowerShell 命令 shell，或將其重新執行回來。

## <a name="manual-walkthrough"></a>手動逐步解說

為了示範此程式並希望進一步瞭解我們所做的事，我要手動逐步執行每個部分。

執行命令1將會傳回類似下列輸出的結果。 為了簡潔起見，此範例只會使用16個檔案。 適用于 Windows 2008 和更新版本的作業系統，請一律使用32，而在 Windows 2003 R2 中則是9

### <a name="example-data-returned-by-powershell"></a>PowerShell 傳回的範例資料

<table>
<tbody>
<tr class="odd">
<td>名稱</td>
<td>長度</td>
</tr>
<tr class="even">
<td><strong>File5.zip</strong></td>
<td>10286089216</td>
</tr>
<tr class="odd">
<td><strong>archive.zip</strong></td>
<td>6029853696</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>5751522304</td>
</tr>
<tr class="odd">
<td> <strong>file9.zip</strong></td>
<td>5472683008</td>
</tr>
<tr class="even">
<td><strong>MENTOS.zip</strong></td>
<td>5241586688</td>
</tr>
<tr class="odd">
<td><strong>File7.zip</strong></td>
<td>4321264640</td>
</tr>
<tr class="even">
<td><strong>file2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="odd">
<td><strong>frd2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>4078994432</td>
</tr>
<tr class="odd">
<td><strong>File44.zip</strong></td>
<td>4058424320</td>
</tr>
<tr class="even">
<td><strong>file11.zip</strong></td>
<td>3858056192</td>
</tr>
<tr class="odd">
<td><strong>Backup2.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="even">
<td><strong>BACKUP3.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="odd">
<td><strong>Current.zip</strong></td>
<td>3576931328</td>
</tr>
<tr class="even">
<td><strong>Backup8.zip</strong></td>
<td>3307488256</td>
</tr>
<tr class="odd">
<td><strong>File999.zip</strong></td>
<td>3274982400</td>
</tr>
</tbody>
</table>

### <a name="how-to-use-this-data-to-determine-the-minimum-staging-area-size"></a>如何使用此資料來判斷最小的臨時區域大小：

  - Name = 檔案名。
  - 長度 = 位元組
  - 1 Gb = 1073741824 個位元組

首先，您必須加總位元組總數。 接下來，將總計除以1073741824。 我建議使用 Excel 或您選擇的試算表來執行數學運算。

### <a name="example"></a>範例

從上述範例中，總位元組數 = 75241684992。 若要取得所需的最小預備區域配額，必須將75241684992除以1073741824。

> 75241684992/1073741824 = 70.07 GB

根據這項資料，如果我進位到最接近的整數，就會將暫存區域設定為 71 GB。

### <a name="real-world-scenario"></a>真實世界案例：

雖然手動逐步解說很有趣，但可能不是您自己的時間可以自行進行運算的最佳用法。 若要將此程式自動化，請使用上述範例中的命令3。 結果看起來會像這樣

> [![包](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/8204.image_thumb_02CB3914.png "image")](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/0876.image_03A39EFE.png)

使用範例命令3而不進行任何額外的工作，除非四捨五入到最接近的整數，我可以判斷我需要一個適用于 d：檔的 6 GB 臨時區域配額 \\ 。

## <a name="do-i-need-to-reboot-or-restart-the-service-for-the-changes-to-be-picked-up"></a>我需要重新開機或重新開機服務，才能挑選變更嗎？

暫存區域配額的變更不需要重新開機或重新開機服務就會生效。 您必須等候 AD 複寫和 DFSR 的 AD 輪詢週期，才會套用變更。

## <a name="how-to-determine-if-you-have-a-staging-area-problem"></a>如何判斷您是否有臨時區域問題

您可以藉由監視 DFSR 伺服器上的特定事件識別碼來偵測臨時區域問題。 事件清單為4202、4204、4206、4208和4212。 以下列出這些事件的文字。 請務必區分4202和4204與其他事件。 在正常作業條件下，可以記錄大量的4202和4204事件。 請將4202和4204事件視為類似于採取您的脈衝，而4206、4208和4212就像櫃的難題。 我會在下面說明如何解讀您的4202和4204事件。

### <a name="staging-area-events"></a>臨時區域事件

> 事件識別碼： **4202**嚴重性：**警告**
>
> DFS 複寫服務偵測到，在本機路徑 (路徑) 的複寫資料夾所使用的暫存空間高於高水位線。 服務會嘗試刪除最舊的暫存檔案。 效能可能會受到影響。
>
> 事件識別碼： **4204**嚴重性：**資訊**
>
> DFS 複寫服務已成功刪除本機路徑 (路徑) 上複寫資料夾的舊暫存檔案。 接下來的空間低於高水位線。
>
> 事件識別碼： **4206**嚴重性：**警告**
>
> DFS 複寫服務無法在本機路徑 (路徑) ，清除複寫資料夾的舊暫存檔案。 服務可能無法複寫一些大型檔案，且複寫資料夾可能無法同步。服務會在 (x) 分鐘內自動重試暫存空間清除。 如果服務偵測到某些暫存檔案已解除鎖定，可能會稍早啟動清理。
>
> 事件識別碼： **4208**嚴重性：**警告**
>
> DFS 複寫服務偵測到暫存空間使用量高於本機路徑 (路徑) 上複寫資料夾的暫存配額。 服務可能無法複寫一些大型檔案，且複寫資料夾可能無法同步。服務會嘗試自動清除暫存空間。
>
> 事件識別碼： **4212**嚴重性：**錯誤**
>
> DFS 複寫服務無法在本機路徑 (路徑) 複寫複寫資料夾，因為暫存路徑無效或無法存取。

## <a name="what-is-the-difference-between-4202-and-4208"></a>4202與4208之間的差異為何？

事件4202和4208有類似的文字;亦即，DFSR 偵測到臨時區域使用量超過上限標準。 差別在於在執行臨時區域清除之後，會記錄4208，而且仍會超過暫存配額。 4202是正常且預期的事件，而4208則不正常，需要介入。

## <a name="how-many-4202-4204-events-are-too-many"></a>有多少4202、4204個事件太多？

此問題沒有單一答案。 不同于4206、4208或4212事件，它們一律是不良的，並且表示需要採取動作，4202和4204事件會在正常操作條件下發生。 看到許多4202和4204事件*可能*表示有問題。 需考量的事項：

1.  複寫資料夾 (RF) 記錄4202執行初始複寫嗎？ 若是如此，記錄4202和4204事件是正常的。 在初始複寫期間，您可以盡可能提供最多的暫存區域，以盡可能減少這些內容
2.  只要檢查4202事件的總數並不足夠。 您必須知道每個 RF 記錄的數目。 如果您在24小時內記錄一個 RF 的 20 4202 事件，這是高的時間。 不過，如果您有20個複寫的資料夾，而且每個資料夾都有一個事件，那麼您就會很順利。
3.  您應該檢查數天的資料來建立趨勢。

我通常會在正常作業條件下，針對每個複寫資料夾的每個複寫資料夾，建議客戶一次不超過 1 4202 個事件。 「正常」表示不會發生初始複寫。 基於下列原因，我的理由是：

1.  清理暫存區域所花費的時間是花在不復寫檔案的時間。 當臨時區域已清除時，複寫就會暫停。
2.  DFSR 從完整的臨時區域中受益于 RDC 和跨檔案 RDC，或將相同的檔案複寫至其他成員
3.  越多的4202和4204事件，您就可以將問題記錄在 DFSR 無法清除臨時區域的情況下，或是必須從臨時區域中提前清除檔案。
4.  4206、4208和4212事件在我的經驗中，一律會在較大量的4202和4204事件前後。

雖然每天僅允許每個 RF 1 4202 事件很保守，但它會大幅降低您遇到臨時區域問題的機率，並更有效地利用 DFSR 伺服器的資源來複寫檔案的目的。


