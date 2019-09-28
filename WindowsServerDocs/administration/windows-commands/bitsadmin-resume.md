---
title: bitsadmin resume
description: '**Bitsadmin resume**的 Windows 命令主題-在傳送佇列中啟用新的或已暫止的工作。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380766"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



在傳送佇列中啟用新的或已暫止的工作。

## <a name="syntax"></a>語法

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會繼續名為*myDownloadJob*的作業。
```
C:\>bitsadmin /Resume myDownloadJob
```
其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)