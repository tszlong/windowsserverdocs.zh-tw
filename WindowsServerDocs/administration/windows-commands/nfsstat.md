---
title: nfsstat
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94eb389fdd694c08dcd1d77325f145a81e59ea0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838881"
---
# <a name="nfsstat"></a>nfsstat



您可以使用**nfsstat**來顯示或重設對 SERVER for NFS 進行的呼叫計數。

## <a name="syntax"></a>語法

```
nfsstat [-z]
```

## <a name="description"></a>描述

當不使用 **-z**選項時， **nfsstat**命令列公用程式會顯示伺服器上的 Nfs V2、Nfs V3 和掛接 V3 呼叫數目，因為計數器設定為0（當服務啟動時），或使用**nfsstat-z**重設計數器時。