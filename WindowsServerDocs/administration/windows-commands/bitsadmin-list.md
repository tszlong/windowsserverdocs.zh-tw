---
title: bitsadmin list
description: '**Bitsadmin 清單**的 Windows 命令主題-列出目前使用者擁有的傳送作業。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381104"
---
# <a name="bitsadmin-list"></a>bitsadmin list



列出目前使用者擁有的傳送作業。

## <a name="syntax"></a>語法

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/Allusers|選擇性：列出所有使用者的作業|
|/Verbose|選擇性-提供每項作業的詳細資訊。|

## <a name="remarks"></a>備註

您必須具有系統管理員許可權，才能使用/allusers 參數

## <a name="BKMK_examples"></a>典型

下列範例會抓取目前使用者所擁有之作業的相關資訊。
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)