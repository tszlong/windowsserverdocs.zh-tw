---
title: dfsdiag
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819139"
---
# <a name="dfsdiag"></a>dfsdiag



`Dfsdiag`命令提供診斷資訊的 DFS 命名空間。

## <a name="syntax"></a>語法

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|檢查網域控制站設定。|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|檢查網站關聯。|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|檢查 DFS 命名空間設定。|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|檢查 DFS 命名空間的完整性。|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|檢查轉介回應。|
|/?|在命令提示字元顯示說明。|

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)