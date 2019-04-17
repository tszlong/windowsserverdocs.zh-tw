---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: "案例中央存取原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>案例：中央存取原則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檔案的中央存取原則讓組織集中部署與管理授權原則，包括條件運算式使用使用者群組，使用者宣告、裝置宣告，以及資源屬性。 （宣告是包括與其相關的物件的屬性判斷提示。） 例如存取高商務影響 (HBI) 的資料，使用者必須任職員工，存取受管理的裝置，而使用智慧卡登入。 這些原則定義和裝載在 Active Directory Domain Services (AD DS)。  
  
組織存取原則皆是以遵循與商務用法規需求。 例如，如果組織只檔案擁有者和獲准檢視 PII 資訊部門人們資源（小時又）的成員檔案中的個人資訊 (PII) 上限制存取商務用需求，這項原則適用於 PII 檔案地方它們位於檔案伺服器跨組織。 在此範例中，您需要可以：  
  
-   找出並標記包含 PII 的檔案。  
  
-   找出小時又成員獲准檢視 PII 資訊的群組。  
  
-   建立套用到所有檔案，包含 PII 地方它們位於檔案伺服器跨組織的中央存取原則。  
  
部署，及執行授權原則促進會可以的原因很多，適用於組織的多個層級。 以下是一些範例原則類型：  
  
-   **授權全組織的原則。** 最常見的資訊安全 office 車載機起始，這項原則授權受到 compliance 或高階組織需求，且相關跨組織。 例如，都可存取只全職員工 HBI 檔案。  
  
-   **部門授權的原則。** 是在組織中的每個部門有一些想要執行的特殊資料處理需求。 例如，財務部門可能要限制財經員工財經伺服器的存取權。  
  
-   **特定資料管理原則。** 這項原則通常與相容性和企業需求，以及它針對保護正確存取受管理的資訊。 例如金融機構可能實作資訊牆，使分析人員不會存取投資資訊和仲介不會存取分析資訊。  
  
-   **需要知道原則。** 此驗證原則類型通常是一起使用與先前原則類型。 例如，廠商應該可以存取和編輯與的專案正在處理相關的檔案。  
  
真實環境也告訴我們，需要有例外組織可以快速反應很重要的企業需要發生時，每個授權原則。 高階主管無法尋找其智慧卡，需要快速存取 HBI 資訊，例如可以撥打電話工程師取得暫時例外來存取該資訊。  
  
中央存取原則做為安全性 umbrellas 組織套用其伺服器上。 這些原則美化（但不是會取代）任意存取控制清單 (DACL)，適用於的檔案和資料夾的存取本機原則。 例如，如果在檔案上的 DACL 可讓存取特定使用者，但會套用至該檔案的中央原則會限制的相同使用者存取，使用者無法取得檔案的存取權。 如果的中央存取原則可讓存取權限，但 DACL 不允許的存取，使用者無法取得檔案的存取權。  
  
中央存取原則規則有下列邏輯一部分：  
  
-   **適。** 定義資料條件原則套用到，例如 Resource.BusinessImpact=High。  
  
-   **存取條件。** 一或多個存取控制定義的項目 (a) 可存取的資料，例如允許清單 |完全控制 |User.EmployeeType=FTE。  
  
-   **例外。** 定義的原則，例如 MemberOf(HBIExceptionGroup) 例外一或多個 a 的詳細清單。  
  
以下兩個工作流程顯示中央存取和稽核原則。  
  
![方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**圖 1**中央存取和稽核原則的概念  
  
![方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**圖 2 所示**的中央存取原則工作流程  
  
中央授權原則結合了特定地區下列元件：  
  
-   針對特定類型的資訊，例如 HBI 或 PII 集中定義的存取規則的清單。  
  
-   包含的規則清單集中定義的原則。  
  
-   原則識別碼指派給期間的存取權的授權應該會套用指定的中央存取原則指向每個檔案伺服器上的檔案。  
  
下圖示範如何將原則結合成原則清單來集中控制存取檔案。  
  
![方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**圖 3 所示**結合原則  
  
## <a name="in-this-scenario"></a>本案例中  
下列指導方針可供您中央存取原則：  
  
-   [計劃的中央存取原則部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [部署的中央存取原則與 #40; 示範步驟和 #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>角色與包含在本案例中的功能  
下表列出的角色與本案例的功能，並告訴他們支援的方式。  
  
|角色/功能|它如何支援此案例|  
|-----------------|---------------------------------|  
|Active Directory Domain Services 角色|Windows Server 2012 中的 AD DS 導入宣告為基礎的授權平台，可讓使用者宣告和裝置宣告、複合的身分、（使用者加上裝置宣告），建立新的中央存取原則（端點）型號，並授權決策檔案分類資訊的使用。|  
|檔案和存放區服務伺服器角色|檔案與儲存空間服務提供技術可協助您設定及管理您的網路位置您可以將檔案儲存並分享的使用者提供中央位置的一或多個檔案伺服器。 若您的網路使用者的存取權的相同的檔案和應用程式，或如果備份與檔案的集中的管理您的組織重要，您應該設定一或多部電腦做為檔案伺服器來將檔案與儲存空間服務角色與適當的角色服務新增到電腦。|  
|Windows client 電腦|透過 client 電腦的使用者可以存取檔案及網路上的資料夾。|  
  


