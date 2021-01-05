---
description: 深入瞭解：檢查清單：設定資源夥伴組織
ms.assetid: 80d50a9f-428e-40fe-b6b3-9837fd9a3efc
title: 檢查清單-設定資源夥伴組織
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: f01f8509e211db31cc043da41b05766cedca8043
ms.sourcegitcommit: 3247e193d9fe1b57543fff215460a6d9db52f58b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97815000"
---
# <a name="checklist-configuring-the-resource-partner-organization"></a>檢查清單：設定資源夥伴組織

資源夥伴組織包含裝載 Web 應用程式的 Web 服務器 \- ，這些應用程式將由帳戶夥伴中的使用者存取。 此組織中的系統管理員必須使用 AD FS 管理] 嵌入式管理單元 \- 來建立宣告提供者信任，以代表與帳戶夥伴組織的信任關係。 接著，帳戶夥伴系統管理員必須為他們想要信任的每個帳戶夥伴組織建立信賴憑證者信任。

此檢查清單包含 \( \) 在資源夥伴組織中部署 Active Directory 同盟服務 AD FS 所需的工作。 它也包含設定建立 \- 同盟合作關係一半所需元件的工作。

如果您要部署 [網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))，則不需要遵循此檢查清單。 不過，您必須完成此檢查清單中的工作，才能成功部署同盟 [網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))。

> [!IMPORTANT]
> 請確定帳戶夥伴組織的系統管理員遵循檢查清單中的指導方針 [：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md) ，以確保所有必要的部署工作都將完成，以成功建立第二部同盟合作關係

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![設定資源夥伴組織檢查清單的核取記號圖示。](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定資源夥伴組織**

|Task|參考|
|--------|-------------|
|如果您目前的生產環境中有現有的 AD FS 1.0 或1.1 部署，請參閱右側的連結，以取得如何將設定從目前的同盟服務遷移至新的 AD FS 同盟服務的資訊。 如果您在組織中第一次使用 AD FS 部署 AD FS，則可以略過此步驟，並繼續進行此檢查清單中的下一項工作，以取得如何設定新的資源夥伴組織的相關資訊。|![規劃遷移至 AD FS 連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃遷移至 AD FS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff678044(v=ws.10))|
|根據您的部署目標，請參閱提供使用者存取同盟應用程式所需之元件的相關資訊。|![的圖示，可讓您的 Active Directory 使用者存取您的宣告感知應用程式和服務連結。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807071(v=ws.11))<p>![的圖示，可讓您的 Active Directory 使用者存取其他組織的應用程式和服務連結。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供其他組織的應用程式和服務存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807123(v=ws.11))<p>![的圖示，可讓其他組織中的使用者存取您的宣告感知應用程式和服務連結。為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[另一個組織中的使用者提供您宣告感知應用程式和服務的存取權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807099(v=ws.11))|
|判斷此資源夥伴組織將與哪個 AD FS 設計相關聯。|![Web SSO 設計連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807033(v=ws.11))<p>![同盟網頁 SSO 設計連結的圖示。同盟](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807050(v=ws.11))|
|檢查不同的應用程式類型，並決定要部署的應用程式。|![在資源夥伴連結中判斷同盟應用程式策略的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷資源夥伴中的同盟應用程式策略](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807077(v=ws.11))|
|開始部署 AD FS 伺服器之前，請先參閱：1. \) 選擇 Windows 內部資料庫 \( WID \) 或 SQL Server 來儲存 AD FS configuration Database 2 的優點和缺點。 \) AD FS 部署拓撲類型及其相關聯的伺服器放置和網路設定建議。|![[判斷您的 AD FS 部署拓撲] 連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![AD FS 部署拓撲考慮] 連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲的考慮](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))|
|請參閱 AD FS 容量規劃指引，以判斷您應在生產環境中使用的適當同盟伺服器和同盟伺服器 proxy 伺服器數目。|![規劃 AD FS 伺服器容量連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS Server 容量](../design/planning-for-ad-fs-server-capacity.md)|
|若要有效地規劃和執行帳戶夥伴部署的實體拓撲，請判斷您的 AD FS 設計是否需要一或多部同盟伺服器或同盟伺服器 proxy。|![檢查清單的圖示：設定同盟伺服器連結。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<p>![檢查清單的圖示：設定同盟伺服器 Proxy 連結。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|
|決定您想要新增至 AD FS 的屬性存放區類型。 然後，使用 AD FS 管理] 嵌入式管理單元新增屬性存放區 \- 。|![[屬性存放區的角色] 連結的圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<p>![[新增屬性存放區] 連結的圖示。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性存放區](../../ad-fs/operations/Add-an-Attribute-Store.md)|
|如果您需要將宣告傳送到或取用使用 AD FS 1.0 或1.1 同盟服務之帳戶夥伴的宣告，請參閱右側的連結，以取得如何設定 AD FS 與舊版 AD FS 交互操作的資訊。 如果帳戶夥伴組織也使用 AD FS 將宣告傳送或取用給您的組織，您可以略過此步驟，並繼續進行此檢查清單中的下一個工作。|![規劃 AD FS 1.x 連結的互通性規劃圖示。](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 1.x 的互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|在資源夥伴組織中部署第一部同盟伺服器之後，請使用 AD FS 管理] 嵌入式管理單元建立宣告提供者信任關係 \- 。 您可以手動輸入帳戶夥伴的相關資料，或使用帳戶夥伴組織的系統管理員為您提供的同盟中繼資料 URL，藉以建立宣告提供者信任。 您可以使用同盟中繼資料自動擷取資源夥伴的資料。 **注意：** 如果帳戶夥伴發佈其同盟中繼資料，或可為您提供檔案副本供您使用，我們建議您自動取出資料，因為它可以節省時間。|![[手動建立宣告提供者信任] 連結的圖示。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動建立宣告提供者信任](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/create-a-claims-provider-trust#to-create-a-claims-provider-trust-manually)<p>![使用同盟元資料連結建立宣告提供者信任的圖示。](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用同盟中繼資料建立宣告提供者信任](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/create-a-claims-provider-trust#to-create-a-claims-provider-trust-using-federation-metadata)|
|視組織的需求而定，請針對 AD FS 管理嵌入式管理單元中所指定的每個宣告提供者信任建立一或多個宣告規則集， \- 以便將傳入的宣告適當地傳遞、轉換或對應到資源夥伴中的對應宣告。|![檢查清單的圖示：建立宣告提供者信任連結的宣告規則。](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：建立宣告提供者信任的宣告規則](Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)|
|\(\)如果您的宣告描述尚未存在，可滿足您組織的需求，則可能必須建立選擇性的宣告描述。 AD FS 包含一組預設的宣告描述，會在 AD FS 管理嵌入式管理單元 \- 中公開。|![設定資源夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|
