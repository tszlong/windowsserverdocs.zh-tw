---
title: bitsadmin setsecurityflags
description: 適用於 Windows 命令主題**bitsadmin setsecurityflags** -集加上旗標適用於判斷位元應該檢查憑證撤銷清單，忽略特定的憑證錯誤，以及定義的原則時要使用的 HTTP 伺服器HTTP 要求重新導向。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858619"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags



判斷位元應該檢查憑證撤銷清單，忽略特定的憑證錯誤，以及定義原則，以在伺服器重新導向的 HTTP 要求時使用的 http 設定旗標。 值是不帶正負號的整數。

## <a name="syntax"></a>語法

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|請參閱 < 備註 >|

## <a name="remarks"></a>備註

**值**參數可以包含一或多個下列的通知旗標。

|動作|二進位表示法|
|------|---------------------|
|啟用 CRL 檢查|設定最小顯著性的位元|
|忽略無效一般名稱，在 伺服器憑證|設定從右邊的第 2 位元|
|忽略無效的日期，在 伺服器憑證|設定從右邊的第 3 位元|
|忽略無效的憑證授權單位，在 伺服器憑證|設定從右邊的第 4 個位元|
|忽略憑證的使用方式無效|設定從右邊的第 5 位元|
|重新導向原則|控制從右邊的 9 日至 11 個位元</br>0,0,0-重新導向，將會自動允許。</br>0,0,1-IBackgroundCopyFile 介面中的遠端名稱將會更新重新導向發生。</br>0,1,0-位元為單位將會失敗的作業重新導向發生。|
|允許從 HTTPS 的重新導向至 HTTP|從右邊將 12 位元|

## <a name="BKMK_examples"></a>範例

下列範例會設定以啟用 CRL 檢查的名為作業的安全性旗標*myJob*。
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)