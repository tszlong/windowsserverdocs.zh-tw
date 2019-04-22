---
title: dfsutil
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 245f8fb2e6419d22da3e2e76eebd9f801ab90664
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821479"
---
# <a name="dfsutil"></a>dfsutil

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Dfsutil 命令管理 DFS 命名空間、 伺服器和用戶端。 dfsutil 命令會將原始的分散式檔案系統術語中，使用提供的大部分命令的說明更新 DFS 命名空間術語。

如需如何使用此命令的範例，請參閱 

## <a name="syntax"></a>語法

```
command </parameter> </param2>
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|[dfsutil Root](dfsutil-root.md)|顯示、 建立、 移除、 匯入、 匯出命名空間根目錄。|
|[dfsutil Link](dfsutil-link.md)|顯示、 建立、 移除或移動資料夾\(連結\)。|
|[dfsutil Target](dfsutil-target.md)|出現時，請建立之後，移除資料夾目標或命名空間伺服器。|
|[dfsutil Property](dfsutil-property.md)|顯示或修改資料夾目標或命名空間伺服器。|
|[dfsutil Client](dfsutil-client.md)|顯示或修改用戶端資訊或登錄機碼。|
|[dfsutil Server](dfsutil-server.md)|顯示或修改命名空間設定。|
|[dfsutil Diag](dfsutil-diag.md)|執行診斷，或檢視 dfsdirs\/dfspath。|
|[dfsutil Domain](dfsutil-domain.md)|顯示所有網域\-基礎網域中的命名空間。|
|[dfsutil Cache](dfsutil-cache.md)|顯示或排清用戶端快取。|
|[dfsutil oldcli](dfsutil-oldcli.md)|使用 dfsutil \/oldcli 命令，以使用原始的 dfsutil 語法。|

## <a name="remarks-optional-section"></a>註解 <optional section>
如果您指定的物件\(例如命名空間伺服器\)命令結尾的大部分命令將不需要進一步的參數或命令顯示物件的相關資訊。 比方說，當使用 dfsutil Root 命令時，您可以將命名空間根目錄附加至命令，以檢視根的相關資訊。

## <a name="BKMK_Examples"></a>範例
<Here is where you put a detailed description of your example.>

```
This /is /the /example /of /calling /command /with /parameters
```

<Here is where you put a detailed description of another example.>

```
This /is /a:different /example
```

## <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)


