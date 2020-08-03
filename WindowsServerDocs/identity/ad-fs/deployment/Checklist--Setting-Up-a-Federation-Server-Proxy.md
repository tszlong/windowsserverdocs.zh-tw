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
ms.openlocfilehash: 0ffb9a33fa5c06c9fa79846c21937ffddb2776cd
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520017"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>檢查清單：設定同盟伺服器 Proxy

此檢查清單包含在 &reg; Active Directory 同盟服務 AD FS 中為同盟伺服器 proxy 角色準備執行 Windows server 2012 之伺服器的部署 \( 工作 \) 。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![設定同盟 proxy 伺服器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定同盟伺服器 proxy**

|Task|參考|
|--------|-------------|
|開始部署 AD FS 同盟伺服器 proxy 之前，請先參閱 AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![設定同盟 proxy 伺服器會](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![設定聯合 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器 Proxy 位置](../design/planning-federation-server-proxy-placement.md)<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[要放置同盟伺服器 proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807048(v=ws.11))的聯盟 proxy 伺服器|
|請參閱 AD FS 容量規劃指南，以判斷您應該在生產環境中使用的適當同盟伺服器 proxy 數目。|![設定同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器 Proxy 容量](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg749898(v=ws.11))|
|判斷單一的同盟伺服器 proxy 或同盟伺服器 proxy 伺服器陣列是否較適合您的部署。 **注意：** 同盟伺服器也會執行同盟伺服器 proxy 的責任。|![設定同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[以建立同盟伺服器 proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807032(v=ws.11))<p>![設定同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[以建立同盟伺服器 proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807082(v=ws.11))伺服器陣列|
|判斷是否要在帳戶夥伴組織或資源夥伴組織的周邊網路中建立這個新的同盟伺服器 proxy。|![設定同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查帳戶夥伴中的同盟伺服器 Proxy 角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807109(v=ws.11))<p>![設定同盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查資源夥伴中的同盟伺服器 Proxy 角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807052(v=ws.11))|
|在將成為同盟伺服器 proxy 的電腦上安裝 AD FS 之前，請閱讀取得伺服器驗證憑證的重要性—針對同盟伺服器 proxy 伺服器陣列，在伺服器陣列中的所有伺服器上新增或共用憑證。|![設定同盟伺服器 proxy 的聯盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[證書需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807054(v=ws.11))|
|請參閱《 AD FS 設計指南》中的資訊，瞭解如何更新 \( 周邊網路中的網域名稱系統 DNS， \) 讓同盟伺服器和同盟伺服器 proxy 的成功名稱解析能夠發生。|![設定同盟伺服器 proxy 的聯盟 proxy 伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))|
|判斷同盟伺服器 proxy 是否必須加入網域。 雖然同盟伺服器 proxy 不需要加入網域，但是在加入網域時，使用遠端系統管理和群組原則功能就可以更輕鬆地進行管理。|![設定同盟 proxy 伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|
|視您周邊網路中的 DNS 基礎結構的設定方式而定，在您的組織中部署同盟伺服器 proxy 之前，請先完成右側主題中的其中一個程式。 **注意：** 請勿執行這兩個程式。 請參閱[同盟伺服器 proxy 的名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))，以判斷哪一個程式最適合貴組織的需求。|![](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在僅服務周邊網路的 DNS 區域中設定同盟伺服器 proxy 的名稱解析](./configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)<p>![](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在同時提供周邊網路和網際網路用戶端的 DNS 區域中設定同盟伺服器 proxy 的名稱解析](./configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md)|
|取得伺服器驗證憑證之後，您必須將它安裝在 \( \) 同盟伺服器 proxy 預設網站的 Internet Information Services IIS 中。|![設定同盟 proxy 伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入到預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|
|\(選擇性 \) ：若要從憑證授權單位單位 CA 取得伺服器驗證憑證 \( \) ，您可以使用 IIS 取得同盟伺服器 proxy 的範例憑證。<p>因為 IIS \- 會產生不是來自受信任來源的自我簽署憑證，所以 \- 只在下列案例中使用它來建立自我簽署憑證：<p>-當您必須在 \( \) 伺服器與有限的已知使用者群組之間建立安全通訊端層 SSL 通道時<br />-當您必須針對協力廠商憑證問題進行疑難排解時，請 \- **注意：** 在生產環境中使用自我 \- 簽署的伺服器驗證憑證來部署同盟伺服器 proxy 並不是安全性最佳作法。|![設定同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立自我 \- 簽署的伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)|
|在即將成為同盟伺服器 Proxy 的電腦上安裝同盟服務 Proxy 角色服務。|![設定同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務 Proxy 角色服務](Install-the-Federation-Service-Proxy-Role-Service.md)|
|使用 [AD FSFederation 伺服器 Proxy 設定] Wizard，將電腦上的 AD FS 軟體設定為要在同盟伺服器 proxy 角色中作用。|![設定同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[為同盟伺服器 Proxy 角色設定電腦](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|
|使用事件檢視器確認同盟伺服器 Proxy 服務是否已啟動。|![設定同盟 proxy 伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[驗證同盟伺服器 Proxy 是否可運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|
