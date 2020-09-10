---
title: logman start and logman stop
description: Logman start 和 logman stop 命令的參考文章，可啟動資料收集器並將開始時間設定為手動，或停止資料收集器集合，並將結束時間設為手動。
ms.topic: reference
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9a684eb010d52e5aba01fee609f878cc5485a7d5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639968"
---
# <a name="logman-start-and-logman-stop"></a>logman start and logman stop

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**Logman start**命令會啟動資料收集器，並將開始時間設定為手動。 **Logman stop**命令會停止資料收集器集合，並將結束時間設為手動。

## <a name="syntax"></a>語法

```
logman start <[-n] <name>> [options]
logman stop <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| [-n] `<name>` | 指定目標物件的名稱。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| -as | 以非同步方式執行要求的作業。 |
| -? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要啟動資料收集器 *perf_log*，請在 [遠端電腦] *server_1*上，輸入：

```
logman start perf_log -s server_1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)
