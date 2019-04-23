---
title: Dfsdiag TestDCs
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836599"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查網域控制站的設定所指定的網域中的每個網域控制站上執行下列測試：  
  
-   確認 「 分散式檔案系統\(DFS\)命名空間服務正在執行，且它的啟動類型設定為自動。  
  
-   檢查針對站台支援\-估計成本 NETLOGON 與 SYSvol 的轉介。  
  
-   驗證主機名稱和 IP 位址的站台關聯的一致性。  
  
  
  
## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|\/網域：<Domain name>|您想要檢查的網域。|  
  
## <a name="remarks"></a>備註  
\/網域是選擇性參數。 預設值是本機主機已加入本機網域。  
  
## <a name="BKMK_Examples"></a>範例  
若要確認 Contoso.com 網域中的網域控制站的組態，請輸入：  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

