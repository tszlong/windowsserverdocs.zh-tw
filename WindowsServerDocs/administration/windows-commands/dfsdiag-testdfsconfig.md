---
title: dfsdiag TestDFSConfig
description: 適用于 dfsdiag TestDFSConfig 的 Windows 命令主題，它會檢查分散式檔案系統（DFS）命名空間的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffb75ba26b4ed90dbf5c8bfda80f4a81f986e46a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846331"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由執行下列動作，檢查分散式檔案系統（DFS）命名空間的設定：  
  
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
  
#### <a name="parameters"></a>參數  
  
|       參數       |               描述               |
|-----------------------|-----------------------------------------|
| /DFSRoot：`<namespace>` | 要診斷的命名空間（DFS 根）。 |
  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

