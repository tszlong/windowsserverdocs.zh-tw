---
title: nfsstat
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853289"
---
# <a name="nfsstat"></a>nfsstat



您可以使用**nfsstat**顯示或重設的 nfs 伺服器所發出的呼叫計數。

## <a name="syntax"></a>語法

```
nfsstat [-z]
```

## <a name="description"></a>描述

未搭配使用時**a-z**選項時， **nfsstat**命令列公用程式會顯示 NFS V2、 NFS V3 和掛接 V3 呼叫計數器設定為 0，因為對伺服器的數目時，服務啟動或使用重設計數器**nfsstat z**。