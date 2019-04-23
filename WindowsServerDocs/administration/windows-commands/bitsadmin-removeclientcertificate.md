---
title: bitsadmin removeclientcertificate
description: 適用於 Windows 命令主題**bitsadmin removeclientcertificate** -從作業中移除的用戶端憑證。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b720800fe80037f38ff01ac3a90d5cbb41a6ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868659"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate



從作業中移除的用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例從名為作業中移除的用戶端憑證*myJob*。
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)