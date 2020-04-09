---
title: bitsadmin setpeercachingflags
description: 適用于 bitsadmin setpeercachingflags 的 Windows 命令主題，其會設定旗標來判斷作業的檔案是否可以快取並提供給對等，以及作業是否可以從對等下載內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d19e4d14b47e4aa96e9ad9d4367e872350ad4d43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849241"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

設定旗標，以判斷是否可以快取作業的檔案，並將其提供給對等，以及作業是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|值|此值為不帶正負號的整數，其在二進位標記法中具有下列對位的解讀。</br>1-作業可以從對等下載內容。</br>2-作業的檔案可以快取並提供給對等。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myJob*的作業設定旗標，讓它可以從對等下載內容。
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)