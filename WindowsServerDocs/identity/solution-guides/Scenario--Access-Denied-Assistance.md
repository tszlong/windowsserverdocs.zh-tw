---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: 案例拒絕存取的協助
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2d3f0f7f0fa611ab34145aa35291ff9d07b0b205
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955950"
---
# <a name="scenario-access-denied-assistance"></a>案例：拒絕存取時的協助

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用者在檔案伺服器上嘗試存取沒有權限的共用檔案和資料夾時，會收到拒絕存取的訊息。 系統管理員通常沒有適當的關聯內容來疑難排解存取問題，因此很難解決問題。  
  
## <a name="scenario-description"></a>案例描述  
拒絕存取時的協助是 Windows Server 2012 中的新功能，可提供下列方法來疑難排解與檔案和資料夾的存取相關的問題：  
  
-   **自我協助。** 如果使用者可以判斷問題並修復問題以便取得所要求的存取，對企業的影響就會變低，而且集中存取原則中不需要額外的特殊例外狀況。 拒絕存取時的協助為檔案伺服器系統管理員提供可以自訂的組織特定資訊的拒絕存取訊息。 例如，系統管理員可以設定訊息，讓使用者向資料擁有者請求存取權，而不用檔案伺服器系統管理員的介入。  
  
-   **資料擁有者提供的協助。** 您可以定義共用資料夾的通訊群組清單，並將它設定為當使用者需要存取資料夾時，資料夾擁有者會收到電子郵件通知。 如果資料擁有者不知道如何幫助使用者取得存取權，擁有者可以將此資訊轉送給檔案伺服器系統管理員。  
  
-   **檔案伺服器系統管理員提供的協助。** 當使用者無法修正問題且資料擁有者也不能幫助時，就提供這類協助。  Windows Server 2012 提供使用者介面，讓檔案伺服器系統管理員可以在檔案或資料夾上查看使用者的有效許可權，以便更輕鬆地對存取問題進行疑難排解。  
  
Windows Server 2012 中的拒絕存取時的協助為檔案伺服器系統管理員提供相關的存取詳細資料，讓他們能夠判斷問題和適當的工具，讓他們能夠進行設定變更以滿足存取要求。 例如，使用者可能遵循此程序存取目前並沒有存取權的檔案：  
  
-   使用者嘗試讀取 \financeshares 共用資料夾中的檔案 \\ ，但伺服器顯示拒絕存取的訊息。  
  
-    Windows Server 2012 會向使用者顯示拒絕存取時的協助資訊，並提供要求協助的選項。  
  
-   如果使用者要求資源存取權，伺服器會傳送一封包含存取要求資訊的電子郵件給資料夾擁有者。  
  
您可以在[規劃拒絕存取時的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)中找到設定拒絕存取時的協助的規劃資訊。  
  
您可以在部署拒絕存取時的協助中找到設定拒絕存取時的協助[&#40;示範步驟&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)的相關步驟。  
  
## <a name="in-this-scenario"></a>在這個案例中  
此案例是動態存取控制案例的一部分。 如需動態存取控制的其他資訊，請參閱：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>實際應用  
Windows Server 2012 中的拒絕存取時的協助讓使用者能夠直接從拒絕存取訊息要求存取共用檔案和資料夾，藉此提供動態存取控制。  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的功能  
下表列出這個案例中的功能，並說明它們如何支援這個案例。  
  
|功能|如何支援本案例|  
|-----------|---------------------------------|  
|[File Server Resource Manager Overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831701(v=ws.11))|在檔案伺服器上使用 [檔案伺服器資源管理員] 主控台，可以設定拒絕存取時的協助。|  
|[檔案和存放服務概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831487(v=ws.11))|[檔案伺服器資源管理員] 為 [檔案和存放服務] 角色服務，它包含一組可以用來管理網路上檔案伺服器的功能。|  
  
