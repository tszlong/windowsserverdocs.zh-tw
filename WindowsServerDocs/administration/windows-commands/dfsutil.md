---
title: dfsutil
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377985"
---
# <a name="dfsutil"></a>dfsutil

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Dfsutil 命令會管理 DFS 命名空間、伺服器和用戶端。 dfsutil 命令會使用原始的分散式檔案系統術語，並提供更新的 DFS 命名空間術語，做為大部分命令的說明。

如需如何使用此命令的範例，請參閱 

## <a name="syntax"></a>語法

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parameters

|參數|描述|
|-------|--------|
|[dfsutil Root](dfsutil-root.md)|顯示、建立、移除、匯入、匯出命名空間根。|
|[dfsutil 連結](dfsutil-link.md)|顯示、建立、移除或移動資料夾 \(連結\)。|
|[dfsutil 目標](dfsutil-target.md)|顯示、建立、移除資料夾目標或命名空間伺服器。|
|[dfsutil 屬性](dfsutil-property.md)|顯示或修改資料夾目標或命名空間伺服器。|
|[dfsutil 用戶端](dfsutil-client.md)|顯示或修改用戶端資訊或登錄機碼。|
|[dfsutil Server](dfsutil-server.md)|顯示或修改命名空間設定。|
|[dfsutil 診斷](dfsutil-diag.md)|執行診斷或 view dfsdirs\/dfspath。|
|[dfsutil 網域](dfsutil-domain.md)|顯示網域中所有以網域\-為基礎的命名空間。|
|[dfsutil 快取](dfsutil-cache.md)|顯示或排清用戶端快取。|
|[dfsutil oldcli](dfsutil-oldcli.md)|使用 dfsutil \/oldcli 命令來使用原始的 dfsutil 語法。|

## <a name="remarks-optional-section"></a>備註 <optional section>
如果您指定物件 \(例如在命令結尾\) 的命名空間伺服器，大部分的命令都會顯示物件的相關資訊，而不需要進一步的參數或命令。 例如，使用 [dfsutil Root] 命令時，您可以將命名空間根目錄附加至命令，以查看根的相關資訊。

## <a name="BKMK_Examples"></a>典型
&lt;此處，您會在其中放置範例的詳細描述。&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;此處，您會在其中放置另一個範例的詳細描述。&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>其他參考

-   [命令列語法關鍵](command-line-syntax-key.md)


