---
title: replace
description: 了解如何使用 replace 命令取代的檔案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 293534a2287fe0219643dacc88926018c37dbdcc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441773"
---
# <a name="replace"></a>replace



取代檔案。 如果搭配 **/a**選項時，**取代**將新檔案新增至目錄，而不是取代現有的檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[\<Drive1>:][\<Path1>]\<FileName>|指定的位置和原始程式檔名稱或一組檔案。 *檔名*為必要項，並可包含萬用字元 ( **&#42;** 並 **？** )。|
|[\<Drive2>:][\<Path2>]|指定目的地檔案的位置。 您無法指定所取代的檔案的檔案名稱。 如果您未指定磁碟機或路徑，**取代**做為目的地會使用目前的磁碟機和目錄。|
|/a|您可以將檔案新增到目的地目錄，而不是取代現有的檔案。 您無法使用這個命令列選項搭配 **/s**或是 **/u**命令列選項。|
|/p|會提示您確認再取代目的地檔案或新增原始程式檔。|
|/r|取代為唯讀，且未受保護的檔案。 如果您嘗試更換唯讀檔案，但您未指定 **/r**，會產生錯誤，並停止取代作業。|
|/w|等到您原始程式檔搜尋開始之前，請插入磁片。 如果您未指定 **/w**，**取代**只有在您按下 ENTER 之後，立即開始取代或新增檔案。|
|/s|搜尋所有子目錄中的目的地目錄，並取代相符的檔案。 您無法使用 **/s**具有 **/a**命令列選項。 **取代**命令不會搜尋中所指定的子目錄*路徑 1*。|
|/u|取代檔案的目的地目錄上早於來源目錄中。 您無法使用 **/u**具有 **/a**命令列選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 作為**取代**新增或取代檔案名稱會顯示在螢幕的檔案。 在後**取代**完成時，摘要列會顯示下列格式之一：  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- 如果您使用磁碟，而且您需要切換期間的磁碟**取代**作業，您可以指定 **/w**命令列選項，讓**取代**會等待您切換磁碟。
- 您無法使用**取代**更新隱藏的檔案或系統檔案。
- 下表顯示每個結束代碼和其意義的簡短描述：  
  |結束代碼|描述|
  |---------|-----------|
  |0|**取代**命令已成功取代或新增的檔案。|
  |1|**取代**命令遇到的 MS-DOS 不正確的版本。|
  |2|**取代**命令找不到原始程式檔。|
  |3|**取代**命令找不到來源或目的地路徑。|
  |5|您想要取代的檔案，使用者並沒有存取權。|
  |8|沒有足夠的系統記憶體來執行命令。|
  |11|使用者在命令列中使用錯誤的語法。|

> [!NOTE]
> 您可以使用 ERRORLEVEL 參數上**如果**命令列中的批次程式，以處理序所傳回的結束代碼**取代**。

## <a name="BKMK_examples"></a>範例

若要更新的檔案，phones.cli （這會顯示 C 磁碟機上的多個目錄中），從磁碟機 A 中的磁碟片 Phones.cli 檔案的最新版本的所有版本中，輸入：

`replace a:\phones.cli c:\ /s`

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)