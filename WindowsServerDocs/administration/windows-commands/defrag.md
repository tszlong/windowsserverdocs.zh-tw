---
title: defrag
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813189"
---
# <a name="defrag"></a>defrag

>適用於：Windows 10，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

找出並合併分散的檔案，在本機的磁碟區，以改善系統效能。
在本機的成員資格**系統管理員**群組或對等項目，是執行此命令所需的最小值。

## <a name="syntax"></a>語法
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|`<volume>`|指定要重組或分析的磁碟區的磁碟機代號或掛接點路徑。|
|A|在指定的磁碟區上執行分析。|
|C|執行所有的磁碟區上的作業。|
|D|執行傳統的磁碟重組 （這是預設值）。 階層式磁碟區上，傳統的磁碟重組會對只容量層。|
|E|執行指定的所有磁碟區上的作業。|
|G|最佳化在指定的磁碟區上的儲存層。|
|H|以一般優先權執行的作業 （預設值為低）。|
|我 n|儲存層最佳化會在每個磁碟區上執行最多 n 秒。|
|K|在指定的磁碟區上執行 slab 彙總。|
|L|在指定的磁碟區上執行 retrim。|
|M [n]|在背景中平行執行每個磁碟區上的作業。 最多 n 執行緒最佳化以平行方式儲存層。|
|O|執行適當的最佳化，針對每個媒體類型。|
|T|在指定的磁碟區上追蹤已在進行中的作業。|
|U|列印在螢幕上作業的進度。|
|V|列印包含片段統計資料的詳細資訊輸出。|
|X|執行指定的磁碟區上的可用空間彙總。|
|?|顯示這個說明資訊。|

## <a name="remarks"></a>備註
-   您不能重組特定類型的檔案系統磁碟區或磁碟機：
    -   您不能重組已鎖定的檔案系統的磁碟區。
    -   您不能重組磁碟區檔案系統已標示為 「 中途 」，這表示可能已損毀。 您必須執行**chkdsk**之前可以進行磁碟重組已變更磁碟區上。 您可以判斷磁碟區是否已變更，使用**fsutil**中途查詢命令。 如需詳細資訊**chkdsk**並**fsutil**已變更，請參閱[其他參考](defrag.md#BKMK_additionalRef)。
    -   您無法重組的網路磁碟機。
    -   您不能重組 cdROMs。
    -   您無法進行重組檔案系統磁碟區不是**NTFS**， **ReFS**， **Fat**或是**Fat32**。
-   Windows server 2008 R2 中，Windows Server 2008 和 Windows Vista 中，您可以排定重組磁碟區。 不過，您無法排程重組固態硬碟 (SSD) 或磁碟區上虛擬硬碟 (VHD) 存放在 SSD 上。
-   若要執行此程序，您必須是本機電腦上的 Administrators 群組成員或是已經委派您適當的權限。 如果該電腦已加入網域，則 Domain Admins 群組的成員便可執行此程序。 基於安全性最佳做法，請考慮使用**執行身分**執行此程序。
-   磁碟區必須至少 15%的可用空間**重組**完整且適當地重組。 **重組**成一個排序的區域會使用此空間檔案片段。 如果磁碟區有可用空間，低於 15%**重組**會只有部分進行磁碟重組。 若要增加磁碟區上的可用空間，刪除不必要的檔案，或將它們移至另一個磁碟。
-   雖然**重組**是分析和重組磁碟區，它會顯示閃爍的游標。 當**重組**已完成的分析，重組磁碟區，它會顯示 [分析] 報表，磁碟重組報告或這兩份報告，然後結束命令提示字元。
-   根據預設，**重組**顯示分析 」 和 「 磁碟重組報告的摘要，如果您未指定 **/a**或是 **/v**參數。
-   您可以將報表傳送給文字檔案輸入 **>** *FileName.txt*，其中*FileName.txt*是您指定的檔案名稱。 例如：`defrag volume /v > FileName.txt`
-   若要中斷的磁碟重組程序，在命令列，請按**CTRL + C**。
-   執行**重組**命令和磁碟重組工具會互斥。 如果您使用磁碟重組工具重組磁碟區，而且您執行**重組**命令的命令列**重組**命令就會失敗。 相反地，如果您執行**重組**命令和開啟磁碟重組工具，磁碟重組工具 中的磁碟重組選項都無法使用。

## <a name="BKMK_examples"></a>範例
若要重組 C 磁碟機上的磁碟區，同時提供進度和詳細資訊輸出，請輸入：
```
defrag C: /U /V
```
若要重組磁碟機 C 和 D 以平行方式在背景上的磁碟區，請輸入：
```
defrag C: D: /M
```
若要執行 C 磁碟機上掛接的磁碟區的分散程度分析，並提供進度，請輸入：
```
defrag C: mountpoint /A /U
```
若要以一般優先順序重組所有的磁碟區，並提供詳細資訊輸出，輸入：
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>排定的工作
重組的排程工作會執行維護工作，通常排程為每週執行。 系統管理員可以變更的頻率 using**最佳化磁碟機**應用程式。
- 當執行排定的工作，從**重組**用於 Ssd 具有以下原則：
   - **繁體重組**（也就移動檔案，使其更合理地連續） 和**retrim**執行一次每個月。
   - 如果兩個**傳統重組**並**retrim**會略過**analysis**就不會執行。
      - 如果使用者已執行**傳統重組**手動於 SSD 上，輸入排程的最後一個之後的 3 週工作執行，然後再執行下一個排定的工作會執行**analysis**並**retrim**但略過**傳統重組**該 SSD 上。
   - 如果**analysis**會略過**上次執行**時間**最佳化磁碟機**將不會更新。  因此，針對 Ssd**上次執行**時間**最佳化磁碟機**可以是一個月。
- 這項維護工作可能會不重組所有磁碟區，有時候因為這項工作會執行下列作業：
   - 無法喚醒電腦，才能執行磁碟重組
   - 電腦使用 AC 電源時，並停止當電腦切換到電池電源時，才會在啟動
   - 如果電腦停止閒置即停止，就會停止

## <a name="BKMK_additionalRef"></a>其他參考資料
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil 已變更](fsutil-dirty.md)
-   [命令列語法關鍵](command-line-syntax-key.md)
