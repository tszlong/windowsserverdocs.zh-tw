---
title: diskperf
description: Diskperf 的參考主題，可以用來從遠端啟用或停用執行 Windows 2000 之電腦上的實體或邏輯磁片效能計數器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8f505d924ee1de311f2f2736ff65be844c3f2ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719450"
---
# <a name="diskperf"></a>diskperf

在 Windows 2000 中，預設不會啟用實體和邏輯磁片效能計數器。

**Diskperf**包含在 windows XP、windows server 2003、windows server 2008、windows Vista、windows Server 2008 R2 和 Windows 7 中，因此可用來從遠端啟用或停用執行 Windows 2000 之電腦上的實體或邏輯磁片效能計數器。

## <a name="syntax"></a>語法

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>選項。

|選項|描述|
|------|-----------|
|-?|顯示即時線上說明。|
|-y|當電腦重新開機時，啟動所有磁片效能計數器。|
|-YD|當電腦重新開機時，啟用實體磁片磁碟機的磁片效能計數器。|
|-YV|當電腦重新開機時，為邏輯磁碟機或存放磁片區啟用磁片效能計數器。|
|-N|當電腦重新開機時，停用所有磁片效能計數器。|
|-ND|當電腦重新開機時，停用實體磁片磁碟機的磁片效能計數器。|
|-NV|當電腦重新開機時，請停用邏輯磁碟機或存放磁片區的磁片效能計數器。|
|\\\\*\<computername>*|指定您要啟用或停用磁片效能計數器的電腦名稱稱。|