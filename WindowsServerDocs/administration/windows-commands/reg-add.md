---
title: reg add
description: Reg add 命令的參考文章，會在登錄中加入新的子機碼或專案。
ms.topic: reference
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b34ad768cdcd324ee2b0601785dbe12b7693d557
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037086"
---
# <a name="reg-add"></a>reg add

將新的子機碼或專案新增至登錄。

## <a name="syntax"></a>語法

```
reg add <keyname> [{/v Valuename | /ve}] [/t datatype] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| /v `<Valuename>` | 指定新增登錄專案的名稱。 |
| /ve | 指定加入的登錄專案具有 null 值。 |
| 一起 `<Type>` | 指定登錄專案的類型。 *型* 別必須是下列其中一項：<ul><li>REG_SZ</li><li>REG_MULTI_SZ</li><li>REG_DWORD_BIG_ENDIAN</li><li>REG_DWORD</li><li>REG_BINARY</li><li>REG_DWORD_LITTLE_ENDIAN</li><li>REG_LINK</li><li>REG_FULL_RESOURCE_DESCRIPTOR</li><li> REG_EXPAND_SZ </li></ul> |
| /s `<Separator>` | 指定當指定了 **REG_MULTI_SZ** 資料類型，而且列出一個以上的專案時，要用來分隔多個資料實例的字元。 如果未指定，預設分隔符號為 **\ 0**。 |
| /d `<Data>` | 指定新登錄專案的資料。 |
| /f | 新增登錄專案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 無法使用此作業新增子樹。 新增子機碼時，此版本的 **reg** 不會要求確認。

- **Reg add**作業的傳回值為：

| 值 | 描述 |
|--|--|
| 0 | 成功 |
| 1 | 失敗 |

- 針對 **REG_EXPAND_SZ** 金鑰類型，使用插入號符號 ( 在 **^** **%** /d 參數內 ) 。

### <a name="examples"></a>範例

若要在遠端電腦*ABC*上新增金鑰*HKLM\Software\MyCo* ，請輸入：

```
reg add \\ABC\HKLM\Software\MyCo
```

若要將登錄專案新增至 *HKLM\Software\MyCo* ，並使用名為 *Data*的值、類型 *REG_BINARY*以及 *fe340ead*的資料，請輸入：

```
reg add HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```

若要使用名為*MRU*的值將多重值登錄專案新增至*HKLM\Software\MyCo* ，類型*REG_MULTI_SZ*以及*fax\0mail\0\0*的資料，請輸入：

```
reg add HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```

若要將擴充的登錄專案新增至 *HKLM\Software\MyCo* ，並使用名為 *Path*的值、類型 *REG_EXPAND_SZ*和 *% systemroot%* 的資料，請輸入：

```
reg add HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
