---
title: Dfsdiag TestDFSConfig
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 922b78b87f3bb66765b87348a3bf136e14c9e837
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436137"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag TestDFSConfig

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查設定分散式檔案系統\(DFS\)命名空間，藉由執行下列動作：  
  
-   確認 DFS 命名空間服務正在執行，而且它的啟動類型設定為自動，命名空間的所有伺服器上。  
  
-   確認 DFS 登錄設定是一致的命名空間伺服器之間。  
  
-   驗證叢集的命名空間伺服器執行 Windows Server 2008 或更新版本上的下列相依性：  
  
    -   命名空間的根資源相依性網路名稱資源。  
  
    -   網路名稱資源相依性的 IP 位址資源。  
  
    -   命名空間實體磁碟資源上的根資源相依性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>參數  
  
|       參數       |               描述               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | 命名空間\(DFS 根目錄\)診斷。 |
  
## <a name="BKMK_Examples"></a>範例  
若待決定，要輸入：  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

