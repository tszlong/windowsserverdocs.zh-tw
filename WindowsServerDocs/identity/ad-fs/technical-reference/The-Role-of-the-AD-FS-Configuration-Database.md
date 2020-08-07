---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: AD FS 設定資料庫的角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3143d4ccb05539d0374b6aea9096758e8f8b8960
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937921"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>AD FS 設定資料庫的角色
AD FS 設定資料庫會儲存代表 Active Directory 同盟服務 AD FS 單一實例的所有設定資料 \( \) \( ，也就是同盟服務 \) 。 AD FS 設定資料庫定義了供 Federation Service 識別夥伴、憑證、屬性存放區、宣告與這些關聯實體之各種相關資料所需的參數集。 您可以將此設定資料儲存在 &reg; \( \) windows Server &reg; 2008、windows Server 2008 R2 和 windows server 2012 隨附的 Microsoft SQL Server 資料庫或 windows 內部資料庫 WID 功能中 &reg; 。

> [!NOTE]
> 您可以將 AD FS 設定資料庫的整個內容儲存在單一 WID 執行個體或單一 SQL 資料庫執行個體中，但不能分開儲存。 這表示對於相同的 AD FS 設定資料庫執行個體，您不能為某些同盟伺服器使用 WID 並為其他同盟伺服器使用 SQL Server 資料庫。

您可以使用此主題中的下列資訊搭配  [AD FS 部署拓撲考量](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))中提供的內容來了解選擇 WID 或 SQL Server 來儲存AD FS 設定資料庫的優點與缺點：

WID 使用關聯式資料存放區，而且沒有自己的管理使用者介面 \( UI \) 。 系統管理員可以改為使用 AD FS 管理嵌入式管理單元 \- 、Fsconfig.exe 或 Windows PowerShell Cmdlet 來修改 AD FS 設定資料庫的內容 &trade; 。

## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>使用 WID 來儲存 AD FS 設定資料庫
您可以使用 [Fsconfig.exe 命令 \- 行工具] 或 [AD FS 同盟伺服器設定] Wizard，以 WID 做為存放區來建立 AD FS 設定資料庫。 使用上述任一工具時，您可以選擇下列任一選項來建立您的同盟伺服器拓撲。 這些選項中的每個選項都是使用 WID 來儲存 AD FS 設定資料庫：

-   建立獨立 \- 同盟伺服器

-   在同盟伺服器陣列中建立第一部同盟伺服器

-   新增同盟伺服器到同盟伺服器陣列

如果您選取 [獨立] \- 選項，則會使用 WID 來儲存 AD FS 設定資料庫的單一實例。 此執行個體無法在多部同盟伺服器之間共用。 它只適用於測試實驗室環境。 如需有關 [獨立 \- 同盟伺服器] 選項或如何設定的詳細資訊，請參閱[使用 WID 的獨立同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982486(v=ws.11))或[建立獨立的同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913579(v=ws.11))。

若選取 [在同盟伺服器陣列中建立第一部同盟伺服器] 選項，會設定 WID 的延展性，允許您稍後將其他同盟伺服器新增到陣列。 如需有關部署 WID 陣列或其設定方式的詳細資訊，請參閱[使用 WID 的同盟伺服器陣列](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982492(v=ws.11))或[在同盟伺服器陣列中建立第一部同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807070(v=ws.11))

若選取新增同盟伺服器選項，會設定 WID 以根據設定的間隔時間將設定資料庫變更複寫到新的同盟伺服器。 如需有關將同盟伺服器新增到 WID 陣列的詳細資訊，請參閱 [使用 WID 的同盟伺服器陣列](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982492(v=ws.11))或[新增同盟伺服器到同盟伺服器陣列](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913575(v=ws.11))。

> [!NOTE]
> 當您使用 WID 部署同盟伺服器陣列時，AD FS 的某些功能可能無法使用。 設定您的伺服器陣列時若要存取完整功能，請考慮改為使用 Microsoft SQL Server 來儲存 AD FS 設定資料庫。 如需詳細資訊，請參閱 [AD FS 部署拓撲考量](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))。

### <a name="how-a-wid-federation-server-farm-works"></a>WID 同盟伺服器陣列的運作方式
本節說明一些重要概念，它們說明 WID 同盟伺服器陣列如何在主要同盟伺服器與次要同盟伺服器之間複寫資料。 .

#### <a name="primary-federation-server"></a>主要同盟伺服器
主要同盟伺服器是一部執行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 的電腦，已 &reg; 使用 AD FS 同盟伺服器設定] Wizard 在同盟伺服器角色中設定，而且具有 AD FS 設定資料庫的讀取/寫入複本。 當您使用 [AD FS 同盟伺服器設定] Wizard 並選取建立新同盟服務的選項，並將該電腦設定為伺服器陣列中的第一部同盟伺服器時，一律會建立主要同盟伺服器。 此陣列中的所有其他同盟伺服器 (亦稱為次要同盟伺服器) 必須將主要同盟伺服器上的變更同步到其本機儲存的 AD FS 設定資料庫複本。

#### <a name="secondary-federation-servers"></a>次要同盟伺服器
次要同盟伺服器會將 AD FS 設定資料庫的複本儲存在主要同盟伺服器上，但這些複本是唯讀的 \- 。 次要同盟伺服器會根據已設定的間隔時間定期連線到陣列中的主要同盟伺服器並進行輪詢以檢查資料是否已變更，來同步資料。 次要同盟伺服器的存在是為了提供主要同盟伺服器的容錯，同時負責在 \- 整個網路環境中的不同網站進行負載平衡存取要求。

> [!NOTE]
> 若主要同盟伺服器當機並離線，所有次要同盟伺服器會正常地繼續處理要求。 不過，在主要同盟伺服器重新上線之前，無法對 Federation Service 進行任何變更。 您也可以使用 Windows PowerShell 將次要同盟伺服器提升為主要同盟伺服器。 如需詳細資訊，請參閱[使用 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。

#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>AD FS 設定資料庫的同步方式
因為 AD FS 設定資料庫所扮演的重要角色，所以可在網路中的所有同盟伺服器上使用，以在 \- \( 使用網路負載平衡器時，在處理要求時提供容錯和負載平衡功能 \- \) 。 不過，若要讓次要同盟伺服器提供此功能，必須同步儲存在主要同盟伺服器上的 AD FS 設定資料庫。

當您新增同盟伺服器到陣列時，將成為次要同盟伺服器的新電腦會連線到主要同盟伺服器，以複寫 AD FS 設定資料庫的複本。 從現在開始，新的同盟伺服器會繼續定期從主要同盟伺服器同步更新，如下圖所示。

![AD FS 角色](media/adfs2_WID.png)

每部次要同盟伺服器每隔五分鐘都會輪詢主要同盟伺服器以同步變更。 您可以調整這個預設 \- 的五分鐘值，或使用 Windows PowerShell Cmdlet 隨時強制立即同步處理。 如需如何執行此動作的詳細資訊，請參閱[使用 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。

WID 同步程序也支援遞增傳輸，可提供更有效率的變更傳輸。 遞增傳輸程序所需的網路頻寬極小，而且傳輸完成時間更快。

> [!NOTE]
> 支援將 AD FS 設定資料庫從 WID 移轉到 SQL Server 執行個體。 如需如何執行此動作的詳細資訊，請參閱 TechNet Wiki 網站上的[AD FS：將您的 AD FS 設定資料庫移轉至 SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) 。

## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>使用 SQL Server 來儲存 AD FS 設定資料庫
您可以使用 Fsconfig.exe 命令列工具，以單一 SQL Server 資料庫實例作為存放區，來建立 AD FS 設定資料庫 \- 。 相較於 WID，使用 SQL Server 資料庫做為 AD FS 設定資料庫提供下列優點：

-   系統管理員可以使用 SQL Server 的高可用性功能

-   在高流量的情況下，它提供更好的效能。

-   它提供 SAML 成品解析和 SAML/WS 同盟權杖重新執行偵測的功能支援， \- \( 如下所述 \) 。

當 AD FS 設定資料庫儲存在 SQL 資料庫實例中時，「主要同盟伺服器」一詞不適用，因為所有的同盟伺服器都可以讀取和寫入使用相同叢集 SQL Server 實例的 AD FS 設定資料庫，如下圖所示。

![AD FS 角色](media/adfs2_SQL.png)

您可以使用 SQL Server 設定兩部或更多部伺服器一起當做伺服器叢集一起運作，以確保 AD FS 會設為高可用性，以服務傳入的用戶端要求。 高可用性提供向 \- 外延展架構，您可以在其中新增額外的伺服器來增加伺服器的容量。 您可以透過自動叢集容錯移轉來減少單一失敗點。

您可以使用 SQL 叢集技術所提供的網路負載 \- 平衡和容錯移轉服務來達到高可用性。 如需如何設定 SQL Server 以取得高可用性的詳細資訊，請參閱[高可用性解決方案總覽](https://go.microsoft.com/fwlink/?LinkId=179853)。

### <a name="saml-artifact-resolution"></a>SAML 成品解析
安全性聲明標記語言 \( saml 成品 \) 解析是以 saml 2.0 通訊協定的一部分為基礎的端點，描述信賴憑證者如何直接從宣告提供者取得權杖。 在解析程序的第一個階段中，瀏覽器用戶端會聯繫資源同盟伺服器並向其提供成品。 在第二個階段中，資源同盟伺服器會將該成品傳送到帳戶夥伴組織中裝載的 SAML 成品端點 URL 以解析成品訊息。 在最後的階段中，帳戶同盟伺服器會代表瀏覽器用戶端將權杖簽發給同盟伺服器。

> [!NOTE]
> 如果您是帳戶夥伴組織的系統管理員，請務必指派 SSL 憑證，或將它系結至伺服器陣列中 \( <ComputerName> \\ \\ \\ \\ \) 所有帳戶同盟伺服器上的同盟被動網站，其會連結至 Windows 根憑證計畫成員的根憑證。 此動作非常重要，它可以讓您不需要在資源同盟伺服器上手動將 SSL 憑證新增至本機電腦「受信任的人」憑證存放區，以及避免資源同盟伺服器無法解析在您的組織中發佈的成品。

### <a name="samlws---federation-token-replay-detection"></a>SAML/WS-同盟權杖重新執行偵測
「權杖重新執行」** 一詞指的是帳戶夥伴組織中的瀏覽器用戶端嘗試多次傳送接收自帳戶同盟伺服器的相同權杖以向資源同盟伺服器驗證的動作。當使用者按一下其瀏覽器中的 [上一頁]**** 按鈕試圖重新傳送驗證頁面時，會發生此動作。

AD FS 提供稱為「權杖重新執行偵測」** 的功能，可偵測使用相同權杖的多個權杖要求並予以捨棄。 啟用這項功能時，權杖重新執行偵測會保護 WS \- 同盟被動設定檔和 SAML WebSSO 設定檔中的驗證要求完整性，方法是確定相同的權杖永遠不會使用一次以上。 在安全性要求非常高的情況下 (例如使用資訊站時)，應該啟用此功能。

在資訊站範例中，當使用者登出網站之後，稍後惡意使用者可以嘗試使用瀏覽器歷程記錄來重新傳送前一位使用者載入的同盟驗證頁面。此功能可以減輕此顧慮，方式是儲存帳戶夥伴組織進行之成功驗證的其他相關資訊，以偵測後續的權杖重新執行並防止惡意使用者進行多次驗證嘗試。

