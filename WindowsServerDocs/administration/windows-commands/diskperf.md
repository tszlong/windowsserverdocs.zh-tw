---
title: diskperf
description: 適用于 diskperf 的 Windows 命令主題，可用來從遠端啟用或停用執行 Windows 2000 之電腦上的實體或邏輯磁片效能計數器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b07471c051d57d0279e4fd8b38afdc4acdc4069
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845461"
---
# <a name="diskperf"></a>diskperf

在 Windows 2000 中，預設不會啟用實體和邏輯磁片效能計數器。

**Diskperf**包含在 windows XP、windows server 2003、windows server 2008、windows Vista、windows Server 2008 R2 和 Windows 7 中，因此可用來從遠端啟用或停用執行 Windows 2000 之電腦上的實體或邏輯磁片效能計數器。

## <a name="syntax"></a>語法

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>選項

|選項|描述|
|------|-----------|
|-?|顯示即時線上說明。|
|-Y|當電腦重新開機時，啟動所有磁片效能計數器。|
|-YD|當電腦重新開機時，啟用實體磁片磁碟機的磁片效能計數器。|
|-YV|當電腦重新開機時，為邏輯磁碟機或存放磁片區啟用磁片效能計數器。|
|-N|當電腦重新開機時，停用所有磁片效能計數器。|
|-ND|當電腦重新開機時，停用實體磁片磁碟機的磁片效能計數器。|
|-NV|當電腦重新開機時，請停用邏輯磁碟機或存放磁片區的磁片效能計數器。|
|\\\\ *\<computername >*|指定您要啟用或停用磁片效能計數器的電腦名稱稱。|