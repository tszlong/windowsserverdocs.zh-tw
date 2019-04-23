---
title: bitsadmin list
description: 適用於 Windows 命令主題**bitsadmin 清單**-列出目前使用者所擁有的傳送工作。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873859"
---
# <a name="bitsadmin-list"></a>bitsadmin list



列出目前使用者所擁有的傳送工作。

## <a name="syntax"></a>語法

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/Allusers|選擇性： 列出所有使用者的工作|
|/Verbose|選擇性： 提供針對每個工作的詳細的資訊。|

## <a name="remarks"></a>備註

您必須擁有系統管理員權限，才能使用 /allusers 參數

## <a name="BKMK_examples"></a>範例

下列範例會擷取目前使用者所擁有的作業的相關資訊。
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)