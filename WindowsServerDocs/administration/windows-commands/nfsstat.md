---
title: nfsstat
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e2c02fdfeb9923993a1d4471862a6c8487c9d86
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723758"
---
# <a name="nfsstat"></a>nfsstat



您可以使用**nfsstat**來顯示或重設對 SERVER for NFS 進行的呼叫計數。

## <a name="syntax"></a>語法

```
nfsstat [-z]
```

## <a name="description"></a>描述

當不使用 **-z**選項時， **nfsstat**命令列公用程式會顯示伺服器上的 Nfs V2、Nfs V3 和掛接 V3 呼叫數目，因為計數器設定為0（當服務啟動時），或使用**nfsstat-z**重設計數器時。