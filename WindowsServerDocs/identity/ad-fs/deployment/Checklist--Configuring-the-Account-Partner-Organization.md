---
description: 深入瞭解：檢查清單：設定帳戶夥伴組織
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: 檢查清單-設定帳戶夥伴組織
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b37db54fdd997698625a64b856e42e8d55bd19f0
ms.sourcegitcommit: e2dadc9b0c227a489a945bbc531aca5e101f18cd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97801722"
---
# <a name="checklist-configuring-the-account-partner-organization"></a>檢查清單：設定帳戶夥伴組織

帳戶夥伴組織包含的使用者將存取 \- 資源夥伴中的 Web 應用程式。 此組織中的系統管理員必須使用 AD FS 管理] 嵌入式管理單元 \- ，建立信賴憑證者信任，以代表與資源夥伴組織的信任關係。 接著，資源夥伴系統管理員必須為他們想要信任的每個帳戶夥伴組織建立宣告提供者信任。

此檢查清單包含 \( \) 在帳戶夥伴組織中部署 Active Directory 同盟服務 AD FS 的工作。 它也包含設定建立 \- 同盟合作關係一半所需元件的工作。

如果您要部署 [網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))，則不需要遵循此檢查清單。 不過，您必須完成此檢查清單中的工作，才能成功部署同盟 [網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))。

> [!IMPORTANT]
> 請確定資源夥伴組織中的系統管理員遵循檢查清單中的指引 [：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md) ，以確保所有必要的部署工作都將完成，以成功建立第二部同盟合作關係。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![核取記號圖示，設定帳戶夥伴組織。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定帳戶夥伴組織**

|Task|參考|
|--------|-------------|
|如果您目前的生產環境中有現有的 AD FS 1.0 或1.1 部署，請參閱右側的連結，以取得如何將設定從目前的同盟服務遷移至新的 AD FS 同盟服務的資訊。 如果您在組織中第一次使用 AD FS 部署 AD FS，則可以略過此步驟，並繼續進行此檢查清單中的下一項工作，以取得如何設定新帳戶夥伴組織的相關資訊。|![圖示，請規劃遷移至 AD FS 2.0。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃遷移至 AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff678044(v=ws.10))|
|根據您的部署目標，請參閱提供使用者存取同盟應用程式所需之元件的相關資訊。|![圖示，為您的 AD 使用者提供對宣告感知應用程式的存取權。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807071(v=ws.11))<p>![圖示，為 AD 使用者提供應用程式和服務的存取權。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供其他組織的應用程式和服務存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807123(v=ws.11))<p>![圖示，為其他組織中的使用者提供您宣告感知應用程式和服務的能力。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[另一個組織中的使用者提供您宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807099(v=ws.11))|
|判斷這個帳戶夥伴組織將與哪個 AD FS 設計相關聯。|![圖示，網頁 SSO 設計。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))<p>![圖示，同盟網頁 SSO 設計。同盟](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))|
|開始部署 AD FS 伺服器之前，請先參閱：1. \) 選擇 Windows 內部資料庫 \( WID \) 或 SQL Server 來儲存 AD FS configuration Database 2 的優點和缺點。 \) AD FS 部署拓撲類型及其相關聯的伺服器放置和網路設定建議。|![圖示，判斷您的 AD FS 部署拓撲。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![圖示，AD FS 部署拓撲的考慮。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲的考慮](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))|
|請參閱 AD FS 容量規劃指引，以判斷您應在生產環境中使用的適當同盟伺服器和同盟伺服器 proxy 伺服器數目。|![圖示，規劃 AD FS server 容量。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS Server 容量](../design/planning-for-ad-fs-server-capacity.md)|
|若要有效地規劃和執行帳戶夥伴部署的實體拓撲，請判斷您的 AD FS 設計是否需要一或多部同盟伺服器或同盟伺服器 proxy。|![圖示，設定同盟伺服器。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<p>![圖示，設定同盟伺服器 proxy。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|決定您想要新增至 AD FS 的屬性存放區類型。 然後，使用 AD FS 管理] 嵌入式管理單元新增屬性存放區 \- 。|![圖示，也就是屬性存放區的角色。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<p>![圖示，加入屬性存放區。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性存放區](../../ad-fs/operations/Add-an-Attribute-Store.md)|
|如果您需要將宣告傳送到或取用使用 AD FS 1.0 或1.1 同盟服務之資源夥伴的宣告，請參閱右邊的連結，以取得如何設定 AD FS 與舊版 AD FS 交互操作的資訊。 如果資源夥伴組織也使用 AD FS 將宣告傳送或取用給您的組織，您可以略過此步驟，並繼續進行此檢查清單中的下一個工作。|![圖示，規劃與 AD FS 1.x 的互通性。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃與 AD FS 1.x 的互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|在帳戶夥伴組織中部署第一部同盟伺服器之後，請使用 AD FS 管理] 嵌入式管理單元，建立信賴憑證者信任關係 \- 。 您可以手動輸入資源夥伴的資料或使用資源夥伴組織系統管理員提供給您的同盟中繼資料 URL，建立信賴憑證者信任。 您可以使用同盟中繼資料自動擷取資源夥伴的資料。 **注意：** 如果資源夥伴發佈其同盟中繼資料，或可為您提供檔案複本供您使用，我們建議您自動取出資料，因為它可以節省時間。|![圖示，手動建立信賴憑證者信任。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任<p>![圖示，使用同盟中繼資料建立信賴憑證者信任。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用同盟中繼資料建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任|
|視組織的需求而定，請針對 AD FS 管理嵌入式管理單元中所指定的每個信賴憑證者信任建立一或多個宣告規則集， \- 以便適當地發出宣告。|![圖示，建立信賴憑證者信任的宣告規則。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查清單：建立信賴憑證者信任的宣告規則](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|
|如果您的宣告描述還不存在，就必須建立宣告描述，以滿足您組織的需求。 AD FS 隨附一組預設的宣告描述，會在 AD FS 管理嵌入式管理單元 \- 中公開。|![圖示，新增宣告描述。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|
|判斷您的組織是否需要使用身分識別委派，來授權或限制指定的帳號「作為」或模擬其他使用者。 這通常是前端 \- web 應用程式必須與後 \- 端 web 服務互動時的需求。|![圖示，請使用身分識別委派。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用身分識別委派的](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807122(v=ws.11))時機|
|準備用戶端電腦以進行同盟依據：<p>-將帳戶夥伴同盟伺服器的 URL 新增至用戶端瀏覽器的 [信任的網站] 清單。<br />-使用群組原則將適當的安全通訊端層 \( SSL \) 憑證推送至用戶端電腦。|![圖示，準備帳戶夥伴中的用戶端電腦。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[準備帳戶夥伴中的用戶端電腦](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807114(v=ws.11))<p>![圖示，設定用戶端電腦以信任帳戶同盟伺服器。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定用戶端電腦以信任帳戶同盟伺服器](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<p>![使用群組原則設定帳戶夥伴組織將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[憑證發佈至用戶端電腦](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)|
