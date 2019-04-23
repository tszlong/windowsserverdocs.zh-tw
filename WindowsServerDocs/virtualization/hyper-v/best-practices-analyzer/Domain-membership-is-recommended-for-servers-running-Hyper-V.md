---
title: 網域成員資格被建議用於執行 HYPER-V 的伺服器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860899"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>網域成員資格被建議用於執行 HYPER-V 的伺服器

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*此伺服器是工作群組的成員。*  
  
## <a name="impact"></a>影響  
  
*沒有此伺服器無法集中管理。*  
  
此電腦加入網域，可讓透過身分識別、 安全性和稽核原則的集中式的管理。  
  
## <a name="resolution"></a>解析度  
  
*如果您有可用的網域環境，請將此伺服器加入該網域。*  
  
> [!IMPORTANT]  
> 我們建議您檢閱此判斷是否有安全性影響此電腦加入網域的電腦上執行虛擬機器中的工作負載。 如果任何虛擬機器的虛擬的網域控制站，請參閱[虛擬化網域控制站的規劃考量](https://go.microsoft.com/fwlink/?LinkId=190192)(https://go.microsoft.com/fwlink/?LinkId=190192)。  
  
加入網域的電腦需要的電腦和網域的權限：   
- 在電腦上，您必須是 Administrators 群組的成員的使用者帳戶。 請使用此類型的帳戶，登入或使用者名稱和密碼提供帳戶，當系統提示您。   
- 在網域中，您必須加入網域的電腦已獲授權的使用者帳戶。 將提示您輸入使用者名稱和密碼。  
  
如需相關指示，請參閱 <<c0> [ 將電腦加入網域](https://go.microsoft.com/fwlink/?LinkId=190193)(https://go.microsoft.com/fwlink/?LinkId=190193)。  
  


