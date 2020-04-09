---
title: dfsdiag
description: 適用于 dfsdiag 的 Windows 命令主題，其提供 DFS 命名空間的診斷資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c895dabbbafbe8ea253920d3bc6de17f42918e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846191"
---
# <a name="dfsdiag"></a>dfsdiag

提供 DFS 命名空間的診斷資訊。

## <a name="syntax"></a>語法

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|檢查網域控制站設定。|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|檢查網站關聯。|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|檢查 DFS 命名空間設定。|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|檢查 DFS 命名空間的完整性。|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|檢查參照回應。|
|/?|在命令提示字元顯示說明。|

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)