---
title: reg unload
description: Reg unload 命令的參考文章，其會移除使用 reg 載入作業載入的登錄區段。
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d2ea6f5981ea613ae5e0d9d4dcae155464b505a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883986"
---
# <a name="reg-unload"></a>reg unload

移除使用**reg 載入**作業載入的登錄區段。

## <a name="syntax"></a>語法

```
reg unload <keyname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱的格式加上 `\\<computername>\`) 作為*keyname*的一部分 (。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg unload**作業的傳回值如下：

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
> 請勿直接編輯登錄，除非您沒有替代方案。 登錄編輯程式會略過標準保護，允許降低效能、損毀您的系統，甚至要求您重新安裝 Windows 的設定。 您可以使用 [控制台] 中的程式或 Microsoft Management Console (MMC) ，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg load 命令](reg-load.md)
