---
ms.assetid: 80d50a9f-428e-40fe-b6b3-9837fd9a3efc
title: 檢查清單-設定資源合作夥伴公司
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4c73d84a6a32a90f5a5a60f0a0f6e6405ea3b729
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="checklist-configuring-the-resource-partner-organization"></a>檢查清單︰ 設定資源合作夥伴公司

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

資源合作夥伴組織包含裝載 Web\ 為基礎的應用程式，將由 account 合作夥伴使用者存取的網頁伺服器。 這個組織中的系統管理員必須使用 AD FS 管理 snap\ 中建立宣告代表 account 合作夥伴公司他們信任關係的提供者信任。 接下來 account 合作夥伴系統管理員必須建立針對每個 account 合作夥伴組織想要信任信賴廠商信任。  
  
此檢查清單會包含所需的部署 Active Directory 同盟服務 \(AD FS\) 資源合作夥伴組織中的工作。 它也包含針對設定所需來建立 one\ 一半聯盟合作關係元件工作。  
  
如果您要部署[網站 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)，您不需要遵循此檢查清單。 不過，您可以完成成功部署檢查清單中的工作[的聯盟網路 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 請務必 account 合作夥伴公司的系統管理員遵循的指導方針[檢查清單︰ 設定 Account 合作夥伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)以確保所有所需部署工作，將會完成成功半部聯盟合作關係建立第二個  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![設定資源合作夥伴組織](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定資源合作夥伴公司**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|如果今天 production 環境中有 AD FS 1.0 或 1.1 現有的部署，看到資訊，了解如何從您目前的同盟服務的設定移轉到新的 AD FS 同盟服務的權限的連結。 如果您在組織中的第一次部署 AD FS 使用 AD FS，您可以略過此步驟後繼續到新的資源合作夥伴公司所設定的相關資訊的檢查清單中的下一個工作。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[移轉到 AD FS 計劃](https://technet.microsoft.com/library/ff678044.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|根據您的部署目標，檢視元件所需的使用者提供聯盟應用程式存取的相關資訊。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供您 Active Directory 使用者存取您宣告感知應用程式與服務](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供您 Active Directory 使用者存取應用程式與其他公司的服務](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[提供使用者另一個組織存取您宣告感知應用程式與服務](https://technet.microsoft.com/library/dd807099.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|判斷哪一個 AD FS 設計此資源的合作夥伴公司將會與。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網站 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的聯盟網路 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|檢視不同的應用程式類型，並選擇要部署的應用程式。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您聯盟應用程式中策略資源合作夥伴](https://technet.microsoft.com/library/dd807077.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|部署 AD FS 伺服器在您開始之前，先檢視。1。\) 優點和缺點選擇 [Windows 內部資料庫 \(WID\) 或 SQL Server 儲存 AD FS 設定資料庫 2。\) AD FS 部署拓撲類型與他們相關聯的伺服器位置與網路的版面配置建議。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲注意事項](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|檢查 AD FS 容量計劃指導方針判斷聯盟伺服器和您應該 production 環境中使用聯盟 server proxy 伺服器的適當的數字。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 伺服器容量的計劃](https://technet.microsoft.com/library/gg749899.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|有效規劃和實作實體拓撲 account 合作夥伴部署，判斷是否 AD FS 設計需要一或多個聯盟伺服器或聯盟的 proxy 伺服器。|![設定資源合作夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![設定資源合作夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定好聯盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|判斷您想要新增到 AD FS 屬性存放區類型。 然後，新增屬性網上商店使用 AD FS 管理 snap\ 中。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的屬性商店角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![設定資源合作夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性網上商店](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|如果您將需要傳送或使用宣告從 account 合作夥伴是使用 AD FS 1.0 或 1.1 同盟服務，請查看連結以了解如何設定 AD FS 資訊的權限舊版 AD FS 交互操作主張。 如果 account 合作夥伴公司也會使用 AD FS 傳送或使用您的組織宣告，您可以略過此步驟，並繼續進行下一個任務檢查清單中。|![設定資源合作夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃 AD FS 使用的跨平台 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|部署資源合作夥伴組織中的第一個聯盟伺服器之後，請使用 AD FS 管理 snap\ 中建立宣告提供者信任關係。 您可以建立宣告提供者信任輸入 account 合作夥伴以手動方式的相關資料，或使用聯盟中繼資料 URL account 合作夥伴公司的系統管理員提供給您。 您可以使用聯盟中繼資料來自動資源夥伴擷取的資料。 **注意：**如果 account 合作夥伴發行其聯盟中繼資料，或可供您使用的檔案複本，我們建議您先自動擷取的資料就可以節省時間，因為。|![設定資源合作夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[可以廠商信任手動建立](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)<br /><br />![設定資源合作夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立可以廠商信任使用聯盟中繼資料](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|根據您的組織的需求，建立一或多個宣告規則集合，連入宣告會通過，AD FS 管理 snap\ 中指定每個宣告提供者信任轉換，或到對應宣告資源合作夥伴在適當地對應。|![設定資源合作夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 建立理賠要求規則信任宣告提供者](Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)|  
|![設定資源合作夥伴組織](media/icon_checkboxo.gif)|宣告描述可能會有一個並不存在，無法建立 \(Optional\) 將滿足您組織的需求。 AD FS 包含 AD FS 管理 snap\ 中宣告描述公開的預設設定。|![設定資源合作夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[需要新增描述宣告](../../ad-fs/operations/Add-a-Claim-Description.md)|  
