---
title: lodctr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e20e085e5e2cadcb0684ef57f80137a6043b1e64
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724444"
---
# <a name="lodctr"></a>lodctr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您在檔案中註冊或儲存效能計數器名稱和登錄設定，並指定信任的服務。
## <a name="syntax"></a>語法
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>參數

|    參數     |                                                                                                                                         描述                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          註冊效能計數器名稱設定，並說明初始化檔檔案名中提供的文字。                                                                                          |
|  /s<filename>   |                                                                                                       儲存效能計數器登錄設定，並說明要檔案<filename>的文字。                                                                                                       |
|        /r        |                                從目前的登錄設定以及與登錄相關的快取效能檔案，還原計數器登錄設定並解說文字。<p>此選項僅適用于 Windows Server 2003 作業系統。                                |
|  /r<filename>   | 還原效能計數器登錄設定，並說明檔案中<filename>的文字。 **警告：** 如果您使用**lodctr/r**命令，將會覆寫所有效能計數器登錄設定並解說文字，並以指定之檔案中所定義的設定來取代。 |
| /t:<servicename> |                                                                                                                       指出服務<servicename>是受信任的。                                                                                                                       |
|        /?        |                                                                                                                             在命令提示字元顯示說明。                                                                                                                             |

## <a name="remarks"></a>備註
如果您提供的資訊包含空格，請使用引號括住文字（例如， <filename>）。
## <a name="examples"></a>範例
若要將目前的效能登錄設定和計數器解說文字儲存到 file **perf 備份 1 .txt**：
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)

