---
title: dfsdiag TestDCs
description: 適用于 dfsdiag TestDCs 的 Windows 命令主題，它會檢查指定網域中的網域控制站設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092ce3710eb6d209f596683bd4ad054dadd11aa3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846313"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由在指定網域中的每個網域控制站上執行下列測試，檢查網域控制站的設定：  
  
-   確認分散式檔案系統（DFS）命名空間服務正在執行，且其啟動類型設定為 [自動]。  
  
-   檢查是否支援 NETLOGON 和 SYSvol 的網站成本參考。  
  
-   依主機名稱和 IP 位址驗證網站關聯的一致性。

## <a name="syntax"></a>語法  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|[/Domain]：`<domain_name>`|您想要檢查的網域。|  
  
## <a name="remarks"></a>備註  

[/Domain] 是選擇性參數。 預設值是本機主機加入的本機網域。  
  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
若要確認 Contoso.com 網域中的網域控制站設定，請輸入：  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

