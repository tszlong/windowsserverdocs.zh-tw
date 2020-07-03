---
title: reg add
description: Reg add 命令的參考文章，它會將新的子機碼或專案新增至登錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db968e8fb55a4de73f5221f8149f794600f6884e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933514"
---
# <a name="reg-add"></a>reg add

將新的子機碼或專案新增至登錄。

## <a name="syntax"></a>語法

```
reg add <keyname> [{/v Valuename | /ve}] [/t datatype] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<keyname>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱（格式為）包含在 `\\<computername>\` *keyname*中。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| 停`<Valuename>` | 指定加入登錄專案的名稱。 |
| /ve | 指定加入的登錄專案具有 null 值。 |
| 一起`<Type>` | 指定登錄專案的類型。 *類型*必須是下列其中一項：<ul><li>REG_SZ</li><li>REG_MULTI_SZ</li><li>REG_DWORD_BIG_ENDIAN</li><li>REG_DWORD</li><li>REG_BINARY</li><li>REG_DWORD_LITTLE_ENDIAN</li><li>REG_LINK</li><li>REG_FULL_RESOURCE_DESCRIPTOR</li><li> REG_EXPAND_SZ </li></ul> |
| /s`<Separator>` | 指定當指定了**REG_MULTI_SZ**資料類型，且列出一個以上的專案時，要用來分隔多個資料實例的字元。 如果未指定，預設分隔符號為**\ 0**。 |
| /d`<Data>` | 指定新登錄專案的資料。 |
| /f | 新增登錄專案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 無法加入此作業的子樹。 這個版本的**reg**不會在新增子機碼時要求確認。

- **Reg add**作業的傳回值如下：

| 值 | 說明 |
|--|--|
| 0 | 成功 |
| 1 | 失敗 |

- 針對**REG_EXPAND_SZ**金鑰類型，請在 **^** /d 參數內部使用插入號（） **%** 。

### <a name="examples"></a>範例

若要在遠端電腦*ABC*上新增金鑰*HKLM\Software\MyCo* ，請輸入：

```
reg add \\ABC\HKLM\Software\MyCo
```

若要使用名為*Data*的值將登錄專案新增至*HKLM\Software\MyCo* ，類型*REG_BINARY*和*fe340ead*的資料，請輸入：

```
reg add HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```

若要使用名為*MRU*的值將多重值登錄專案新增至*HKLM\Software\MyCo* ，類型*REG_MULTI_SZ*和*fax\0mail\0\0*的資料，請輸入：

```
reg add HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```

若要使用名為*Path*的值將擴充的登錄專案新增至*HKLM\Software\MyCo* ，類型*REG_EXPAND_SZ*和 *% systemroot%* 的資料，請輸入：

```
reg add HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
