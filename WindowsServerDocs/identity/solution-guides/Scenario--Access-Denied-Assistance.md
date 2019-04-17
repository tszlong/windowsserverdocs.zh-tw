---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: "案例存取的協助"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>案例：存取的協助

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在嘗試存取共用的檔案和資料夾的檔案伺服器的它們不需要權限時，使用者會出現存取的訊息。 系統管理員通常不需要進行疑難排解的存取問題，可讓您修正問題的相關硬碟適當操作。  
  
## <a name="scenario-description"></a>案例描述  
存取的協助是提供下列方式的相關問題進行疑難排解的 Windows Server 2012 中的新功能的檔案和資料夾的存取：  
  
-   **自我尋求協助。** 如果使用者可以判斷問題，使其可以存取要求唯讀問題的影響企業太低，且不特殊例外所需的中央存取原則。 存取的協助提供的檔案伺服器管理員可自訂的公司特定資訊的存取的訊息。 例如，系統管理員可以安裝訊息，讓使用者可以要求的資料擁有的存取權不需要的檔案伺服器管理員。  
  
-   **資料擁有者來協助。** 您可以定義散發清單的共用資料夾，並使資料夾擁有者接收的電子郵件通知，當使用者需要存取設定。 如果資料擁有者不知道如何協助存取的使用者，擁有者可以檔案伺服器管理員向前這項資訊。  
  
-   **檔案伺服器系統管理員尋求協助。** 當使用者無法修正問題，並協助的資料擁有者無法使用這種類型的協助。  Windows Server 2012 提供使用者介面位置檔案伺服器管理員可以檢視有效權限使用者的檔案或資料夾，讓它更容易存取問題的疑難排解。  
  
Windows Server 2012 存取的協助提供檔案伺服器管理員存取的相關詳細資訊，他們就可以判斷的問題和適當的工具，讓他們可以設定的變更滿足存取要求。 例如，使用者可能會依照此程序存取它們目前不可以存取檔案：  
  
-   使用者嘗試朗讀 \\\financeshares 共用資料夾中的檔案，但伺服器會顯示存取的訊息。  
  
-    Windows Server 2012 顯示選項，以要求協助使用者存取的協助資訊。  
  
-   如果使用者要求存取的資源，伺服器會傳送具有存取要求資訊的電子郵件資料夾擁有者。  
  
您可以找到計劃的資訊設定存取的協助[計劃 Access-Denied 協助的](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)。  
  
您可以找到關於設定存取的協助步驟[Deploy Access-Denied 協助與 #40; 示範步驟和 #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>本案例中  
本案例是動態存取控制案例的一部分。 如需關於動態存取控制的詳細資訊，請查看：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>實用的應用程式  
Windows Server 2012 存取的協助提供動態存取控制，讓使用者要求直接從存取的訊息共用的檔案和資料夾的存取權的能力。  
  
## <a name="BKMK_NEW"></a>本案例中所包含的功能  
下表列出的功能是此案例，並告訴他們支援的方式。  
  
|功能|它如何支援此案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|您可以使用該檔案伺服器上的檔案伺服器資源管理員」主控台設定存取的協助。|  
|[檔案和儲存服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是一個檔案，並儲存服務角色服務，並包含數一組可用來管理您的網路上的檔案伺服器的功能。|  
  


