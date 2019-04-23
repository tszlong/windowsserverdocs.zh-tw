---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: 案例拒絕存取時的協助
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829089"
---
# <a name="scenario-access-denied-assistance"></a>案例：拒絕存取時的協助

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用者在檔案伺服器上嘗試存取沒有權限的共用檔案和資料夾時，會收到拒絕存取的訊息。 系統管理員通常沒有適當的關聯內容來疑難排解存取問題，因此很難解決問題。  
  
## <a name="scenario-description"></a>案例描述  
拒絕存取時的協助是在 Windows Server 2012，提供下列方式來進行疑難排解的相關問題的新功能來存取檔案和資料夾：  
  
-   **自我協助。** 如果使用者可以判斷問題並修復問題以便取得所要求的存取，對企業的影響就會變低，而且集中存取原則中不需要額外的特殊例外狀況。 拒絕存取時的協助為檔案伺服器系統管理員提供可以自訂的組織特定資訊的拒絕存取訊息。 例如，系統管理員可以設定訊息，讓使用者向資料擁有者請求存取權，而不用檔案伺服器系統管理員的介入。  
  
-   **資料擁有者提供的協助。** 您可以定義共用資料夾的通訊群組清單，並將它設定為當使用者需要存取資料夾時，資料夾擁有者會收到電子郵件通知。 如果資料擁有者不知道如何幫助使用者取得存取權，擁有者可以將此資訊轉送給檔案伺服器系統管理員。  
  
-   **檔案伺服器系統管理員提供的協助。** 當使用者無法修正問題且資料擁有者也不能幫助時，就提供這類協助。  Windows Server 2012 提供使用者介面，讓檔案伺服器系統管理員可以檢視使用者的有效權限在檔案或資料夾，以便更輕鬆地對存取問題進行疑難排解。  
  
拒絕存取時的協助，在 Windows Server 2012 檔案伺服器系統管理員提供存取相關詳細資料以便判斷問題並提供適當的工具，讓它們可以進行設定變更來滿足存取要求。 例如，使用者可能遵循此程序存取目前並沒有存取權的檔案：  
  
-   使用者嘗試讀取檔案\\\financeshares 共用資料夾，但伺服器顯示拒絕存取訊息。  
  
-    Windows Server 2012 的使用者要求協助的選項顯示拒絕存取時的協助資訊。  
  
-   如果使用者要求資源存取權，伺服器會傳送一封包含存取要求資訊的電子郵件給資料夾擁有者。  
  
您可以在 [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)中找到設定拒絕存取時的協助的規劃資訊。  
  
您可以找到設定拒絕存取時的協助，在[各別協助&#40;示範步驟&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)。  
  
## <a name="in-this-scenario"></a>在這個案例中  
此案例是動態存取控制案例的一部分。 如需動態存取控制的其他資訊，請參閱：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>實際應用  
拒絕存取時的協助，在 Windows Server 2012 中的動態存取控制 」 可藉由讓使用者可以要求直接從拒絕存取訊息的存取權的共用的檔案和資料夾。  
  
## <a name="BKMK_NEW"></a>在此案例中所包含的功能  
下表列出這個案例中的功能，並說明它們如何支援這個案例。  
  
|功能|如何支援本案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|在檔案伺服器上使用 [檔案伺服器資源管理員] 主控台，可以設定拒絕存取時的協助。|  
|[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|[檔案伺服器資源管理員] 為 [檔案和存放服務] 角色服務，它包含一組可以用來管理網路上檔案伺服器的功能。|  
  


