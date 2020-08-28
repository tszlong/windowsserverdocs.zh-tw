---
title: reg unload
description: Reg unload 命令的參考文章，此命令會移除使用 reg 載入作業載入的登錄區段。
ms.topic: reference
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4f0ba24fd2d6dead3ff4f224584b56adbfd8425
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024992"
---
# <a name="reg-unload"></a>reg unload

移除使用 **reg 載入** 作業載入的登錄區段。

## <a name="syntax"></a>語法

```
reg unload <keyname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg unload**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

## <a name="examples"></a>範例

若要卸載 file HKLM 中的 hive TempHive，請輸入：

```
reg unload HKLM\TempHive
```

> [!CAUTION]
> 除非您沒有替代方案，否則請勿直接編輯登錄。 登錄編輯程式會略過標準的保護，允許可能降低效能、損毀系統或甚至需要重新安裝 Windows 的設定。 您可以使用主控台或 Microsoft Management Console (MMC) 中的程式，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg load 命令](reg-load.md)
