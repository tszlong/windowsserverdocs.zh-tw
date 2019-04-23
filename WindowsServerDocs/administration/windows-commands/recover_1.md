---
title: recover
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867179"
---
# <a name="recover"></a>recover



重新整理磁碟群組中的所有磁碟的狀態，嘗試復原不正確的磁碟群組中的磁碟和重新同步處理鏡像磁碟區和有過時資料的 raid-5 磁碟區。

> [!IMPORTANT]
> 無法在任何版本的 Windows Vista 中使用這個 DiskPart 命令。

## <a name="syntax"></a>語法

```
recover [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   此命令運作的磁碟群組。
-   此命令僅適用於群組的動態磁碟。 如果此命令可在基本磁碟的群組，它不會傳回錯誤，但會採取任何動作。
-   此命令運作上會失敗的磁碟或失敗。 它也會運作失敗，失敗的磁碟區上，或在失敗的備援狀態。
-   磁碟群組一部分的磁碟必須選取這個命令才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要復原磁碟群組，其中包含具有焦點的磁碟，請輸入：
```
recover
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

