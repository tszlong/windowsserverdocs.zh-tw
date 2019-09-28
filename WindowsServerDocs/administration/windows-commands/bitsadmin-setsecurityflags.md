---
title: bitsadmin setsecurityflags
description: 適用于**bitsadmin setsecurityflags**的 Windows 命令主題-設定適用于 HTTP 的旗標，以判斷 BITS 是否應檢查憑證撤銷清單、忽略特定憑證錯誤，以及定義伺服器重新導向 HTTP 要求時所要使用的原則。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acc5a64ef7c82b14e6815b6d51dda5ea4700dcad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380406"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags



設定 HTTP 的旗標，以判斷 BITS 是否應檢查憑證撤銷清單、忽略某些憑證錯誤，以及定義伺服器重新導向 HTTP 要求時所要使用的原則。 值為不帶正負號的整數。

## <a name="syntax"></a>語法

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|請參閱備註|

## <a name="remarks"></a>備註

**Value**參數可以包含下列一個或多個通知旗標。

|Action|二進位標記法|
|------|---------------------|
|啟用 CRL 檢查|設定最不重要的位|
|忽略伺服器憑證中不正確一般名稱|設定右邊的第二個位|
|忽略伺服器憑證中的無效日期|設定右邊的第三個位|
|忽略伺服器憑證中的無效憑證授權單位單位|從右邊設定第4個位|
|忽略不正確憑證使用方式|設定右邊的第5位|
|重新導向原則|由右邊9到11位控制</br>0，0，0-將自動允許重新導向。</br>如果發生重新導向，IBackgroundCopyFile 介面中的0，0，1-遠端名稱將會更新。</br>0，1，0位將會在發生重新導向時讓工作失敗。|
|允許從 HTTPS 重新導向至 HTTP|設定右邊的第12個位|

## <a name="BKMK_examples"></a>典型

下列範例會設定安全性旗標，以啟用名為*myJob*之作業的 CRL 檢查。
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)