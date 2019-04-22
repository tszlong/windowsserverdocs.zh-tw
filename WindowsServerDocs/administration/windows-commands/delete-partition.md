---
title: 刪除磁碟分割
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf9f456ad6ab3010493154da843b2b519754e250
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816329"
---
# <a name="delete-partition"></a>刪除磁碟分割



刪除具有焦點的磁碟分割。

## <a name="syntax"></a>語法

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|覆寫|啟用 DiskPart 來刪除任何類型的磁碟分割。 通常，DiskPart 只允許您刪除已知的資料分割。|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

> [!CAUTION]
> 刪除動態磁碟上的磁碟分割，可以刪除所有的動態磁碟區的磁碟上，因此終結任何資料並將磁碟停留在損毀的狀態。 若要刪除動態磁碟區，請務必使用**刪除磁碟區**命令。 分割區可以刪除從動態磁碟，但他們不應建立。 比方說，就可以刪除在動態 GPT 磁碟上無法辨認的 GUID 磁碟分割表格 (GPT) 磁碟分割。 刪除磁碟分割不會造成可用空間結果變成可用。 此命令旨在讓您能夠在緊急情況下損毀的離線動態磁碟上的 reclame 空間所在**全新**中 DiskPart 命令無法使用。
-   您無法刪除系統磁碟分割、 開機磁碟分割或任何包含作用中分頁檔案或損毀傾印資訊的資料分割。
-   這項作業成功時，必須選取資料分割。 使用**選取資料分割**命令來選取資料分割，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要刪除具有焦點的磁碟分割，請輸入：
```
delete partition
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

