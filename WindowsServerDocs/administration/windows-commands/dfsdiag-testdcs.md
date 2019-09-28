---
title: dfsdiag TestDCs
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378444"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由在指定網域中的每個網域控制站上執行下列測試，檢查網域控制站的設定：  
  
-   確認分散式檔案系統 \(DFS @ no__t-1 Namespace 服務正在執行，且其啟動類型設定為 [自動]。  
  
-   檢查是否支援 NETLOGON 和 SYSvol 的 site @ no__t-0costed 的參考。  
  
-   依主機名稱和 IP 位址驗證網站關聯的一致性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/Domain： <Domain name>|您想要檢查的網域。|  
  
## <a name="remarks"></a>備註  
@no__t 0Domain 是選擇性參數。 預設值是本機主機加入的本機網域。  
  
## <a name="BKMK_Examples"></a>典型  
若要確認 Contoso.com 網域中的網域控制站設定，請輸入：  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>其他參考  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

