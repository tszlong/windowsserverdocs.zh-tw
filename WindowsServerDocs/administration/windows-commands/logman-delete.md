---
title: logman delete
description: Logman delete 命令的參考文章，它會刪除現有的資料收集器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 505cd8a09bcbb1e46f16242818825f3c504955b9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930421"
---
# <a name="logman-delete"></a>logman delete

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除現有的資料收集器。

## <a name="syntax"></a>語法

```
logman delete <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| -[-] u`<user [password]>` | 指定要當做執行身分的使用者。 輸入密碼時，會 \* 產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| /? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要刪除資料收集器*perf_log*，請輸入：

```
logman delete perf_log
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)
