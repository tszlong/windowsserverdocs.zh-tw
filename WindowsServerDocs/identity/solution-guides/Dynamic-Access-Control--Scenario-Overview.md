---
ms.assetid: 7b22adfa-298d-413c-88d0-1231825b7d4d
title: 動態存取控制案例概觀
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4d39ee3053133286c07a93d18b5c3bd5809e3b54
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182394"
---
# <a name="dynamic-access-control-scenario-overview"></a>動態存取控制：案例概觀

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012 中，您可以在整個檔案伺服器上套用資料管理，以控制可以存取訊號的人員，以及審查誰已存取訊號。 動態存取控制可讓您：

-   使用自動和手動檔案分類來識別資料。 例如，您可以標記整個組織內檔案伺服器中的資料。

-   透過套用使用集中存取原則的安全網原則，控制檔案的存取。 例如，您可以定義組織內誰可以存取健康情況資訊。

-   在更新狀態報告和鑑證分析使用集中稽核原則，可稽核檔案的存取。 例如，您可以識別哪些人存取了高度敏感的資訊。

-   為敏感的 Microsoft Office 文件使用自動 RMS 加密，套用 Rights Management Services (RMS) 保護。 例如，您可以設定 RMS 加密包含「健康保險流通與責任法案」(Health Insurance Portability and Accountability Act，HIPAA) 資訊的所有文件。

動態存取控制功能集是以可由合作夥伴及企業營運應用程式進一步使用的基礎結構投資為基礎，而這些功能可以為使用 Active Directory 的組織提供極大的價值。 這個基礎結構包括：

-   適用於 Windows 的新授權和稽核引擎，可處理條件運算式和集中原則。

-   Kerberos 驗證支援使用者宣告和裝置宣告。

-   對檔案分類基礎結構 (FCI) 的改進。

-   RMS 擴充性支援，讓合作夥伴可以提供加密非 Microsoft 檔案的解決方案。

## <a name="in-this-scenario"></a>在這個案例中
這個內容集包含下列案例和指導方針：

## <a name="dynamic-access-control-content-roadmap"></a><a name="BKMK_APP"></a>動態存取控制內容藍圖

|狀況|評估|計畫|部署|操作|
|------------|------------|--------|----------|-----------|
|**案例：集中存取原則**<p>為檔案建立集中存取原則，可讓組織集中部署和管理授權原則，這些原則包含使用使用者宣告、裝置宣告及資源屬性的條件運算式。 這些原則採用規範及商業法規要求。 這些原則是在 Active Directory 中建立並裝載在內，因此更便於管理及部署。<p>**跨樹系部署宣告**<p>在 Windows Server 2012 中，AD DS 會在每個樹系中維護「宣告字典」，而樹系內使用的所有宣告類型都是在 Active Directory 樹系層級定義的。 在許多案例中，主體可能需要周遊信任邊界。 這個案例說明宣告如何周遊信任邊界。|[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)<p>[跨樹系部署宣告](Deploy-Claims-Across-Forests.md)|[規劃：集中存取原則部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<p>-   [將商務要求對應到集中存取原則的流程](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_1)<br />-   [動態存取控制的管理委派](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.1)<br />-   [規劃集中存取原則的例外狀況機制](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.2)<p>使用使用者宣告的最佳做法<p>-   [選擇正確的設定以啟用使用者網域中的宣告](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DC_OP3)<br />-   [啟用使用者宣告的作業](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.4.2)<br />-   [在檔案伺服器中使用使用者宣告的考慮不使用集中存取原則的任意 Acl](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_5)<p>[使用裝置宣告和裝置安全性群組](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DeviceClaims)<p>-   [使用靜態裝置宣告的考慮](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.1)<br />-   [啟用裝置宣告的作業](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.3)<p>部署工具<p>-   [資料分類工具組](https://go.microsoft.com/fwlink/?LinkId=%20244300)|[部署集中存取原則 &#40;示範步驟&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)<p>[跨樹系部署宣告 &#40;示範步驟&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)|-模型化集中存取原則|
|**案例：檔案存取稽核**<p>安全性稽核是可以協助維護企業安全最有力的工具之一。 安全性稽核的其中一個關鍵目標就是法規相符性。 例如，業界標準 (像是沙氏法案、HIPAA 以及付款卡產業 (PCI)) 要求企業遵循一套與資料安全性和隱私權相關的嚴格規定。 安全性稽核可以協助確定您的企業中是否已經設定這些原則，因此，它們可以檢驗您的企業是否符合這些標準。 此外，安全性稽核還可以協助偵測異常行為、發現和補強安全性原則漏洞，以及透過建立使用者活動記錄 (可以用在資料安全分析上) 遏止不負責任的行為。|[案例：檔案存取稽核](Scenario--File-Access-Auditing.md)|[規劃檔案存取稽核](Plan-for-File-Access-Auditing.md)|[使用集中稽核原則部署安全性審核 &#40;示範步驟&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)|-   [監視檔案伺服器上套用的集中存取原則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574188(v=ws.11))<br />-   [監視與檔案和資料夾相關聯的集中存取原則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574198(v=ws.11))<br />-   [監視檔案和資料夾上的資源屬性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574208(v=ws.11))<br />-   [監視宣告類型](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574086(v=ws.11))<br />-   [在登入期間監視使用者和裝置宣告](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574082(v=ws.11))<br />-   [監視集中存取原則和規則定義](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574115(v=ws.11))<br />-   [監視資源屬性定義](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574155(v=ws.11))<br />-   [監視卸載式儲存裝置的使用](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574128(v=ws.11))。|
|**案例：拒絕存取時的協助**<p>目前，當使用者嘗試存取檔案伺服器上的遠端檔案時，會收到的唯一指示是拒絕存取。 這會向技術服務人員或 IT 系統管理員產生要求，而這些人員需要找出問題所在，但是通常他們很難從使用者了解整件事情的始末，因此讓解決問題變得更為困難。 <br />在 Windows Server 2012 中，目標是要嘗試並協助資料的資訊工作者和商務擁有者處理拒絕存取的問題，並在涉及時，提供所有正確的資訊以供快速解決。 達到此目標的其中一項挑戰，就是沒有集中的方式來處理拒絕存取，而且每個應用程式處理它的方式不同，因此在 Windows Server 2012 中，其中一個目標是改善 Windows Explorer 的拒絕存取使用經驗。|[案例：拒絕存取時的協助](Scenario--Access-Denied-Assistance.md)|[規劃拒絕存取時的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)<p>-   [判斷「拒絕存取時的協助」模型](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.1)<br />-   [判斷誰應處理存取要求](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_12)<br />-   [自訂拒絕存取時的協助訊息](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_13)<br />-   [規劃例外狀況](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.4)<br />-   [判斷如何部署拒絕存取時的協助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.5)|[部署拒絕存取時的協助 &#40;示範步驟&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)||
|**案例：依分類來加密 Office 文件**<p>保護敏感資訊主要是為組織免除風險。 各種標準規定 (如 HIPAA 或付款卡產業資料安全標準 (Payment Card Industry Data Security Standard，PCI-DSS)) 要求加密資訊，而且還有各種業務方面的原因需要加密敏感的商業資訊。 不過，加密資訊所費不貲，也可能會削弱企業生產力。 正因如此，所以組織傾向用不同的方式及優先順序來加密他們的資訊。 <br />為了支援此案例，Windows Server 2012 可讓您根據使用者的分類自動加密敏感性 Windows Office 檔案。 它的實現方式是透過檔案管理工作，當檔案伺服器上將檔案識別為敏感檔案之後的數秒內，就為這些敏感文件叫用 Active Directory Rights Management Server (AD RMS) 保護。|[案例：依分類來加密 Office 文件](Scenario--Classification-Based-Encryption-for-Office-Documents.md)|[依分類加密文件的部署規劃](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)|[部署 Office 檔案加密 &#40;示範步驟&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)||
|**案例：使用分類深入了解您的資料**<p>對大部分的組織來說，對資料及存放裝置資源的依賴變得越來越重要。 IT 系統管理員面對日益艱鉅的挑戰，不但要監看不斷擴充且愈加複雜的存放裝置基礎結構，同時還肩負了確保擁有權總成本維持在合理範圍內的責任。 管理存放裝置資源不再只限於資料的數量或可用性，現在還包含了強制執行公司政策，清楚知道存放裝置的使用方式，以便有效地利用它們及符合公司規範以消除風險。 檔案分類基礎結構透過自動化分類程序，讓您深入了解資料，以便更有效地管理資料。 檔案分類基礎結構提供下列分類方法：手動、透過程式設計以及自動。 這個案例著眼於自動檔案分類方法。|[案例：使用分類深入了解您的資料](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)|[規劃自動檔案分類](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)|[&#40;示範步驟部署自動檔案分類&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)||
|**案例：在檔案伺服器上實作資訊保留**<p>保留期限是文件到期之間應該被保留的時間。 保留期限取決於每個組織而有所不同。 您可以將資料夾中的檔案分類為短期、中期或長期保留期限，然後為每個期限指派一個時間範圍。 如果檔案適用法務保存措施，就可以無限期地保留檔案。 <br />檔案分類基礎結構及檔案伺服器資源管理員會使用檔案管理工作和檔案分類，對一組檔案套用保留期限。 您可以在資料夾上指派一個保留期限，然後使用檔案管理工作來設定所指派之保留期限的持續時間。 當資料夾中的檔案快要到期時，檔案的擁有人會收到一封通知電子郵件。 您也可以將檔案分類為適用法務保存措施，這樣檔案管理工作就不會讓檔案到期。|[案例：在檔案伺服器上實作資訊保留](Scenario--Implement-Retention-of-Information-on-File-Servers.md)|[在檔案伺服器上規劃資料保留](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)|[在檔案伺服器上部署資訊的實施保留 &#40;示範步驟&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)||

> [!NOTE]
> ReFS (復原檔案系統) 不支援動態存取控制。

## <a name="see-also"></a>另請參閱

|內容類型|參考|
|----------------|--------------|
|**產品評估**|- [動態存取控制審核者指南](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc732148(v=ws.10))<br>- [動態存取控制開發人員指導方針](https://docs.microsoft.com/previous-versions/windows/desktop/dacx/dynamic-access-control-developer-extensibility-roadmap)|
|**規劃**|- [規劃集中存取原則部署](https://docs.microsoft.com/windows-server/identity/solution-guides/scenario--central-access-policy)<br>- [規劃檔案存取審核](Plan-for-File-Access-Auditing.md)|
|**部署**|- [Active Directory 部署](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/ad-ds-deployment)<br>- [檔案和存放服務部署](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831487(v=ws.11))|
|**作業**|[動態存取控制 PowerShell 參考](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps)|
|**工具及設定**|[資料分類工具組](https://www.microsoft.com/download/details.aspx?id=27123)|
|**社群資源**|[目錄服務論壇](https://docs.microsoft.com/answers/topics/windows-active-directory.html)|
