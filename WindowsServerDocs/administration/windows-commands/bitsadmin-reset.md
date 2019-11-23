---
title: bitsadmin reset
description: '**Bitsadmin 重設**的 Windows 命令主題-取消目前使用者擁有的傳送佇列中的所有工作。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380806"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消目前使用者擁有之傳輸佇列中的所有工作。

**BITSAdmin 1.5 和更早版本**：如果您有系統管理員許可權，請 **重設** 取消佇列中的所有作業。 不支援/AllUsers 選項。

## <a name="syntax"></a>語法

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|AllUsers|選擇性-取消佇列中的所有作業。|

## <a name="remarks"></a>備註

您必須具有系統管理員許可權，才能使用**AllUsers**參數。

> [!NOTE]
> 系統管理員無法重設本機系統所建立的工作。 使用工作排程器，使用本機系統認證將此命令排定為工作。

## <a name="BKMK_examples"></a>典型

下列範例會取消目前使用者在傳送佇列中的所有工作。
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)