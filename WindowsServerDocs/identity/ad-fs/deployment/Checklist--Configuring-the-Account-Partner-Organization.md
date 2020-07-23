---
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: 檢查清單-設定帳戶夥伴組織
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 646612eec225f9e6e2847d9aa8c3ded588d550f2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966380"
---
# <a name="checklist-configuring-the-account-partner-organization"></a>檢查清單：設定帳戶夥伴組織

帳戶夥伴組織包含的使用者將會存取 \- 資源夥伴中的 Web 應用程式。 此組織中的系統管理員必須使用 [AD FS 管理] 嵌入式管理單元 \- 來建立信賴憑證者信任，以代表與資源夥伴組織的信任關係。 接著，資源夥伴系統管理員必須為他們想要信任的每個帳戶夥伴組織建立宣告提供者信任。  
  
此檢查清單包含 \( \) 在帳戶夥伴組織中部署 Active Directory 同盟服務 AD FS 的工作。 其中也包括設定建立一 \- 一半同盟合作關係所需元件的工作。  
  
如果您要部署[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))，則不需要遵循這份檢查清單。 不過，您必須完成此檢查清單中的工作，才能成功部署同盟[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))。  
  
> [!IMPORTANT]  
> 請確定資源夥伴組織中的系統管理員遵循[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)中的指導方針，以確保所有必要的部署工作都將完成，以成功建立同盟合作關係的後半個部分。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定帳戶夥伴組織](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：配置帳戶夥伴組織**  
  
||工作|參考|  
|-|--------|-------------|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果您目前的生產環境中有現有的 AD FS 1.0 或1.1 部署，請參閱右邊的連結，以取得如何將設定從目前的同盟服務遷移至新 AD FS 同盟服務的資訊。 如果您是第一次使用 AD FS 在組織中部署 AD FS，則可以略過此步驟，並繼續進行此檢查清單中的下一項工作，以取得如何設定新帳戶夥伴組織的相關資訊。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃將遷移至 AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff678044(v=ws.10))|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|根據您的部署目標，檢查為使用者提供同盟應用程式存取所需元件的相關資訊。|![設定帳戶夥伴組織為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供您的宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807071(v=ws.11))<p>![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[為您的 Active Directory 使用者提供其他組織的應用程式和服務存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807123(v=ws.11))<p>![設定帳戶夥伴組織為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[另一個組織中的使用者提供您的宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807099(v=ws.11))|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|判斷此帳戶夥伴組織將與哪個 AD FS 設計相關聯。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))<p>![設定帳戶夥伴組織同盟](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|開始部署 AD FS 伺服器之前，請先參閱;1. \) 選擇 Windows 內部資料庫 \( WID \) 或 SQL Server 儲存 AD FS 設定資料庫2的優點和缺點。 \)AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指導方針，以判斷您應該在生產環境中使用的適當同盟伺服器和同盟伺服器 proxy 伺服器數目。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 伺服器容量的](../design/planning-for-ad-fs-server-capacity.md)帳戶夥伴組織規劃|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|若要有效規劃及執行帳戶夥伴部署的實體拓撲，請判斷您的 AD FS 設計是否需要一或多部同盟伺服器或同盟伺服器 proxy。|![設定帳戶夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<p>![設定帳戶夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|決定您想要新增至 AD FS 的屬性存放區類型。 然後，使用 AD FS 管理] 嵌入式管理單元來新增屬性存放區 \- 。|![為帳戶夥伴組織設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<p>![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性存放區](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果您需要將宣告傳送至使用 AD FS 1.0 或1.1 同盟服務的資源夥伴或從中取用宣告，請參閱右邊的連結，以取得如何設定 AD FS 與舊版 AD FS 交互操作的資訊。 如果資源夥伴組織也使用 AD FS 來傳送或使用您組織的宣告，您可以略過此步驟，並繼續進行此檢查清單中的下一項工作。|![為 AD FS 1.x 的](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))設定帳戶夥伴組織規劃|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|在帳戶夥伴組織中部署第一部同盟伺服器之後，請使用 [AD FS 管理] 嵌入式管理單元來建立信賴憑證者信任關係 \- 。 您可以手動輸入資源夥伴的資料或使用資源夥伴組織系統管理員提供給您的同盟中繼資料 URL，建立信賴憑證者信任。 您可以使用同盟中繼資料自動擷取資源夥伴的資料。 **注意：** 如果資源夥伴發佈其同盟中繼資料，或可以提供檔案複本供您使用，我們建議您自動抓取資料，因為它可以節省時間。|![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任<p>![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用同盟中繼資料建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|視組織的需求而定，針對 [AD FS 管理] 嵌入式管理單元中指定的每個信賴憑證者信任建立一或多個宣告規則集， \- 以便適當地發行宣告。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查清單：建立信賴憑證者信任的宣告規則](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果尚未存在可滿足貴組織需求的宣告描述，則必須加以建立。 AD FS 隨附一組預設的宣告描述，會在 AD FS 管理嵌入式管理單元 \- 中公開。|![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|判斷您的組織是否需要使用身分識別委派來授權或限制指定的帳號「採取行動」或模擬其他使用者。 當前端 \- web 應用程式必須與後 \- 端 web 服務互動時，這通常是一項需求。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用身分識別委派的](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807122(v=ws.11))時機|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|準備用戶端電腦以進行同盟：<p>-將帳戶夥伴同盟伺服器的 URL 新增至用戶端瀏覽器的信任的網站清單。<br />-使用群組原則將適當的安全通訊端層 \( SSL \) 憑證推送至用戶端電腦。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在帳戶夥伴中準備用戶端電腦](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807114(v=ws.11))<p>![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[設定用戶端電腦以信任帳戶同盟伺服器](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<p>![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用群組原則將憑證散發到用戶端電腦](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)| 
