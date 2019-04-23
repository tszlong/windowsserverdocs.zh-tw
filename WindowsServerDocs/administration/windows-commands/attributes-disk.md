---
title: 磁碟屬性
description: 適用於 Windows 命令主題**屬性磁碟**-顯示、 集合或清除磁碟的屬性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890349"
---
# <a name="attributes-disk"></a>磁碟屬性



顯示、 設定，或清除磁碟的屬性。

> [!IMPORTANT]
> 此參數不適用於所有版本的 Windows Vista。

## <a name="syntax"></a>語法

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|設定|設定具有焦點的磁碟的指定的屬性。|
|clear|清除指定的屬性具有焦點的磁碟。|
|唯讀|指定磁碟處於唯讀狀態。|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   當**屬性磁碟**是用來顯示磁碟的目前屬性，啟動磁碟屬性表示用來啟動電腦的磁碟。 動態鏡像，它會顯示包含的開機磁碟區網狀開機磁碟區的磁碟。
-   必須選取一個磁碟**屬性磁碟**命令才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要檢視所選磁碟的屬性，請輸入：
```
attributes disk
```
若要將所選的磁碟設為唯讀模式中，輸入：
```
attributes disk set readonly
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

