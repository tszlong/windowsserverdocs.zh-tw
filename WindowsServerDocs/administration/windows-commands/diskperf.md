---
title: diskperf
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377806"
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