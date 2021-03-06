---
title: lodctr
description: Lodctr 命令的參考文章，可讓您在檔案中註冊或儲存效能計數器名稱和登錄設定，並指定信任的服務。
ms.topic: reference
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: db458fea0a4a291c999ae88b6180b5da4c887033
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640012"
---
# <a name="lodctr"></a>lodctr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您在檔案中註冊或儲存效能計數器名稱和登錄設定，並指定信任的服務。

## <a name="syntax"></a>語法

```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<filename>` | 指定註冊效能計數器名稱設定和解說文字的初始化檔案名稱。 |
| /s`<filename>` | 指定效能計數器登錄設定和解說文字儲存的檔案名。 |
| /r | 從目前的登錄設定和與登錄相關的快取效能檔案還原計數器登錄設定和解說文字。 |
| /r`<filename>` | 指定可還原效能計數器登錄設定和解說文字的檔案名。<p>**警告：** 如果您使用此命令，將會覆寫所有效能計數器登錄設定和解說文字，並以指定的檔案中所定義的設定來取代。 |
| /t:`<servicename>` | 表示服務 `<servicename>` 是受信任的。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字 (例如「檔案名1」 ) 。

### <a name="examples"></a>範例

若要將目前的效能登錄設定和解說文字儲存到檔案 *"performance backup1.txt"*，請輸入：

```
lodctr /s:"perf backup1.txt"
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
