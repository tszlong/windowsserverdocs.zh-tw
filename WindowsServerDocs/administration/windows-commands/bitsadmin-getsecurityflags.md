---
title: bitsadmin getsecurityflags
description: 適用于**bitsadmin getsecurityflags**的 Windows 命令主題-報告 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb53664a6366b411ae1eb9b0fe7c93392d60b542
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381465"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

報告用於 URL 重新導向的 HTTP 安全性旗標，以及在傳送期間在伺服器憑證上執行的檢查。

## <a name="syntax"></a>語法

```
bitsadmin /GetSecurityFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型
下列範例會從名為*myJob*的作業中，捕獲 securitly 旗標。

```
C:\>bitsadmin /GetSecurityFlags myJob 
```

## <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)


