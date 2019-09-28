---
title: bitsadmin cache 和 setexpirationtime
description: '**Bitsadmin cache 和 setexpirationtime**的 Windows 命令主題-設定快取到期時間。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381916"
---
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache 和 setexpirationtime
設定快取到期時間。
## <a name="syntax"></a>語法
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|秒鐘|快取到期之前的秒數。|
## <a name="BKMK_examples"></a>典型
下列範例會在60秒內過期快取。
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)
