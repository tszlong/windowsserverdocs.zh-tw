---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: 將 AD FS 設定為將宣告傳送至 AD FS 1.x 宣告感知網路代理程式
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 2303570097193ee564254357452784be28b29ac1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969765"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>檢查清單：將 AD FS 設定為將宣告傳送至 AD FS 1.x 宣告感知網路代理程式


## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>檢查清單：設定 AD FS 將宣告傳送至 AD FS 1.x 宣告 \- 感知 Web 代理程式
此檢查清單包含 \( 在 Windows server 2012 中設定 Active Directory 同盟服務 AD FS 同盟服務所需的工作， \) 以傳送執行 AD FS 1 的 Web 服務器所裝載的應用程式可以理解的宣告。*x*宣告 \- 感知 Web 代理程式。

> [!NOTE]
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。

![設定 AD FS 以傳送宣告](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x 宣告 \- 感知網路代理程式**

|Task|參考|
|--------|-------------|
|規劃 Windows Server 2012 中 AD FS 與舊版 AD FS 之間的互通性，並深入瞭解名稱識別碼宣告類型。|![設定 AD FS 以傳送](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[與 AD FS 1.X 互通性](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))的宣告規劃|
|如果您尚未這麼做，請使用右邊的連結，先在 Windows Server 2012 中的 AD FS 同盟服務和 AD FS 1 之間建立信賴憑證者信任。*x*同盟服務。|[檢查清單：將 AD FS 設定為將宣告傳送至 AD FS 1.x 同盟服務](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|
|之前，您可以利用 AD FS 1 所裝載的應用程式來達到互通性。*x*宣告 \- 感知網路代理程式，您必須先在 Windows Server 2012 的 AD FS 同盟服務中建立信賴憑證者信任，才能 AD FS 1。 *x*宣告 \- 感知 Web 代理程式。 **注意：** 在 AD FS 同盟服務中建立這個信任，相當於將新的**應用程式**新增至 AD FS 1.x 同盟服務 \( **同盟服務 \\ 信任原則我的 \\ 組織 \\ 應用程式** \) 。 這個信賴憑證者信任是必要的，因為 AD FS 在自己的嵌入式管理單元中沒有對等的**應用程式**節點 \- 。 不過，它仍然必須具有應用程式的安全通道。<p>當您使用右側連結中的程式來設定信任時，您必須在 [新增信賴憑證者信任] 嚮導中執行下列步驟，以設定此信任與 AD FS 1 互通。*x*宣告 \- 感知網路代理程式：<p>1. 在 [**選取資料來源**] 頁面上，選取 **[手動輸入信賴憑證者信任的相關資料**]。<br />2. 在 [**選擇設定檔**] 頁面上，選取 [ **AD FS 1.0 和1.1 設定檔**]。<br />3. 在 [**設定 URL** ] 頁面的 [ **WS \- 同盟被動 URL**] 底下，輸入 AD FS 1 中所定義的**應用程式 URL** 。*x*合作夥伴的 x 同盟服務。<br />4. 在 [**設定識別碼**] 頁面的 [信賴憑證者**信任識別碼**] 底下，輸入 AD FS 1 中所定義的**應用程式 URL** 。*x*宣告 \- 感知網路代理程式|![設定 AD FS 以手動方式傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立信賴](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)憑證者信任|
|請洽詢執行 AD FS 1 之 Web 服務器的系統管理員。*x*宣告 \- 感知 Web 代理程式，並讓系統管理員在 \- Internet Information Services IIS 中的預設網站下，編輯與宣告感知應用程式相關聯的 web.config 檔案， \( \( \) \) 以指向 AD FS 同盟服務的 web 代理程式。<p>例如，以有效*myresourcefederationserver*的 `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` AD FS 同盟伺服器名稱取代 web.config 檔案標記中的 myresourcefederationserver。<p>這是應用程式和 AD FS 1.x 宣告感知 Web 代理程式的必要項，能夠取用 \- 從 Windows Server 2012 中的 AD FS 同盟服務傳送給它的宣告。|N\/A|
|在您稍早建立的信賴憑證者信任上，您必須建立宣告規則，以接受從屬性存放區解壓縮的傳入宣告，然後通過、篩選或將它們轉換成名稱識別碼宣告類型，讓 AD FS 1 可以加以理解及取用。*x*宣告 \- 感知 Web 代理程式。 **注意：** 建立此規則之前，請確定您在其中建立此規則的宣告規則集具有先 \( \) 從屬性存放區解壓縮輕量型目錄存取協定 LDAP 屬性宣告之前的規則。 此宣告將用來做為您所建立之規則的輸入，以傳送 AD FS 1。*x* \-相容的宣告。 如需如何建立規則來解壓縮 LDAP 屬性的詳細資訊，請參閱[建立規則以將 Ldap 屬性當做宣告來傳送](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![設定 AD FS 以傳送宣告](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立規則以傳送 AD FS 1.X 相容](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)宣告|

