---
title: replace
description: 瞭解如何使用 replace 命令來取代檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d44e4f8383a77582177f4d9b161210207ce46e63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835901"
---
# <a name="replace"></a>replace



取代檔案。 如果搭配使用 **/a**選項， **replace**會將新檔案新增至目錄，而不是取代現有的檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive1 >：][\<Path1 >]\<FileName >|指定來源檔案或檔案集的位置和名稱。 *FileName*是必要的，而且可以包含萬用字元（ **&#42;** 和 **？** ）。|
|[\<Drive2 >：][\<Path2 >]|指定目的檔案的位置。 您不能指定所取代檔案的檔案名。 如果您未指定磁片磁碟機或路徑， **replace**會使用目前的磁片磁碟機和目錄做為目的地。|
|/a|將新檔案新增至目的地目錄，而不是取代現有的檔案。 您不能將此命令列選項與 **/s**或 **/u**命令列選項搭配使用。|
|/p|在取代目的地檔案或新增原始程式檔之前，先提示您確認。|
|/r|取代唯讀和未受保護的檔案。 如果您嘗試取代唯讀檔案，但未指定 **/r**，則會產生錯誤，並停止取代作業。|
|/w|等待您插入磁片，然後才開始搜尋原始程式檔。 如果您未指定 **/w**， **replace**會在您按下 enter 之後立即開始取代或新增檔案。|
|/s|搜尋目的地目錄中的所有子目錄，並取代相符的檔案。 您不能使用 **/s**搭配 **/a**命令列選項。 **Replace**命令不會搜尋*Path1*中指定的子目錄。|
|/u|只會取代目的地目錄中的檔案，而這些檔案比來原始目錄中的檔案還舊。 您不能搭配使用 **/u**與 **/a**命令列選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- As **replace**新增或取代檔案時，檔案名會顯示在畫面上。 **Replace**完成後，會顯示下列其中一種格式的摘要行：  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- 如果您使用的是軟碟，而且需要在**更換**作業期間切換磁片，您可以指定 **/w**命令列選項，讓**replace**會等待您切換磁片。
- 您不能使用**replace**來更新隱藏的檔案或系統檔案。
- 下表顯示每個結束代碼和其意義的簡短描述：  
  |結束代碼|描述|
  |---------|-----------|
  |0|**Replace**命令已成功取代或新增檔案。|
  |1|**Replace**命令遇到不正確的 MS-DOS 版本。|
  |2|**Replace**命令找不到來源檔案。|
  |3|**Replace**命令找不到來源或目的地路徑。|
  |5|使用者沒有您想要取代之檔案的存取權。|
  |8|系統記憶體不足，無法執行命令。|
  |11|使用者在命令列上使用了錯誤的語法。|

> [!NOTE]
> 您可以在 batch 程式中的**if**命令列上使用 ERRORLEVEL 參數，以處理**replace**所傳回的結束代碼。

## <a name="examples"></a><a name="BKMK_examples"></a>典型

若要更新名為 phone 的檔案的所有版本（出現在 C 磁片磁碟機上的多個目錄中），請從磁片磁碟機 A 中的軟碟使用最新版本的 phone. cli 檔案，輸入：

`replace a:\phones.cli c:\ /s`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)