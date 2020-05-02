---
title: logman
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9aed5a83c503c03f52757abf525aa5d122f41466
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724277"
---
# <a name="logman"></a>logman

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**logman**會建立和管理事件追蹤會話和效能記錄檔，並從命令列支援效能監視器的許多功能。
## <a name="syntax"></a>語法
```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```
## <a name="actions"></a>動作
|動作|描述|
|-----|--------|
|[logman create](logman-create.md)|建立計數器、追蹤、設定資料收集器或 API。|
|[logman query](logman-query.md)|查詢資料收集器屬性。|
|[logman start &#124; stop](logman-start-stop.md)|開始或停止資料收集。|
|[logman delete](logman-delete.md)|刪除現有的資料收集器。|
|[logman update](logman-update.md)|更新現有資料收集器的屬性。|
|[logman import &#124; export](logman-import-export.md)|從 XML 檔案匯入資料收集器集合檔，或將資料收集器集合匯出至 XML 檔案。|
