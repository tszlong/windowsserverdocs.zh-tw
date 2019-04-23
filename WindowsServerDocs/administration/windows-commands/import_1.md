---
title: 匯入
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379d5923a9355db2965b56c27cedd207b1b63006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885989"
---
# <a name="import"></a>匯入



將外部磁碟群組匯入本機電腦的磁碟群組。

## <a name="syntax"></a>語法

```
import [noerr]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|

## <a name="remarks"></a>備註

-   匯入命令會匯入具有焦點的磁碟位於相同群組中每個磁碟。
-   這項作業成功時，必須選取外部磁碟群組中的動態磁碟。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

若要匯入到本機電腦的磁碟群組焦點的磁碟相同的磁碟群組中每個磁碟，請輸入：
```
import
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

