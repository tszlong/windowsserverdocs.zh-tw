---
title: 詳細資料的磁碟區
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7cae6dc9b82992b58c4f94801f90c0b7072492b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856919"
---
# <a name="detail-volume"></a>詳細資料的磁碟區



會顯示目前的磁碟區所在的磁碟。

## <a name="syntax"></a>語法

```
detail volume
```

## <a name="remarks"></a>備註

-   這項作業成功，就必須選取磁碟區。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。
-   磁碟區詳細資料不適用於唯讀磁碟區，例如 DVD 或 CD-ROM 光碟機。

## <a name="BKMK_examples"></a>範例

若要查看目前的磁碟區所在的所有磁碟，請輸入：
```
detail volume
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

