---
title: bitsadmin monitor
description: '**Bitsadmin 監視**的 Windows 命令主題-監視目前使用者擁有的傳送佇列中的作業。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381216"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



監視目前使用者擁有的傳送佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Allusers|選擇性-監視所有使用者的作業。|
|重新整理|選擇性-以*秒數*指定的間隔重新整理資料。 預設重新整理間隔為5秒。|

## <a name="remarks"></a>備註

您必須具有系統管理員許可權，才能使用**Allusers**參數。

使用 CTRL + C 停止重新整理。

## <a name="BKMK_examples"></a>典型

下列範例會監視目前使用者所擁有之作業的傳送佇列，並每隔60秒重新整理一次資訊。
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)