---
title: dfsdiag
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 61a6ab9a90e4d0220cfe27d2d21120be19b9ff1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378316"
---
# <a name="dfsdiag"></a>dfsdiag



@No__t-0 命令會提供 DFS 命名空間的診斷資訊。

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
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|檢查參照回應。|
|/?|在命令提示字元顯示說明。|

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)