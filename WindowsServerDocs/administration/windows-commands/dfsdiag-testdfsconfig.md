---
title: dfsdiag TestDFSConfig
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8008e02d588edaa6fe7700a331c43f9680d89431
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378416"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列動作，檢查分散式檔案系統 @no__t 0DFS @ no__t-1 命名空間的設定：  
  
-   確認 DFS 命名空間服務正在執行，且其啟動類型已在所有命名空間伺服器上設定為 [自動]。  
  
-   確認 DFS 登錄設定在命名空間伺服器之間是一致的。  
  
-   在執行 Windows Server 2008 或更新版本的叢集命名空間伺服器上驗證下列相依性：  
  
    -   網路名稱資源的命名空間根資源相依性。  
  
    -   IP 位址資源的網路名稱資源相依性。  
  
    -   實體磁片資源上的命名空間根資源相依性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>參數  
  
|       參數       |               描述               |
|-----------------------|-----------------------------------------|
| \/DFSRoot： <namespace> | 命名空間 \(DFS root @ no__t-1 以進行診斷。 |
  
## <a name="BKMK_Examples"></a>典型  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

