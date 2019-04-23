---
title: Dfsdiag TestDFSIntegrity
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837089"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查 「 分散式檔案系統的完整性\(DFS\)命名空間，藉由執行下列測試：  
  
-   DFS 中繼資料損毀或網域控制站之間的不一致性檢查。  
  
-   驗證的存取權限設定\-基礎的列舉，以確保一致 DFS 中繼資料和命名空間伺服器共用。  
  
-   偵測到重疊的 DFS 資料夾\(連結\)，重複的資料夾和重疊資料夾目標的資料夾。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|若要診斷 DFS 命名空間。|  
|\/Recurse|執行測試其中包括命名空間連結。|  
|\/完整|確認共用和所有資料夾目標上的 NTFS Acl 和用戶端端設定的一致性。 它也會確認，線上屬性會設定。|  
  
## <a name="BKMK_Examples"></a>範例  
若待決定，要輸入：  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

