---
title: 清單陰影
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50e4c4b8c7ea97ec65cecb6b8e904abd8c6d98eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848769"
---
# <a name="list-shadows"></a>清單陰影



列出在系統上的持續性和現有的非持續性陰影複製。

## <a name="syntax"></a>語法

```
list shadows {all | set <SetID> | id <ShadowID>}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|全部|列出所有的陰影複製。|
|set \<SetID>|列出陰影複製屬於指定的陰影複製設定識別碼。|
|識別碼\<ShadowID >|列出任何陰影複製，並提供指定的陰影複製識別碼。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)