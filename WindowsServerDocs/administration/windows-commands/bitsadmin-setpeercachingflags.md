---
title: bitsadmin setpeercachingflags
description: 適用于**bitsadmin setpeercachingflags**的 Windows 命令主題-設定旗標，以判斷作業的檔案是否可以快取並提供給對等，以及作業是否可以從對等下載內容。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 147f28268f1b4dd6dfb40cff85f073feabbc35a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380459"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags



設定旗標，以判斷是否可以快取作業的檔案，並將其提供給對等，以及作業是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|此值為不帶正負號的整數，其在二進位標記法中具有下列對位的解讀。</br>1-作業可以從對等下載內容。</br>2-作業的檔案可以快取並提供給對等。|

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myJob*的作業設定旗標，讓它可以從對等下載內容。
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)