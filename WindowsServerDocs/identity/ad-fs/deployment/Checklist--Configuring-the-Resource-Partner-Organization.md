---
ms.assetid: 80d50a9f-428e-40fe-b6b3-9837fd9a3efc
title: 檢查清單-設定資源夥伴組織
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 342214d03441b7394baa5f0219e448622b389225
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408531"
---
# <a name="checklist-configuring-the-resource-partner-organization"></a>檢查清單：設定資源夥伴組織

資源夥伴組織包含的 Web 服務器，主控的 Web\-應用程式將由帳戶夥伴中的使用者存取。 此組織中的系統管理員必須使用中的 [AD FS 管理] 嵌入式管理\-建立宣告提供者信任，以代表與帳戶夥伴組織的信任關係。 接著，帳戶夥伴系統管理員必須為他們想要信任的每個帳戶夥伴組織建立信賴憑證者信任。  
  
此檢查清單包含在資源夥伴組織中部署 Active Directory 同盟服務 \(AD FS\) 所需的工作。 其中也包括設定建立一\-的同盟合作關係之一半所需元件的工作。  
  
如果您要部署[網頁 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)，則不需要遵循這份檢查清單。 不過，您必須完成此檢查清單中的工作，才能成功部署同盟[網頁 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)。  
  
> [!IMPORTANT]  
> 請確定帳戶夥伴組織的系統管理員遵循[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)中的指導方針，以確保所有必要的部署工作都將完成，以成功建立第二部分的同盟合作關係  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定資源夥伴組織](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定資源夥伴組織**  
  
||工作|參考|  
|-|--------|-------------|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|如果您目前的生產環境中有現有的 AD FS 1.0 或1.1 部署，請參閱右邊的連結，以取得如何將設定從目前的同盟服務遷移至新 AD FS 同盟服務的資訊。 如果您是第一次使用 AD FS 在組織中部署 AD FS，則可以略過此步驟，並繼續進行此檢查清單中的下一項工作，以取得如何設定新的資源夥伴組織的相關資訊。|![設定資源夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃遷移至 AD FS](https://technet.microsoft.com/library/ff678044.aspx)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|根據您的部署目標，檢查為使用者提供同盟應用程式存取所需元件的相關資訊。|![設定資源夥伴組織會為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[您的 Active Directory 使用者提供您的宣告感知應用程式和服務的存取權](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![設定資源夥伴組織會](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[為您的 Active Directory 使用者提供其他組織的應用程式和服務存取權](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![設定資源夥伴組織為](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[另一個組織中的使用者提供您的宣告感知應用程式和服務的存取權](https://technet.microsoft.com/library/dd807099.aspx)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|判斷此資源夥伴組織將與哪個 AD FS 設計相關聯。|![設定資源夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![設定資源夥伴組織同盟](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[網頁 SSO 設計](https://technet.microsoft.com/library/dd807050.aspx)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|請檢查不同的應用程式類型，並決定要部署的應用程式。|![設定資源夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定資源夥伴中的同盟應用程式策略](https://technet.microsoft.com/library/dd807077.aspx)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|開始部署 AD FS 伺服器之前，請先參閱;1.\) 選擇 Windows 內部資料庫 \(WID\) 或 SQL Server 儲存 AD FS 設定資料庫2的優點和缺點。\) AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![設定資源夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定資源夥伴組織](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指導方針，以判斷您應該在生產環境中使用的適當同盟伺服器和同盟伺服器 proxy 伺服器數目。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 伺服器容量的](https://technet.microsoft.com/library/gg749899.aspx)資源夥伴組織規劃|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|若要有效規劃及執行帳戶夥伴部署的實體拓撲，請判斷您的 AD FS 設計是否需要一或多部同盟伺服器或同盟伺服器 proxy。|![設定資源夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![設定資源夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|決定您想要新增至 AD FS 的屬性存放區類型。 然後，使用中的 [AD FS 管理] 嵌入式管理單元\-新增屬性存放區。|![為資源夥伴組織設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![設定資源夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增屬性存放區](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|如果您需要傳送宣告給或取用使用 AD FS 1.0 或1.1 同盟服務之帳戶夥伴的宣告，請參閱右邊的連結，以取得如何設定 AD FS 與舊版 AD FS 交互操作的資訊。 如果帳戶夥伴組織也使用 AD FS 來傳送或使用您組織的宣告，您可以略過此步驟，並繼續進行此檢查清單中的下一項工作。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[與 AD FS 1.X 互通性](https://technet.microsoft.com/library/ff678040.aspx)的資源夥伴組織規劃|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|在資源夥伴組織中部署第一部同盟伺服器之後，請使用中的 [AD FS 管理] 嵌入式管理單元\-建立宣告提供者信任關係。 您可以手動輸入有關帳戶夥伴的資料，或使用帳戶夥伴組織的系統管理員提供給您的同盟中繼資料 URL，藉以建立宣告提供者信任。 您可以使用同盟中繼資料自動擷取資源夥伴的資料。 **注意：** 如果帳戶夥伴發佈其同盟中繼資料，或可以提供檔案複本供您使用，我們建議您自動抓取資料，因為它可以節省時間。|![設定資源夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動建立信賴](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)憑證者信任<br /><br />![設定資源夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[使用同盟中繼資料建立信賴](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)憑證者信任|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|視組織的需求而定，針對在 AD FS 管理 嵌入式管理單元\-中指定的每個宣告提供者信任建立一或多個宣告規則集，以便將內送宣告傳遞、轉換或適當地對應至資源夥伴中的對應宣告。|![設定資源夥伴組織](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：建立宣告提供者信任的宣告規則](Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)|  
|![設定資源夥伴組織](media/icon_checkboxo.gif)|\(選擇性\) 若尚未存在可滿足貴組織需求的宣告描述，可能就必須加以建立。 AD FS 包含一組預設的宣告描述，會在的 AD FS 管理嵌入式\-管理單元中公開。|![設定資源夥伴組織](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增宣告描述](../../ad-fs/operations/Add-a-Claim-Description.md)|  
