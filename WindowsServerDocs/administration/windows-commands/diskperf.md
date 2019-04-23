---
title: diskperf
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852579"
---
# <a name="diskperf"></a>diskperf



在 Windows 2000 中，預設不會啟用實體和邏輯磁碟效能計數器。

**Diskperf**包含在 Windows XP、 Windows Server 2003、 Windows Server 2008、 Windows Vista、 Windows Server 2008 R2 和 Windows 7 中，以便它可以用來從遠端啟用或停用在執行電腦上的實體或邏輯磁碟效能計數器Windows 2000。

## <a name="syntax"></a>語法

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>選項。

|選項|描述|
|------|-----------|
|-?|顯示即時線上說明。|
|-Y|當電腦重新啟動時，請啟動所有的磁碟效能計數器。|
|-YD|當電腦重新啟動時，請啟用實體磁碟機的磁碟效能計數器。|
|-YV|當電腦重新啟動時，請啟用邏輯磁碟機或儲存體磁碟區的磁碟效能計數器。|
|-N|當電腦重新啟動，請停用所有的磁碟效能計數器。|
|-ND|當電腦重新啟動，請停用實體磁碟機的磁碟效能計數器。|
|-NV|當電腦重新啟動，請停用邏輯磁碟機或儲存體磁碟區的磁碟效能計數器。|
|\\\\*\<computername>*|指定您要啟用或停用磁碟的效能計數器的電腦名稱。|