---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: 案例實作的檔案伺服器上的資料保留
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880249"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>案例：在檔案伺服器上實作資訊保留

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

保留期限是文件到期之間應該被保留的時間。 保留期限取決於每個組織而有所不同。 您可以將資料夾中的檔案分類為短期、中期或長期保留期限，然後為每個期限指派一個時間範圍。 如果檔案適用法務保存措施，就可以無限期地保留檔案。  
  
## <a name="BKMK_OVER"></a>案例描述  
檔案分類基礎結構及檔案伺服器資源管理員會使用檔案管理工作和檔案分類，對一組檔案套用保留期限。 您可以在資料夾上指派一個保留期限，然後使用檔案管理工作來設定所指派之保留期限的持續時間。 當資料夾中的檔案快要到期時，檔案的擁有人會收到一封通知電子郵件。 您也可以將檔案分類為適用法務保存措施，這樣檔案管理工作就不會讓檔案到期。  
  
您可以在 [Plan for Retention of Information on File Servers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)找到設定保留的規劃資訊。  
  
您可以找到分類適合法務保存措施的檔案和設定保留期限中的步驟[部署實作保留資訊的檔案伺服器上&#40;示範步驟&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)。  
  
> [!NOTE]  
> 上述案例只討論如何手動分類適合法務保存措施的文件。 不過，就可以自動分類適合法務保存措施的文件的 Windows Server 2012 中。 若要如此，其中一種方式便是建立 Windows PowerShell 分類器，比對檔案擁有者與在法務保存措施之下的使用者帳戶清單。 如果檔案擁有者屬於使用者帳戶清單，檔案會被歸類為適合法務保存措施。  
  
## <a name="in-this-scenario"></a>在這個案例中  
此案例是動態存取控制案例的一部分。 如需動態存取控制的其他資訊，請參閱：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>在此案例中所包含的功能  
下表列出這個案例中的功能，並說明它們如何支援這個案例。  
  
|功能|如何支援本案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|檔案分類基礎結構是包含在檔案伺服器資源管理員的功能。|  
|[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是包含在檔案服務伺服器角色的功能。|  
  
  


