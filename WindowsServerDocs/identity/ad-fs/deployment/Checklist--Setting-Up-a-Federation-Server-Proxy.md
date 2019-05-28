---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 檢查清單-設定同盟伺服器 Proxy
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8cd5cbe2ce0c7985a56f8444edfa7c71ee17c2d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192354"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>檢查清單：設定同盟伺服器 Proxy

此檢查清單包含準備同盟伺服器 proxy 角色，在 Active Directory Federation Services 中執行 Windows Server® 2012年的伺服器所需的部署工作\(AD FS\)。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定同盟的 proxy 伺服器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定同盟伺服器 proxy**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|在開始部署 AD FS 同盟伺服器 proxy 之前，請檢閱 AD FS 部署拓撲類型及其相關聯的伺服器放置地點和網路配置建議。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 Federation Server Proxy 的位置](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何處放置同盟伺服器 Proxy](https://technet.microsoft.com/library/dd807048.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|檢閱 AD FS 容量規劃指引，以判斷您應該使用您的生產環境中的同盟伺服器 proxy 的適當數目。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器 Proxy 容量](https://technet.microsoft.com/library/gg749898.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|判斷是否適合您的部署單一的同盟伺服器 proxy 或同盟伺服器 proxy 伺服器陣列。 **注意：** 同盟伺服器也執行同盟伺服器 proxy 的責任。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立同盟伺服器 Proxy 的時機](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[When to Create a Federation Server Proxy Farm](https://technet.microsoft.com/library/dd807082.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|判斷是否要此新的同盟伺服器 proxy 建立的帳戶夥伴組織或資源夥伴組織周邊網路中。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢閱帳戶夥伴中的同盟伺服器 proxy 角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢閱資源夥伴中的同盟伺服器 proxy 角色](https://technet.microsoft.com/library/dd807052.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器 proxy 電腦上安裝 AD FS 之前，請閱讀有關取得伺服器驗證憑證的重要性 — 同盟伺服器 proxy 陣列 — 新增，或在伺服陣列中的所有伺服器之間共用的憑證。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器 Proxy 的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|檢閱有關如何更新網域名稱系統中的 AD FS 設計指南\(DNS\)在周邊網路，因此該同盟伺服器和同盟伺服器 proxy 的成功名稱解析可能會發生。|![設定同盟的 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|判斷是否必須加入網域的同盟伺服器 proxy。 雖然同盟伺服器 proxy 不需要加入網域，它們是您更輕鬆地加入網域時，使用遠端系統管理及群組原則功能。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|根據您的周邊網路中的 DNS 基礎結構的設定方式，完成其中一種在右邊的主題中的程序之前在組織中部署同盟伺服器 proxy。 **注意：** 不會執行兩個程序。 讀取[同盟伺服器 Proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)來判斷哪一個程序最適合貴組織的需求。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定 DNS 區域，可僅對周邊網路中的同盟伺服器 Proxy 的名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定 DNS 區域，可同時在周邊網路和網際網路用戶端中的同盟伺服器 Proxy 的名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|取得伺服器驗證憑證之後，您就必須將它安裝在 Internet Information Services \(IIS\)預設網站上的同盟伺服器 proxy。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|\(選擇性\)做為憑證授權單位取得伺服器驗證憑證的替代\(CA\)，您可以使用 IIS 來為您的同盟伺服器 proxy 取得範例憑證。<br /><br />因為 IIS 會產生自我\-不是來自受信任的來源，使用它來建立自我簽署的憑證\-簽署憑證僅在下列情況：<br /><br />-當您必須建立安全通訊端層\(SSL\)伺服器與使用者有限，已知群組之間的通道<br />-當您需要疑難排解第三\-合作對象憑證問題**注意：** 它不是安全性最佳作法來部署同盟伺服器 proxy 在生產環境中使用自我\-已簽署的伺服器驗證憑證。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS:建立自我\-簽署伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器 proxy 電腦上安裝 Federation Service Proxy 角色服務。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝 Federation Service Proxy 角色服務](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|設定 AD FS 軟體以扮演同盟伺服器 proxy 角色使用 AD FSFederation 伺服器 Proxy 設定精靈在電腦上。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定同盟伺服器 Proxy 角色的電腦](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![設定同盟的 proxy 伺服器](media/icon_checkboxo.gif)|使用事件檢視器確認同盟伺服器 Proxy 服務是否已啟動。|![設定同盟的 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[確認同盟伺服器 Proxy 是否正確運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
