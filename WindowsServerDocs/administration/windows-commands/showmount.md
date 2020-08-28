---
title: showmount
description: Showmount –的參考文章，會顯示掛接的目錄。
ms.topic: reference
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba118e711af040bba7dd6c1e63a14405b3116743
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024792"
---
# <a name="showmount"></a>showmount

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用 **showmount –** 來顯示裝載的目錄。

## <a name="syntax"></a>語法
```
showmount {-e|-a|-d} <Server>
```

## <a name="description"></a>描述
**Showmount –** 命令 \- 行公用程式會顯示伺服器所指定電腦上，server for NFS 所匯出的已掛接*Server*檔案系統的相關資訊。 如果未提供 *伺服器* ， **showmount –** 會顯示執行 **showmount –** 命令之電腦的相關資訊。

您必須提供下列其中一個選項：

- ** \- e** -顯示伺服器上匯出的所有檔案系統。
- ** \- a** -顯示所有網路檔案系統 \( NFS \) 用戶端，以及伺服器上每個已掛接的目錄。
- ** \- d** -顯示伺服器上目前由 NFS 用戶端裝載的所有目錄。

## <a name="see-also"></a>另請參閱
[網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)