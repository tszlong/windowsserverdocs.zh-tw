---
title: 設定內容
description: Set coNtext 命令的參考文章，此命令會設定用於建立陰影複製的內容。
ms.topic: reference
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ce1414ab90f116f58618fcd67bc203f25dc58e46
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389092"
---
# <a name="set-context"></a>設定內容

設定用於建立陰影複製的內容。 如果使用時不含參數，請在命令提示字元中 **設定 coNtext** 顯示說明。

## <a name="syntax"></a>語法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| clientaccessible | 指定陰影複製可供用戶端版本的 Windows 使用。 此內容預設為持續性。 |
| 持續 | 指定在程式結束、重設或重新開機期間保存陰影複製。 |
| volatile | 在結束或重設時刪除陰影複製。 |
| nowriters | 指定排除所有的寫入器。 |

## <a name="examples"></a>範例

若要防止在您結束 DiskShadow 時刪除陰影複製，請輸入：

```
set context persistent
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [設定中繼資料命令](set-metadata.md)

- [set option 命令](set-option.md)

- [設定詳細資訊命令](set-verbose.md)
