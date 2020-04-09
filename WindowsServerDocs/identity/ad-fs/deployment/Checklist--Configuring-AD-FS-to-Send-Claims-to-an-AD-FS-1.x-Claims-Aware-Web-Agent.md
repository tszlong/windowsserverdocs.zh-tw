---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: 部署同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 450364d7fe760276b0d8c47167f8e65559b6f93f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854131"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>檢查清單：將 AD FS 設定為將宣告傳送至 AD FS 1.x 宣告感知網路代理程式

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>檢查清單：設定 AD FS 將宣告傳送至 AD FS 1.x 宣告\-感知網路代理程式  
此檢查清單包含在 Windows Server 2012 中設定 Active Directory 同盟服務 \(AD FS\) 同盟服務所需的工作，以傳送執行 AD FS 1 的 Web 服務器所裝載的應用程式可以理解的宣告。*x*宣告\-感知 Web 代理程式。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定 AD FS 以傳送宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：將 AD FS 設定為將宣告傳送至 AD FS 1.x 宣告\-感知 Web 代理程式**  
  
||工作|參考|  
|-|--------|-------------|  
|![設定 AD FS 以傳送宣告](media/icon_checkboxo.gif)|規劃 Windows Server 2012 中 AD FS 與舊版 AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。|![設定 AD FS 來傳送](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[與 AD FS 1.X 互通性](https://technet.microsoft.com/library/ff678040.aspx)的宣告規劃|  
|![設定 AD FS 以傳送宣告](media/icon_checkboxo.gif)|如果您尚未這麼做，請使用右邊的連結，先在 Windows Server 2012 中的 AD FS 同盟服務和 AD FS 1 之間建立信賴憑證者信任。*x*同盟服務。|[檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 同盟服務](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![設定 AD FS 以傳送宣告](media/icon_checkboxo.gif)|之前，您可以利用 AD FS 1 所裝載的應用程式來達到互通性。*x*宣告\-感知 Web 代理程式，您必須先在 Windows Server 2012 的 AD FS 同盟服務中建立信賴憑證者信任到 AD FS 1。 *x*宣告\-感知 Web 代理程式。 **注意：** 在 AD FS 同盟服務中建立此信任，等同于將新的**應用程式**新增至 AD FS 1.x 同盟服務 \(同盟服務的**組織\\應用程式\\\\信任原則**。\) 這個信賴憑證者信任是必要的，因為 AD FS 在的嵌入式管理\-中沒有對等的**應用程式**節點。 不過，它仍然必須具有應用程式的安全通道。<p>當您使用右側連結中的程式來設定信任時，您必須在 [新增信賴憑證者信任] 嚮導中執行下列步驟，以設定此信任與 AD FS 1 互通。*x*宣告\-感知 Web 代理程式：<p>1. 在 [**選取資料來源**] 頁面上，選取 **[手動輸入信賴憑證者信任的相關資料**]。<br />2. 在 [**選擇設定檔**] 頁面上，選取 [ **AD FS 1.0 和1.1 設定檔**]。<br />3. 在 [**設定 URL** ] 頁面的 [ **WS\-同盟被動 URL**] 底下，輸入 AD FS 1 中所定義的**應用程式 URL** 。合作夥伴的*x*同盟服務。<br />4. 在 [**設定識別碼**] 頁面的 [信賴憑證者**信任識別碼**] 底下，輸入 AD FS 1 中所定義的**應用程式 URL** 。*x*宣告\-感知 Web 代理程式|![設定 AD FS 以手動方式傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任|  
|![設定 AD FS 以傳送宣告](media/icon_checkboxo.gif)|請洽詢執行 AD FS 1 之 Web 服務器的系統管理員。*x*宣告\-感知 web 代理程式，並讓系統管理員編輯 web.config 檔案，該檔案與 INTERNET INFORMATION SERVICES \(IIS\)\) 中的 [預設的網站] 下的宣告\-感知應用程式 \(相關聯，以 AD FS 同盟服務。<p>例如，將 web.config 檔案的標記 `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` 中的*myresourcefederationserver*取代為有效的 AD FS 同盟伺服器名稱。<p>這是應用程式和 AD FS 1.x 宣告\-感知 Web 代理程式的必要項，以便從 Windows Server 2012 中的 AD FS 同盟服務使用傳送給它的宣告。|N\/A|  
|![設定 AD FS 以傳送宣告](media/icon_checkboxo.gif)|在您稍早建立的信賴憑證者信任上，您必須建立宣告規則，以接受從屬性存放區解壓縮的傳入宣告，然後通過、篩選或將它們轉換成名稱識別碼宣告類型，讓 AD FS 1 可以加以理解及取用。*x*宣告\-感知 Web 代理程式。 **注意：** 建立此規則之前，請確定您在其中建立此規則的宣告規則集具有先從屬性存放區解壓縮輕量型目錄存取協定 \(LDAP\) 屬性宣告的規則。 此宣告將用來做為您所建立之規則的輸入，以傳送 AD FS 1。*x*\-相容的宣告。 如需如何建立規則來解壓縮 LDAP 屬性的詳細資訊，請參閱[建立規則以將 Ldap 屬性當做宣告來傳送](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定 AD FS 以傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則以傳送 AD FS 1.X 相容](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)宣告|  
  

