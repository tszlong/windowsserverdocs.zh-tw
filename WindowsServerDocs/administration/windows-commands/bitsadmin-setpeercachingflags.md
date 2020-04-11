---
title: bitsadmin setpeercachingflags
description: 適用于**bitsadmin setpeercachingflags**的 Windows 命令主題，其會設定旗標來判斷作業的檔案是否可以快取並提供給對等，以及作業是否可以從對等下載內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b4a7807975fb46440301e30b1fdbd01784d7c85
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122771"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

設定旗標，以判斷是否可以快取作業的檔案，並將其提供給對等，以及作業是否可以從對等下載內容。

## <a name="syntax"></a>語法

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| 值 | 不帶正負號的整數，包括：<ul><li>**1.** 作業可以從對等下載內容。</li><li>**2.** 作業的檔案可以快取並提供給對等。</li></ul> |

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業設定旗標，讓它可以從對等下載內容。

```
C:\>bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)