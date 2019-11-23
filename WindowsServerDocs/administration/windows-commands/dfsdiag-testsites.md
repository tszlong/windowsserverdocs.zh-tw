---
title: dfsdiag TestSites
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378386"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查 active directory 網域服務 \(AD DS\) 網站的設定，方法是確認作為命名空間伺服器或資料夾 \(連結\) 目標的伺服器在所有網域控制站上都有相同的網站關聯。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parameters  
  
|參數|描述|  
|-------|--------|  
|\/機：<server name>|要驗證其網站關聯的伺服器名稱。|  
|\/DFSpath：<namespace root or DFS folder>|命名空間根目錄或分散式檔案系統 \(DFS\) 資料夾 \(連結\) 與要驗證其網站關聯的目標。|  
|\/遞迴|列舉並驗證指定之命名空間根目錄下所有資料夾目標的網站關聯。|  
|\/完整|確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊。|  
  
## <a name="BKMK_Examples"></a>典型  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
若要 TBD，請輸入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

