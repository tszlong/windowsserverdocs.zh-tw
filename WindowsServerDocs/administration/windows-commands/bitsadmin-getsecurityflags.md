---
title: bitsadmin getsecurityflags
description: 適用於 Windows 命令主題**bitsadmin getsecurityflags** -報告 URL 重新導向的 HTTP 安全性旗標，並在傳輸期間會檢查伺服器憑證上執行。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e1db167b12d47afccb8842da617f1e9fe72acff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434958"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

URL 重新導向和檢查報表 HTTP 安全性旗標會在傳輸期間對伺服器憑證。

## <a name="syntax"></a>語法

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例
下列範例會從名為作業擷取 securitly 旗標*myJob*。

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)


