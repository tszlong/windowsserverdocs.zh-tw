---
title: 針對執行 Hyper-v 的伺服器，建議使用網域成員資格
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 48ea52e962f2f476d1428a69bab6c6e38c4ec005
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364921"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>針對執行 Hyper-v 的伺服器，建議使用網域成員資格

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*這部伺服器是工作組的成員。*  
  
## <a name="impact"></a>影響  
  
*此伺服器沒有中央管理。*  
  
將此電腦加入網域可讓您透過原則進行集中式管理，以進行身分識別、安全性和審核。  
  
## <a name="resolution"></a>解析度  
  
*如果您有可用的網域環境，請將此伺服器加入該網域。*  
  
> [!IMPORTANT]  
> 我們建議您檢查在這部電腦上的虛擬機器中執行的工作負載，以判斷將這部電腦加入網域是否有安全性影響。 如果有任何虛擬機器是虛擬網域控制站，請參閱[虛擬網域控制站的規劃考慮](https://go.microsoft.com/fwlink/?LinkId=190192)（ https://go.microsoft.com/fwlink/?LinkId=190192) 。  
  
將電腦加入網域需要電腦和網域的許可權：   
- 在電腦上，您將需要屬於 Administrators 群組成員的使用者帳戶。 請使用這種類型的帳戶登入，或在系統提示時提供帳戶的使用者名稱和密碼。   
- 在網域上，您將需要有權將電腦加入網域的使用者帳戶。 系統會提示您輸入使用者名稱和密碼。  
  
如需指示，請參閱將[電腦加入網域](https://go.microsoft.com/fwlink/?LinkId=190193)（ https://go.microsoft.com/fwlink/?LinkId=190193) 。  
  


