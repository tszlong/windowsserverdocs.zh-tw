---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 檢查清單-設定同盟伺服器 Proxy
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 43fb8db4bf494ded93e592a1346a9ba99f0aff55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359932"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>檢查清單：設定同盟伺服器 Proxy

此檢查清單包含在 Active Directory 同盟服務 @no__t 0AD FS @ no__t-1 中為同盟伺服器 proxy 角色準備執行 Windows Server®2012之伺服器的部署工作。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![setting 同盟 proxy 伺服器 @ no__t-1Checklist：設定同盟伺服器 proxy @ no__t-0  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|開始部署 AD FS 同盟伺服器 proxy 之前，請先參閱 AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![setting 同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![setting 聯合 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器 Proxy 位置](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[要放置同盟伺服器 proxy](https://technet.microsoft.com/library/dd807048.aspx)的聯盟 proxy 伺服器|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指南，以判斷您應該在生產環境中使用的適當同盟伺服器 proxy 數目。|![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[針對同盟伺服器 Proxy 容量規劃的](https://technet.microsoft.com/library/gg749898.aspx)聯盟 proxy 伺服器|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|判斷單一的同盟伺服器 proxy 或同盟伺服器 proxy 伺服器陣列是否較適合您的部署。 **注意：** 同盟伺服器也會執行同盟伺服器 proxy 的責任。|@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在建立同盟伺服器 Proxy 時](https://technet.microsoft.com/library/dd807032.aspx)，0setting 聯合 proxy 伺服器<br /><br />@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[當建立同盟伺服器 proxy](https://technet.microsoft.com/library/dd807082.aspx)伺服器陣列時，0setting|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|判斷是否要在帳戶夥伴組織或資源夥伴組織的周邊網路中建立這個新的同盟伺服器 proxy。|@no__t-向上0setting 同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查帳戶夥伴中的同盟伺服器 Proxy 角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />@no__t-向上0setting 同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查資源夥伴中的同盟伺服器 Proxy 角色](https://technet.microsoft.com/library/dd807052.aspx)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|在將成為同盟伺服器 proxy 的電腦上安裝 AD FS 之前，請閱讀取得伺服器驗證憑證的重要性—針對同盟伺服器 proxy 伺服器陣列，在伺服器陣列中的所有伺服器上新增或共用憑證。|![setting 同盟伺服器 proxy 的同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[證書需求](https://technet.microsoft.com/library/dd807054.aspx)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 設計指南中的資訊，瞭解如何在周邊網路中更新網域名稱系統 \(DNS @ no__t-1，以便同盟伺服器和同盟伺服器 proxy 的成功名稱解析也可能發生。|![setting 同盟伺服器 proxy 的同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|判斷同盟伺服器 proxy 是否必須加入網域。 雖然同盟伺服器 proxy 不需要加入網域，但是在加入網域時，使用遠端系統管理和群組原則功能就可以更輕鬆地進行管理。|![setting 同盟 proxy 伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|視您周邊網路中的 DNS 基礎結構的設定方式而定，在您的組織中部署同盟伺服器 proxy 之前，請先完成右側主題中的其中一個程式。 **注意：** 請勿執行這兩個程式。 請參閱[同盟伺服器 proxy 的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)，以判斷哪一個程式最適合貴組織的需求。|![setting 同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在僅服務周邊網路的 DNS 區域中設定同盟伺服器 proxy 的名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![setting 同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在同時提供周邊網路和網際網路用戶端的 DNS 區域中設定同盟伺服器 proxy 的名稱解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|取得伺服器驗證憑證之後，您必須將它安裝在同盟伺服器 proxy 預設網站上的 Internet Information Services \(IIS @ no__t-1 中。|@no__t-向上0setting 同盟 proxy 伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入到預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 做為從憑證授權單位單位取得伺服器驗證憑證的替代 \(CA @ no__t-3，您可以使用 IIS 來取得同盟伺服器 proxy 的範例憑證。<br /><br />因為 IIS 會產生不是來自受信任來源的自我 @ no__t-0signed 憑證，所以請只在下列案例中使用它來建立自我 @ no__t 1signed 憑證：<br /><br />-當您必須在伺服器與有限的已知使用者群組之間建立安全通訊端層 \(SSL @ no__t-1 通道<br />-當您必須針對第三個 @ no__t-0party 憑證問題進行疑難排解時，請**注意：** 使用自我 @ no__t-0signed 伺服器驗證憑證，在生產環境中部署同盟伺服器 proxy 並不是安全性最佳作法。|![setting 同盟 proxy 伺服器 @ no__t-1IIS：建立自我 @ no__t-0Signed 伺服器憑證 @ no__t-1|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器 Proxy 的電腦上安裝同盟服務 Proxy 角色服務。|![setting 同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務 Proxy 角色服務](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|使用 [AD FSFederation 伺服器 Proxy 設定] Wizard，將電腦上的 AD FS 軟體設定為要在同盟伺服器 proxy 角色中作用。|@no__t-向上0setting 同盟 proxy 伺服器設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器 Proxy 角色的電腦](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![設定同盟 proxy 伺服器](media/icon_checkboxo.gif)|使用事件檢視器確認同盟伺服器 Proxy 服務是否已啟動。|@no__t-向上0setting 同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[，確認同盟伺服器 Proxy 可運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
