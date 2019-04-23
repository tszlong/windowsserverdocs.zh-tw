---
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: 檢查清單-設定帳戶夥伴組織
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: df8057bf8afb51cbd9ca2ec704144b5863bdf064
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878979"
---
# <a name="checklist-configuring-the-account-partner-organization"></a>檢查清單：設定帳戶夥伴組織

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

帳戶夥伴組織包含會存取 Web 的使用者\-資源夥伴中的應用程式。 此組織中的系統管理員必須使用 AD FS 管理嵌入式管理單元\-中建立信賴憑證者信任來代表資源夥伴組織其信任關聯性。 接著，資源夥伴系統管理員必須建立每個帳戶夥伴組織，他們想要信任的宣告提供者信任。  
  
這份檢查清單包含針對部署 Active Directory Federation Services 工作\(AD FS\)帳戶夥伴組織中。 它也包含設定的元件所需建立一個工作\-同盟合作關係的下半部。  
  
如果您要部署[網頁 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)，您就不必在遵循這份檢查清單。 不過，您需要完成此檢查清單，若要成功部署中的工作[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 請確定資源夥伴組織中的系統管理員遵循中的指導方針[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)以確保會在成功地同盟合作關係的下半部建立第二個已完成所有必要的部署工作。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定帳戶夥伴組織](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定帳戶夥伴組織**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果目前生產環境中有現有的 AD FS 1.0 或 1.1 部署，請參閱右邊，如需有關如何將您目前的同盟服務中的設定移轉至新的 AD FS 同盟服務資訊的連結。 如果您要在您的組織第一次部署 AD FS 使用 AD FS 中，您可以略過此步驟並繼續此檢查清單，如需如何設定新的帳戶夥伴組織中的下一個工作。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃移轉至 AD FS 2.0](https://technet.microsoft.com/library/ff678044.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|根據您的部署目標，請檢閱元件為使用者提供存取同盟應用程式所需的相關資訊。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供 Your Active Directory Users Access to Your Claims-aware Applications and Services](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Provide Your Active Directory Users Access 的應用程式和其他組織的服務](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供另一個組織存取您的宣告感知應用程式和服務的使用者](https://technet.microsoft.com/library/dd807099.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|判斷哪一個 AD FS 設計這個帳戶夥伴組織將會與相關聯。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟網頁 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|在開始部署 AD FS 伺服器之前，請檢閱;1.\)選擇其中一個 Windows 內部資料庫的優缺點\(WID\)或 SQL Server 來儲存 AD FS 設定資料庫 2。\)AD FS 部署拓撲類型和其相關聯的伺服器位置和網路配置建議。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|檢閱 AD FS 容量規劃指引，以判斷適當數目之同盟伺服器與同盟伺服器 proxy 伺服器，您應該使用您的生產環境中。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 伺服器容量](https://technet.microsoft.com/library/gg749899.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|若要有效規劃及實作帳戶夥伴部署的實體拓撲，判斷您的 AD FS 設計需要一或多個同盟伺服器或同盟伺服器 proxy。|![設定帳戶夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![設定帳戶夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|決定您想要新增至 AD FS 的屬性存放區的類型。 然後，新增 屬性存放區使用 AD FS 管理嵌入式管理單元\-中。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[角色的屬性存放區](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性存放區](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果您將需要將宣告傳送至或 AD FS 1.0 或 1.1 同盟服務使用來自使用者的 資源夥伴的宣告，請參閱連結的權限，如需如何設定 AD FS 與 AD FS 的舊版交互操作。 如果資源夥伴組織也使用 AD FS 來傳送，或使用您的組織宣告，您可以略過此步驟，並繼續進行此檢查清單中的下一個工作。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃互通性與 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|第一部同盟伺服器在帳戶夥伴組織中的部署之後，建立信賴憑證者的合作對象信任關係，使用 AD FS 管理嵌入式管理單元\-中。 您可以手動輸入資源夥伴的資料或使用資源夥伴組織系統管理員提供給您的同盟中繼資料 URL，建立信賴憑證者信任。 您可以使用同盟中繼資料自動擷取資源夥伴的資料。 **注意：** 如果資源夥伴發佈它的同盟中繼資料或可以提供它的檔案複本，建議您自動擷取資料，因為比較省時。|![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[信賴憑證者合作對象信任手動建立](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)<br /><br />![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立信賴憑證者合作對象信任使用同盟中繼資料](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|根據您組織的需求，建立一或多個宣告規則設定為每個信賴憑證者信任指定 AD FS 管理嵌入式管理單元中\-中，因此會適當地發出宣告。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢查清單：建立信賴憑證者信任宣告規則](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|如果不存在，就必須建立宣告描述的因應組織的需求。 AD FS 隨附一組預設的宣告描述會公開在 AD FS 管理嵌入式管理單元中\-中。|![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|決定您的組織是否需要使用授權，或限制指定的帳戶 」 做為 」 或模擬其他使用者的身分識別委派。 這通常是需求時 front\-結束的 Web 應用程式必須搭配 回到互動\-端 Web 服務。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[使用身分識別委派的時機](https://technet.microsoft.com/library/dd807122.aspx)|  
|![設定帳戶夥伴組織](media/icon_checkboxo.gif)|準備同盟的用戶端電腦：<br /><br />-新增至用戶端瀏覽器的信任的網站清單的帳戶夥伴同盟伺服器的 URL。<br />-使用群組原則，將適當的安全通訊端層\(SSL\)到用戶端電腦的憑證。|![設定帳戶夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[帳戶夥伴中準備用戶端電腦](https://technet.microsoft.com/library/dd807114(v=ws.11).aspx)<br /><br />![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[信任帳戶同盟伺服器設定用戶端電腦](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<br /><br />![設定帳戶夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[發佈的憑證，透過使用群組原則的用戶端電腦](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)| 
