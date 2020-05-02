---
title: dfsdiag TestSites
description: Dfsdiag TestSites 的參考主題，它會確認做為命名空間伺服器或資料夾（連結）目標的伺服器在所有網域控制站上都有相同的網站關聯，藉此檢查 active directory 網域服務（AD DS）網站的設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68048699a812beac94fa121d6801da5f42e5393b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719558"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查 active directory 網域服務（AD DS）網站的設定，方法是確認作為命名空間伺服器或資料夾（連結）目標的伺服器在所有網域控制站上都有相同的網站關聯。

## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/機器碼<server name>|要驗證其網站關聯的伺服器名稱。|  
|\/DFSpath:<namespace root or DFS folder>|命名空間根或分散式檔案系統（DFS）資料夾（連結），具有要驗證其網站關聯的目標。|  
|\/遞迴|列舉並驗證指定之命名空間根目錄下所有資料夾目標的網站關聯。|  
|\/寫|確認伺服器的 AD DS 和登錄包含相同的網站關聯資訊。|  
  
## <a name="examples"></a>範例  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

