---
title: reg query
description: Reg query 命令的參考文章，此命令會傳回下一層的子機碼，以及位於登錄中指定之子機碼底下的專案清單。
ms.topic: reference
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d004c42e4561cd96502e8dedc9ee472df964b5b8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639870"
---
# <a name="reg-query"></a>reg query

傳回登錄中指定的子機碼下的下一層子機碼和專案清單。

## <a name="syntax"></a>語法

```
reg query <keyname> [{/v <Valuename> | /ve}] [/s] [/se <separator>] [/f <data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| /v `<Valuename>` | 指定要查詢的登錄值名稱。 如果省略，則會傳回 *keyname* 的所有值名稱。 如果也使用 **/f**選項，此參數的*Valuename*是選擇性的。 |
| /ve | 針對空白的值名稱執行查詢。 |
| /s | 指定以遞迴方式查詢所有子機碼和值名稱。 |
| /se `<separator>` | 指定要在值名稱類型 **REG_MULTI_SZ**中搜尋的單一值分隔符號。 如果未指定 *分隔符號* ，則會使用 **\ 0** 。 |
| /f `<data>` | 指定要搜尋的資料或模式。 如果字串包含空格，請使用雙引號。 如果未指定，會使用萬用字元 (**&#42;**) 做為搜尋模式。 |
| /k | 指定只在索引鍵名稱中搜尋。 |
| /d | 指定只在資料中搜尋。 |
| /C | 指定查詢區分大小寫。 依預設，查詢不會區分大小寫。 |
| /e | 指定只傳回完全相符的專案。 預設會傳回所有符合專案。 |
| 一起 `<Type>` | 指定要搜尋的登錄類型。 有效的類型為： **REG_SZ**、 **REG_MULTI_SZ**、 **REG_EXPAND_SZ**、 **REG_DWORD**、 **REG_BINARY**、 **REG_NONE**。 如果未指定，則會搜尋所有類型。 |
| /z | 指定在搜尋結果中包含登錄類型的對等數值。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 查詢**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要在 HKLM\Software\Microsoft\ResKit 索引鍵中顯示名稱值版本的值，請輸入：

```
reg query HKLM\Software\Microsoft\ResKit /v Version
```

若要在名為 ABC 的遠端電腦上，顯示金鑰 HKLM\Software\Microsoft\ResKit\Nt\Setup 下的所有子機碼和值，請輸入：

```
reg query \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```

若要顯示 REG_MULTI_SZ 使用做為分隔符號之類型的所有子機碼和值 **#** ，請輸入：

```
reg query HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```

若要顯示索引鍵、值和資料，以取得資料類型 REG_SZ 之 HKLM 根目錄下的精確和區分大小寫的相符專案，請輸入：

```
reg query HKLM /f SYSTEM /t REG_SZ /c /e
```

若要顯示與資料類型 REG_BINARY 之 HKCU 根金鑰下資料中的 **0F** 相符的索引鍵、值和資料，請輸入：

```
reg query HKCU /f 0F /d /t REG_BINARY
```

若要顯示 null 值的值和資料 (預設) 在 HKLM\SOFTWARE 下，請輸入：

```
reg query HKLM\SOFTWARE /ve
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
