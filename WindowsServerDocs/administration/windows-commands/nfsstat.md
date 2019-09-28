---
title: nfsstat
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373185"
---
# <a name="nfsstat"></a>nfsstat



您可以使用**nfsstat**來顯示或重設對 SERVER for NFS 進行的呼叫計數。

## <a name="syntax"></a>語法

```
nfsstat [-z]
```

## <a name="description"></a>描述

當不使用 **-z**選項時， **nfsstat**命令列公用程式會顯示伺服器的 Nfs V2、Nfs V3 和掛接 V3 呼叫數目，因為計數器已設定為0（可能是在啟動服務時，或是使用來**重設計數器時）nfsstat-z**。