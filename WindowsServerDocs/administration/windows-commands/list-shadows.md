---
title: 清單陰影
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e22b0006709e1cf6636ad6c2bcc18432f59b4d1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841161"
---
# <a name="list-shadows"></a>清單陰影



列出系統上的持續性和現有的非持續陰影複製。

## <a name="syntax"></a>語法

```
list shadows {all | set <SetID> | id <ShadowID>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|全部|列出所有陰影複製。|
|設定 \<SetID >|列出屬於指定陰影複製組識別碼的陰影複製。|
|識別碼 \<ShadowID >|列出具有指定陰影複製識別碼的任何陰影複製。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)