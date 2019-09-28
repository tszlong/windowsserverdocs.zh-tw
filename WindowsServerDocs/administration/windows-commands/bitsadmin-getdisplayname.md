---
title: bitsadmin getdisplayname
description: '**Bitsadmin getdisplayname**的 Windows 命令主題-抓取指定之作業的顯示名稱。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381635"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



抓取指定之作業的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的顯示名稱。
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)