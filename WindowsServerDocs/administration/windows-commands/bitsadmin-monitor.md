---
title: bitsadmin monitor
description: 適用於 Windows 命令主題**bitsadmin 監視器**-監視目前使用者擁有的傳送佇列中的作業。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814589"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



在目前的使用者擁有的傳輸佇列中的監視作業。

## <a name="syntax"></a>語法

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Allusers|選擇性： 監視的所有使用者的工作。|
|重新整理|選擇性： 所指定的間隔重新整理資料*秒*。 預設重新整理間隔為五秒。|

## <a name="remarks"></a>備註

您必須擁有要使用的系統管理員權限**Allusers**參數。

您可以使用 CTRL + C 來停止重新整理。

## <a name="BKMK_examples"></a>範例

下列範例會監視目前的使用者所擁有的作業的傳輸佇列，並重新整理資訊每隔 60 秒。
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)