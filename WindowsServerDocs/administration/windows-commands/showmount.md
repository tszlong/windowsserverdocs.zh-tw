---
title: showmount
description: Showmount –命令的參考文章，此命令會顯示在指定電腦上，Server for NFS 所匯出的已掛接檔案系統的相關資訊。
ms.topic: reference
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3af87ba4ed42789cf07a2ae63ce9e720043a196e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718325"
---
# <a name="showmount"></a>showmount

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用 **showmount –** ，顯示指定電腦上 SERVER for NFS 所匯出的已掛接檔案系統的相關資訊。 如果您未指定伺服器，此命令會顯示執行 **showmount –** 命令之電腦的相關資訊。

## <a name="syntax"></a>語法

```
showmount {-e|-a|-d} <server>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -E | 顯示伺服器上匯出的所有檔案系統。 |
| -a | 顯示所有網路檔案系統 (NFS) 用戶端，以及每個已掛接伺服器上的目錄。 |
| -d | 顯示目前由 NFS 用戶端裝載之伺服器上的所有目錄。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)