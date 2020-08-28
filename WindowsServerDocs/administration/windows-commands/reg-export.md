---
title: reg export
description: Reg export 命令的參考文章，此命令會將指定的子機碼、專案和本機電腦的值複製到檔案中，以傳送到其他伺服器。
ms.topic: reference
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 258fde37c886335073c7eac660297e1dcce083c0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028056"
---
# <a name="reg-export"></a>reg export

將本機電腦的指定子機碼、專案和值複製到檔案中，以傳送到其他伺服器。

## <a name="syntax"></a>語法

```
reg export <keyname> <filename> [/y]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 匯出作業只適用于本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| `<filename>` | 指定作業期間要建立的檔案名和路徑。 檔案的副檔名必須是 .reg。 |
| /y | 使用名稱 *檔案名* 覆寫任何現有的檔案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 匯出**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將機碼 MyApp 的所有子機碼和值的內容匯出至檔案 AppBkUp，請輸入：

```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
