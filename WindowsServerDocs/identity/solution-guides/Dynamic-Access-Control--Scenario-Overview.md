---
ms.assetid: 7b22adfa-298d-413c-88d0-1231825b7d4d
title: "動態存取控制案例概觀"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f70bd138a16b23f1fbf7bfabc98ee184e3b9ab37
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="dynamic-access-control-scenario-overview"></a>動態存取控制：案例概觀

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012，您可以將資料控管套用所有檔案伺服器控制人可以存取的資訊和稽核：已存取的資訊。 動態存取控制可讓您：  
  
-   使用自動和手動分類檔案找出資料。 例如，您可能會檔案伺服器標記資料跨組織。  
  
-   控制檔案套用安全網路的原則，使用中央存取原則。 例如，您可能會定義人可以存取健康在組織中的資訊。  
  
-   稽核檔案的存取權的相容性報告和法庭分析使用中央稽核原則。 例如，您可能會找出使用者存取高機密資訊。  
  
-   套用 Rights Management Services (RMS) 保護的機密 Microsoft Office 文件使用自動 RMS 加密設定。 例如，您可能會設定 RMS 加密所有文件包含健康保證移植性與責任動作 (HIPAA) 資訊。  
  
動態存取控制功能設定為基礎的基礎結構投資可使用進一步的合作夥伴和業務的應用程式，並針對功能可以提供的組織使用 Active Directory 變得更好的值。 這個基礎結構包含：  
  
-   適用於 Windows 的可以處理條件運算式和中央原則新授權與稽核引擎。  
  
-   使用者宣告和裝置宣告 Kerberos 驗證支援。  
  
-   改進檔案分類基礎結構 (FCI)。  
  
-   RMS 擴充性支援讓合作夥伴可以提供方案的非 Microsoft 檔案加密。  
  
## <a name="in-this-scenario"></a>本案例中  
下列案例和指導方針是此內容集的一部分：  
  
## <a name="BKMK_APP"></a>動態存取控制內容藍圖  
  
|案例|評估|計劃|部署|運作|  
|------------|------------|--------|----------|-----------|  
|**案例：中央存取原則**<br /><br />建立檔案允許組織集中部署與管理授權原則，包括條件運算式使用使用者宣告、裝置宣告和資源屬性的中央存取原則。 這些原則 compliance 和商務法規為基礎。 這些原則建立和裝載在 Active Directory，因此讓它更容易管理和部署。<br /><br />**跨樹系部署宣告**<br /><br />在 Windows Server 2012，AD DS 每個森林中的 '宣告字典' 和所有取得定義類型使用樹系的 Active Directory 森林層級。 有許多主體可能需要往返信任邊界的案例。 本案例告訴您如何理賠要求穿過信任邊界。|[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)<br /><br />[跨樹系部署宣告](Deploy-Claims-Across-Forests.md)|[規劃：中央存取原則部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br /><br />-   [商務用要求對應至的中央存取原則程序](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_1)<br />-   [動態存取控制的管理委派](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.1)<br />-   [規劃中央存取原則例外機制](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.2)<br /><br />使用使用者宣告最佳做法<br /><br />-   [選擇的正確設定，可讓您的使用者網域中的宣告](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DC_OP3)<br />-   [若要讓使用者宣告作業](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.4.2)<br />-   [使用該檔案伺服器使用者宣告考量任意 Acl 不使用的中央存取原則](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_5)<br /><br />[使用裝置宣告和裝置安全性群組](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DeviceClaims)<br /><br />-   [使用靜態裝置宣告注意事項](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.1)<br />-   [若要讓裝置宣告作業](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.3)<br /><br />部署工具<br /><br />-   [資料分類工具組](https://go.microsoft.com/fwlink/?LinkId=%20244300)|[部署的中央存取原則與 #40; 示範步驟和 #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)<br /><br />[跨樹系與 #40; 示範步驟和 #41; 部署宣告](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)|-建模的中央存取原則|  
|**案例：檔案存取稽核**<br /><br />安全性稽核是以企業的安全性維持最有力的工具之一。 安全性稽核的主要目標是法規。 例如，業界標準沙法案、HIPAA，和付款卡片 Industry (PCI) 需要遵循嚴格組規則的相關資料的安全性和隱私權的企業。 安全性稽核協助建立是否存在的此類原則。因此，它們證明符合或使用這些標準不符合。 此外，安全性稽核協助偵測異常行為，找出並減少縫隙中的安全性原則，並建立的使用者活動，可用於法庭分析記錄阻止 irresponsible 行為。|[案例：檔案存取稽核](Scenario--File-Access-Auditing.md)|[檔案計劃存取稽核](Plan-for-File-Access-Auditing.md)|[部署安全性稽核中央稽核原則和 #40; 示範步驟和 #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)|-   [監視器套用檔案伺服器的中央存取原則](https://technet.microsoft.com/library/jj574188.aspx)<br />-   [監視器的檔案和資料夾相關聯的中央存取原則](https://technet.microsoft.com/library/jj574198.aspx)<br />-   [監視器上的檔案和資料夾的資源屬性](https://technet.microsoft.com/library/jj574208.aspx)<br />-   [監視器宣告類型](https://technet.microsoft.com/library/jj574086.aspx)<br />-   [在登入時監視使用者和裝置宣告](https://technet.microsoft.com/library/jj574082.aspx)<br />-   [監視器中央存取原則和定義規則](https://technet.microsoft.com/library/jj574115.aspx)<br />-   [監視器的資源屬性定義](https://technet.microsoft.com/library/jj574155.aspx)<br />-   [監視抽取式存放裝置使用](https://technet.microsoft.com/library/jj574128.aspx)。|  
|**案例：存取的協助**<br /><br />今天使用者嘗試存取遠端伺服器上的檔案檔案，將會被的唯一指示時，存取。 這會產生支援請求或 IT 系統管理員必須找出問題，且經常系統管理員很難使用者取得適當的操作讓它更難征服的山峰修正問題的相關。 <br />Windows Server 2012，目標是試用，並協助處理拒絕之前 IT 取得問題存取的資訊背景工作和企業資料擁有者相關及何時 IT 取得參與，提供快速解析度所有正確的資訊。 在達成這個目標挑戰之一是處理拒絕中央無法與每個應用程式獨享優惠它以不同的方式與因此在 Windows Server 2012 的目標是改進 Windows 檔案總管] 存取的體驗。|[案例：存取的協助](Scenario--Access-Denied-Assistance.md)|[規劃存取的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)<br /><br />-   [判斷型號存取的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.1)<br />-   [判斷使用者應該處理存取要求](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_12)<br />-   [自訂訊息存取的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_13)<br />-   [例外計劃](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.4)<br />-   [判斷如何存取的協助部署](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.5)|[部署存取的協助與 #40; 示範步驟和 #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)||  
|**Office 文件案例：分類型加密**<br /><br />保護的機密資訊是主要緩和組織的風險。 各種 compliance 規範，例如 HIPAA 或付款卡業界資料安全標準 (PCI-DSS) 聽寫加密的詳細資訊，並有許多企業原因加密機密資訊。 不過，資訊加密很高，或其可能會影響生產力。 因此，組織通常會有不同的方式與加密其資訊的優先順序。 <br />若要支援此案例，Windows Server 2012 提供敏感根據其分類的 Windows Office 檔案加密的能力。 這是透過叫用 Active Directory 授權管理伺服器 (AD RMS) 保護的機密文件幾秒後檔案被視為敏感的檔案，檔案伺服器上的檔案管理工作。|[Office 文件案例：分類型加密](Scenario--Classification-Based-Encryption-for-Office-Documents.md)|[想要部署的分類加密的文件](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)|[部署加密的 Office 檔案和 #40; 示範步驟和 #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)||  
|**案例︰ 使用分類取得深入了解您的資料**<br /><br />傳送的資料與儲存空間資源仍持續增加中重要性大部分的組織。 IT 系統管理員必須面對越來越要求的作業時同時負責確保擁有成本總責任與維護合理的層級的更大、更複雜的儲存空間基礎結構。 管理儲存空間資源不幾乎可用性的資料，但有關執法公司原則和了解如何為了讓有效率使用量和降低風險 compliance 耗用儲存空間的磁碟區。 檔案分類基礎結構提供深入了解您的資料，自動執行分類程序，以便您可以更有效率地管理您的資料。 使用檔案分類基礎結構分類下列方法可：手動，以程式設計方式，並自動。 本案例焦某自動檔案分類方法。|[案例︰ 使用分類取得深入了解您的資料](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)|[自動檔案分類計劃](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)|[部署自動檔案分類與 #40; 示範步驟和 #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)||  
|**案例︰ 檔案伺服器實作保留的資訊**<br /><br />保留期間是應該文件的時間量保持之前已經過期。 根據組織，可以不同的保留時間。 您可以分類有簡短、中或長期保留期間的資料夾中的檔案，並再每段指定時間範圍。 若要將它放法律保留無限期保留檔案。 <br />檔案分類基礎結構和檔案伺服器資源管理員使用檔案管理工作和檔案分類套用保留期間的準則一組的檔案。 您可以保留期間指定資料夾，並設定指派的保留期間的到最後一個使用的檔案管理工作。 即將到期的資料夾中的檔案時，該檔案的擁有者取得通知的電子郵件。 您也可以將分類成法律保留檔案管理工作不會到期檔案，檔案。|[案例︰ 檔案伺服器實作保留的資訊](Scenario--Implement-Retention-of-Information-on-File-Servers.md)|[保持檔案伺服器的詳細資訊的計劃](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)|[部署上檔案伺服器與 #40; 示範步驟和 #41; 實作保留的資訊](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)||  
  
> [!NOTE]  
> 不支援動態存取控制 ReFS（復原檔案系統）。  
  
## <a name="BKMK_LINKS"></a>也了  
  
|內容類型|資訊尋找參考資料|  
|----------------|--------------|  
|**Product 評估**|-   [動態存取控制檢閱指南](https://go.microsoft.com/fwlink/?LinkId=244309)<br />-   [動態存取控制開發人員指南](https://go.microsoft.com/fwlink/?LinkId=245870)|  
|**規劃**|-   [規劃中央存取原則部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br />-   [檔案計劃存取稽核](Plan-for-File-Access-Auditing.md)|  
|**部署**|-   [Active Directory 部署](https://go.microsoft.com/fwlink/p/?LinkID=238318)<br />-   [檔案與儲存空間服務部署](https://go.microsoft.com/fwlink/?LinkID=24430)|  
|**作業**|[動態存取控制 PowerShell 參考資料](https://go.microsoft.com/fwlink/?LinkId=243150)|  
|**工具和設定**|[資料分類工具組](https://go.microsoft.com/fwlink/?LinkID=244300)|  
|**社群資源**|[Directory 服務論壇](https://social.technet.microsoft.com/Forums/winserverDS/threads)|  
  


