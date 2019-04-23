---
title: Dfsdiag TestSites
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873969"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查 active directory 網域服務的組態\(AD DS\)驗證的站台做為命名空間伺服器或資料夾的伺服器\(連結\)目標上所有的網域中有相同的站台關聯控制站。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/電腦：<server name>|若要確認站台關聯的伺服器名稱。|  
|\/DFSpath:<namespace root or DFS folder>|命名空間根目錄或分散式檔案系統\(DFS\)資料夾\(連結\)要確認站台關聯的目標。|  
|\/Recurse|列舉，並確認指定的命名空間根目錄下的所有資料夾目標的站台關聯。|  
|\/完整|確認 AD DS 與伺服器的登錄都包含相同的站台關聯資訊。|  
  
## <a name="BKMK_Examples"></a>範例  
若待決定，要輸入：  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
若待決定，要輸入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
若待決定，要輸入：  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

