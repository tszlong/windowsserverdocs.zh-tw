---
title: bitsadmin reset
description: 適用於 Windows 命令主題**bitsadmin 重設**-取消目前的使用者擁有的傳輸佇列中的所有工作。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874249"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消目前的使用者擁有的傳輸佇列中的所有工作。

**1.5 和更早版本的 BITSAdmin**: 如果您有系統管理員權限， **重設** 取消佇列中的所有作業。 不支援 /AllUsers 選項。

## <a name="syntax"></a>語法

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|AllUsers|選擇性 — 取消佇列中的所有工作。|

## <a name="remarks"></a>備註

您必須擁有要使用的系統管理員權限**AllUsers**參數。

> [!NOTE]
> 系統管理員無法重設本機系統所建立的工作。 使用工作排程器來排程此命令為工作，使用本機系統認證。

## <a name="BKMK_examples"></a>範例

下列範例會取消目前使用者的傳輸佇列中的所有工作。
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)