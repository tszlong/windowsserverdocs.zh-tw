---
title: bitsadmin setpeercachingflags
description: 適用於 Windows 命令主題**bitsadmin setpeercachingflags** -設定旗標，決定如果作業的檔案可以快取，並提供給對等電腦，以及工作可從對等下載內容。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814249"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags



從對等設定旗標，決定如果作業的檔案可以快取，並提供給對等電腦，以及工作可以下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|以下解譯的二進位表示法中的位元的不帶正負號的整數的值。</br>1-工作可從對等下載內容。</br>2-工作檔可以快取，並提供給對等。|

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的旗標*myJob*這可讓它從對等下載內容。
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)