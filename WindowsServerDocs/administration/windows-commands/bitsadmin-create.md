---
title: bitsadmin create
description: '**Bitsadmin create**的 Windows 命令主題-使用指定的顯示名稱建立傳送工作。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381815"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立具有指定之顯示名稱的傳送工作。 下載作業會將資料從伺服器傳送到本機檔案。 上傳作業會將資料從本機檔案傳送到伺服器。 上傳-回復作業會將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔案。

使用[bitsadmin resume](bitsadmin-resume.md)參數來啟動傳送佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|型別|-    **/Download**會將資料從伺服器傳送到本機檔案。<br />-    **/Upload**會將資料從本機檔案傳送到伺服器。<br />-    **/Upload-Reply**會將資料從本機檔案傳送到伺服器，並從伺服器接收回複檔案。<br />-在命令列上未指定時，此參數的預設值為 **/Download** 。|
|DisplayName|指派給新建立之作業的顯示名稱。|

**BITS 1.2 和更早版本**： 無法使用/Upload 和/Upload-Reply 類型。

## <a name="BKMK_examples"></a>典型

建立名為*myDownloadJob*的下載作業。

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他參考

[命令列語法關鍵](command-line-syntax-key.md)
