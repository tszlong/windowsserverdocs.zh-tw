---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: "案例實作保留的檔案伺服器的資訊"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>案例︰ 檔案伺服器實作保留的資訊

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

保留期間是應該文件的時間量保持之前已經過期。 根據組織，可以不同的保留時間。 您可以分類有簡短、中或長期保留期間的資料夾中的檔案，並為每一段指定時間範圍。 若要將它放法律保留無限期保留檔案。  
  
## <a name="BKMK_OVER"></a>案例描述  
檔案分類基礎結構和檔案伺服器資源管理員使用檔案管理工作和檔案分類套用保留期間的準則一組的檔案。 您可以保留期間指定資料夾，並設定指派的保留期間的到最後一個使用的檔案管理工作。 即將到期的資料夾中的檔案時，該檔案的擁有者接收的通知電子郵件。 您也可以將分類成法律保留檔案管理工作不會到期檔案，檔案。  
  
您可以找到計劃的資訊設定保持在[檔案伺服器計劃保留的資訊用於](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)。  
  
您可以找到步驟誤判按壓法律按住的檔案及設定中的保留期間的[部署實作保留的資訊檔案伺服器與 #40; 示範步驟和 #41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> 該案例僅討論如何手動分類的法律按住文件。 不過，也可在 Windows Server 2012 自動分類的法律按住文件。 若要這樣做方式是建立比較 [檔案擁有者的法律按住不放在帳號清單 Windows PowerShell 器。 如果檔案擁有者的部分使用者 account 清單中，檔案就會被歸類的法律按住不放。  
  
## <a name="in-this-scenario"></a>本案例中  
本案例是動態存取控制案例的一部分。 如需關於動態存取控制的詳細資訊，請查看：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>本案例中所包含的功能  
下表列出的功能是此案例，並告訴他們支援的方式。  
  
|功能|它如何支援此案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|檔案分類基礎結構是隨附檔案伺服器資源管理員] 中的功能。|  
|[檔案和儲存服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是隨附檔案的服務伺服器角色功能。|  
  
  


