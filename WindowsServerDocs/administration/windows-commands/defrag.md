---
title: defrag
description: 重組的 Windows 命令主題，它會在本機磁片區上尋找併合並分散的檔案，以改善系統效能。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8723afc936fa1ea311e275a58a85b20988f92a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846711"
---
# <a name="defrag"></a>defrag

>適用于： Windows 10、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

尋找併合並本機磁片區上的分散檔案，以改善系統效能。

若要執行此命令，至少需要本機**Administrators**群組的成員資格或同等許可權。

## <a name="syntax"></a>語法
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|`<volume>`|指定要進行磁碟重組或分析之磁片區的磁碟機號或掛接點路徑。|
|A|在指定的磁片區上執行分析。|
|C|在所有磁片區上執行操作。|
|D|執行傳統磁碟重組（這是預設值）。 不過，在分層磁片區上，傳統的磁碟重組只會在容量層上執行。|
|E|在除了指定的磁片區上執行作業。|
|G|將指定磁片區上的儲存層優化。|
|H|以一般優先權執行作業（預設值為 [低]）。|
|I n|在每個磁片區上最多可執行 n 秒的層優化。|
|K|在指定的磁片區上執行樓板匯總。|
|L|在指定的磁片區上執行 retrim。|
|M [n]|在背景中平行執行每個磁片區上的作業。 最多 n 個執行緒會平行優化儲存層。|
|O|針對每種媒體類型執行適當的優化。|
|T|追蹤在指定磁片區上已在進行中的作業。|
|U|在螢幕上列印工作的進度。|
|V|列印包含片段統計資料的詳細資訊輸出。|
|X|在指定的磁片區上執行可用空間匯總。|
|?|顯示此說明資訊。|

## <a name="remarks"></a>備註
- 您無法重組特定類型的檔案系統磁片區或磁片磁碟機：
  -   您無法重組檔案系統已鎖定的磁片區。
  -   您無法重組檔案系統已標示為中途的磁片區，這表示可能有損毀。 您必須先在中途磁片區上執行**chkdsk** ，然後才能進行重組。 您可以使用 [ **fsutil** dirty query] 命令來判斷磁片區是否已變更。 如需有關**chkdsk**和**fsutil** dirty 的詳細資訊，請參閱[其他參考](defrag.md#BKMK_additionalRef)資料。
  -   您無法重組網路磁碟機機。
  -   您無法重組 cdROMs。
  -   您無法重組非**NTFS**、 **ReFS**、 **Fat**或**Fat32**的檔案系統磁片區。
- 有了 Windows Server 2008 R2、Windows Server 2008 和 Windows Vista，您可以排程來重組磁片區。 不過，您無法排程在位於 SSD 上的虛擬硬碟（VHD）上重組固態硬碟（SSD）或磁片區。
- 若要執行這項程序，您必須是本機電腦的 Administrators 群組成員，或者必須委派有適當的授權。 如果電腦已加入網域中，那麼只有 Domain Admins 群組成員才能執行這項程序。 作為安全性最佳作法，請考慮使用 [**執行**身分] 來執行此程式。
- 磁片區必須至少有15% 的可用空間，才能完整重組並適當**地進行碎片**整理。 **磁碟重組**會使用此空間做為檔案片段的排序區域。 如果磁片區的可用空間少於15%，**磁碟重組**只會部分重組。 若要增加磁片區上的可用空間，請刪除不需要的檔案，或將它們移至另一個磁片。
- 當**磁碟重組**正在分析並重組磁片區時，它會顯示閃爍的游標。 當重組完成分析並重組磁片區時，**它會顯示**分析報表、磁碟重組報告或這兩個報表，然後結束到命令提示字元。
- 根據預設，如果您未指定 **/a**或 **/v** **參數，重組會同時顯示分析**和磁碟重組報告的摘要。
- 您可以輸入 **>** <em>檔案名 .txt</em>，將報告傳送到文字檔，其中*FileName*是您指定的檔案名。 例如：`defrag volume /v > FileName.txt`
- 若要中斷磁碟重組程式，請在命令列中按**CTRL + C**。
- 執行**磁碟重組**命令和磁碟重組工具是互斥的。 如果您使用磁碟重組工具來重組磁片區，而您在命令列執行**磁碟重組**命令，則**磁碟重組**命令會失敗。 相反地，如果您執行**碎片**整理命令並開啟磁碟重組工具，磁碟重組工具中的重組選項將無法使用。

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要重組磁片磁碟機 C 上的磁片區，同時提供進度和詳細資訊輸出，請輸入：
```
defrag C: /U /V
```
若要在背景中以平行方式重組 C 和 D 磁片磁碟機上的磁片區，請輸入：
```
defrag C: D: /M
```
若要對裝載于 C 磁片磁碟機的磁片區執行分散分析，並提供進度，請輸入：
```
defrag C: mountpoint /A /U
```
若要重組所有具有一般優先順序的磁片區，並提供詳細資訊輸出，請輸入：
```
defrag /C /H /V
```

## <a name="scheduled-task"></a><a name=BKMK_scheduledTask></a>排定的工作
磁碟重組的排程工作會以維護工作的形式執行，而且通常會排程為每週執行一次。 系統管理員可以使用**優化磁片磁碟機**應用程式來變更頻率。
- 從排定的工作執行時，**磁碟重組**的 ssd 原則如下：
   - **傳統磁碟重組**（也就是移動檔案以使其相當連續），而**retrim**只會每個月執行一次。
   - 如果略過**傳統**重組和**retrim** ，則不會執行**分析**。
      - 如果使用者在 SSD 上手動執行**傳統的磁碟重組**，在上一次排程工作執行後的3周內，則下一個排定的工作回合將會執行**分析**和**retrim** ，但略過該 SSD 上的**傳統磁碟重組**。
   - 如果略過**分析**，則不會更新**優化磁片磁碟機**中的**上次運行**時間。  因此，對於 Ssd，**優化磁片磁碟機**中的**上次運行**時間可能是一個月的舊。
- 這項維護工作可能不會重組所有的磁片區，因為這項工作會執行下列動作：
   - 不喚醒電腦以執行磁碟重組
   - 只有在電腦使用 AC 電源時才會啟動，並在電腦切換到電池電源時停止
   - 如果電腦停止閒置，則停止

## <a name="additional-references"></a><a name=BKMK_additionalRef></a>其他參考
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil dirty](fsutil-dirty.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)
