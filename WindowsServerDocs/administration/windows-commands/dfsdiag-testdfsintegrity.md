---
title: dfsdiag TestDFSIntegrity
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f344e2d1fecc542efc39688f20165fd3e39a04a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378435"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列測試，檢查分散式檔案系統 \(DFS\) 命名空間的完整性：  
  
-   檢查網域控制站之間的 DFS 中繼資料損毀或不一致。  
  
-   驗證存取\-型列舉的設定，以確保 DFS 中繼資料與命名空間伺服器共用之間的設定一致。  
  
-   偵測重迭的 DFS 資料夾，\(連結\)、重複的資料夾，以及具有重迭資料夾目標的資料夾。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|參數|描述|  
|-------|--------|  
|\/DFSRoot：<DFS root path>|要診斷的 DFS 命名空間。|  
|\/遞迴|執行測試，包括命名空間連結。|  
|\/完整|在所有資料夾目標上驗證共用和 NTFS Acl 和用戶端設定的一致性。 它也會驗證 online 屬性是否已設定。|  
  
## <a name="BKMK_Examples"></a>典型  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

