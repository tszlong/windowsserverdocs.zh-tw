---
title: lodctr
description: Lodctr 命令的參考文章，可讓您在檔案中註冊或儲存效能計數器名稱和登錄設定，並指定信任的服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8b1cae87818d3f77474e4193b03836bf1c84990
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931655"
---
# <a name="lodctr"></a>lodctr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您在檔案中註冊或儲存效能計數器名稱和登錄設定，並指定信任的服務。

## <a name="syntax"></a>語法

```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<filename>` | 指定用來註冊效能計數器名稱設定和解說文字之初始化檔的名稱。 |
| /s`<filename>` | 指定要儲存效能計數器登錄設定和解說文字的檔案名。 |
| /r | 從目前的登錄設定和與登錄相關的快取效能檔案還原計數器登錄設定和解說文字。 |
| /r`<filename>` | 指定還原效能計數器登錄設定和解說文字的檔案名。<p>**警告：** 如果您使用此命令，將會覆寫所有效能計數器登錄設定和解說文字，並以指定檔案中定義的設定來取代。 |
| /t:`<servicename>` | 指出服務 `<servicename>` 是受信任的。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字（例如，"file name 1"）。

### <a name="examples"></a>範例

若要將目前的效能登錄設定和解說文字儲存到 file *perf backup1.txt*，請輸入：

```
lodctr /s:perf backup1.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
