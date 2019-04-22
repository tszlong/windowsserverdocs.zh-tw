---
title: bitsadmin 對等快取和 getconfigurationflags
description: 適用於 Windows 命令主題**bitsadmin 對等快取和 getconfigurationflags** -取得判斷是否電腦就會提供內容給對等的組態旗標，並可從對等下載內容。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826349"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin 對等快取和 getconfigurationflags



取得判斷電腦是否提供內容給對等，以及可從對等下載內容的組態旗標。

## <a name="syntax"></a>語法

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會取得名為作業的組態旗標*myJob*。
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)