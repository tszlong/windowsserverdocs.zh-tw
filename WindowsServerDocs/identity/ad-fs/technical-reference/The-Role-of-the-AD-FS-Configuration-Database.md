---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: AD FS 設定資料庫的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 40f1f4952730fad0749a173fdc968714d043b1c1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188514"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>AD FS 設定資料庫的角色
AD FS 設定資料庫會儲存代表 Active Directory Federation Services 的單一執行個體的所有設定資料\(AD FS\) \(亦即 Federation Service\)。 AD FS 設定資料庫定義了供 Federation Service 識別夥伴、憑證、屬性存放區、宣告與這些關聯實體之各種相關資料所需的參數集。 您可以將這項組態資料儲存在 Microsoft SQL Server® 資料庫或 Windows 內部資料庫中\(WID\)隨附於 Windows Server® 2008年、 Windows Server 2008 R2 和 Windows Server® 2012年的功能。  
  
> [!NOTE]  
> 您可以將 AD FS 設定資料庫的整個內容儲存在單一 WID 執行個體或單一 SQL 資料庫執行個體中，但不能分開儲存。 這表示對於相同的 AD FS 設定資料庫執行個體，您不能為某些同盟伺服器使用 WID 並為其他同盟伺服器使用 SQL Server 資料庫。  
  
您可以使用此主題中的下列資訊搭配  [AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489.aspx)中提供的內容來了解選擇 WID 或 SQL Server 來儲存AD FS 設定資料庫的優點與缺點：  
  
WID 使用關聯式資料存放區，並不需要自己管理的使用者介面\(UI\)。 相反地，系統管理員時，可以使用其中一個 AD FS 管理嵌入式管理單元修改 AD FS 設定資料庫的內容\-中 Fsconfig.exe 或 Windows PowerShell™ cmdlet。  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>使用 WID 來儲存 AD FS 設定資料庫  
您可以建立使用 WID 做為存放區，使用 AD FS 設定資料庫 Fsconfig.exe 命令\-列工具或 AD FS 同盟伺服器設定精靈。 使用上述任一工具時，您可以選擇下列任一選項來建立您的同盟伺服器拓撲。 這些選項中的每個選項都是使用 WID 來儲存 AD FS 設定資料庫：  
  
-   建立獨立\-單獨的同盟伺服器  
  
-   在同盟伺服器陣列中建立第一部同盟伺服器  
  
-   新增同盟伺服器到同盟伺服器陣列  
  
如果您選取之獨立\-單獨的選項，WID 來儲存 AD FS 設定資料庫的單一執行個體。 此執行個體無法在多部同盟伺服器之間共用。 它只適用於測試實驗室環境。 如需有關待命\-單獨使用同盟伺服器選項或其中一個設定，請參閱[獨立伺服器使用 WID 的同盟](https://technet.microsoft.com/library/gg982486.aspx)或是[建立獨立同盟伺服器](https://technet.microsoft.com/library/ee913579.aspx)。  
  
若選取 [在同盟伺服器陣列中建立第一部同盟伺服器] 選項，會設定 WID 的延展性，允許您稍後將其他同盟伺服器新增到陣列。 如需有關部署 WID 陣列或其設定方式的詳細資訊，請參閱[使用 WID 的同盟伺服器陣列](https://technet.microsoft.com/library/gg982492.aspx)或[在同盟伺服器陣列中建立第一部同盟伺服器](https://technet.microsoft.com/library/dd807070.aspx)  
  
若選取新增同盟伺服器選項，會設定 WID 以根據設定的間隔時間將設定資料庫變更複寫到新的同盟伺服器。 如需有關將同盟伺服器新增到 WID 陣列的詳細資訊，請參閱 [使用 WID 的同盟伺服器陣列](https://technet.microsoft.com/library/gg982492.aspx)或[新增同盟伺服器到同盟伺服器陣列](https://technet.microsoft.com/library/ee913575.aspx)。  
  
> [!NOTE]  
> 當您部署使用 WID 的同盟伺服器陣列時，AD FS 的某些功能可能無法使用。 設定您的伺服器陣列時若要存取完整功能，請考慮改為使用 Microsoft SQL Server 來儲存 AD FS 設定資料庫。 如需詳細資訊，請參閱 [AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx)。  
  
### <a name="how-a-wid-federation-server-farm-works"></a>WID 同盟伺服器陣列的運作方式  
本節說明一些重要概念，它們說明 WID 同盟伺服器陣列如何在主要同盟伺服器與次要同盟伺服器之間複寫資料。 .  
  
#### <a name="primary-federation-server"></a>主要同盟伺服器  
主要同盟伺服器是執行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server® 2012 設定同盟伺服器角色與 AD FS 同盟伺服器設定精靈中，且具有 AD FS 的讀取/寫入複本的電腦組態資料庫。 當您使用 AD FS 同盟伺服器設定精靈，並選取要建立新的 Federation Service，並將該電腦的設定，第一部同盟伺服器陣列中的選項時，一律會建立在主要同盟伺服器。 此陣列中的所有其他同盟伺服器 (亦稱為次要同盟伺服器) 必須將主要同盟伺服器上的變更同步到其本機儲存的 AD FS 設定資料庫複本。  
  
#### <a name="secondary-federation-servers"></a>次要同盟伺服器  
次要同盟伺服器會儲存來自主要同盟伺服器，AD FS 設定資料庫的複本，但這些複本會讀取\-只。 次要同盟伺服器會根據已設定的間隔時間定期連線到陣列中的主要同盟伺服器並進行輪詢以檢查資料是否已變更，來同步資料。 次要同盟伺服器存在主要同盟伺服器載入的動作時提供容錯功能\-平衡在不同的站台，在整個網路環境中進行的存取要求。  
  
> [!NOTE]  
> 若主要同盟伺服器當機並離線，所有次要同盟伺服器會正常地繼續處理要求。 不過，在主要同盟伺服器重新上線之前，無法對 Federation Service 進行任何變更。 您也可以使用 Windows PowerShell 將次要同盟伺服器提升為主要同盟伺服器。 如需詳細資訊，請參閱 <<c0> [ 使用 Windows PowerShell 進行 AD FS 系統管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>AD FS 設定資料庫的同步方式  
AD FS 設定資料庫所扮演的重要角色，因為它可供在網路中的所有同盟伺服器上提供容錯與負載\-平衡功能，處理要求時\(時網路負載\-平衡器會使用\)。 不過，若要讓次要同盟伺服器提供此功能，必須同步儲存在主要同盟伺服器上的 AD FS 設定資料庫。  
  
當您新增同盟伺服器到陣列時，將成為次要同盟伺服器的新電腦會連線到主要同盟伺服器，以複寫 AD FS 設定資料庫的複本。 從現在開始，新的同盟伺服器會繼續定期從主要同盟伺服器同步更新，如下圖所示。  
  
![AD FS 角色](media/adfs2_WID.png)  
  
每部次要同盟伺服器每隔五分鐘都會輪詢主要同盟伺服器以同步變更。 您可以調整這個五個預設值\-分鐘值，或使用 Windows PowerShell cmdlet 來隨時強制立即同步處理。 如需如何執行這項操作的詳細資訊，請參閱[使用 Windows PowerShell 進行 AD FS 系統管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
WID 同步程序也支援遞增傳輸，可提供更有效率的變更傳輸。 遞增傳輸程序所需的網路頻寬極小，而且傳輸完成時間更快。  
  
> [!NOTE]  
> 支援將 AD FS 設定資料庫從 WID 移轉到 SQL Server 執行個體。 如需如何執行這項操作的詳細資訊，請參閱[AD FS:將 AD FS 設定資料庫移轉至 SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) TechNet Wiki 網站上。  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>使用 SQL Server 來儲存 AD FS 設定資料庫  
您可以建立使用單一的 SQL Server 資料庫執行個體做為存放區，使用 Fsconfig.exe 命令的 AD FS 設定資料庫\-列工具。 相較於 WID，使用 SQL Server 資料庫做為 AD FS 設定資料庫提供下列優點：  
  
-   系統管理員可以使用 SQL Server 的高可用性功能  
  
-   在高流量的情況下，它提供更好的效能。  
  
-   它提供的功能支援 SAML 成品解析與 SAML/WS\-同盟權杖重新執行偵測\(下述\)。  
  
當 AD FS 設定資料庫是儲存在 SQL 資料庫執行個體中時，「主要同盟伺服器」一詞不適用，因為所有同盟伺服器都可以讀取及寫入使用相同叢集 SQL Server 執行個體的 AD FS 設定資料庫，如下圖所示。  
  
![AD FS 角色](media/adfs2_SQL.png)  
  
若要設定兩個或多個一起運作為伺服器叢集，請確定 AD FS 進行高可用性服務內送的用戶端要求的伺服器，您可以使用 SQL Server。 高可用性提供相應\-放大架構，您可以在此增加透過新增其他伺服器的伺服器容量。 您可以透過自動叢集容錯移轉來減少單一失敗點。  
  
您可以使用網路負載，以達到高可用性\-平衡與容錯移轉 SQL 叢集技術提供的服務。 如需如何設定 SQL Server 高可用性的詳細資訊，請參閱[高可用性解決方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)。  
  
### <a name="saml-artifact-resolution"></a>SAML 成品解析  
安全性聲明標記語言\(SAML\)成品解析是根據部分描述如何信賴憑證者的合作對象時，可以直接從宣告提供者擷取權杖的 SAML 2.0 通訊協定的端點。 在解析程序的第一個階段中，瀏覽器用戶端會聯繫資源同盟伺服器並向其提供成品。 在第二個階段中，資源同盟伺服器會將該成品傳送到帳戶夥伴組織中裝載的 SAML 成品端點 URL 以解析成品訊息。 在最後的階段中，帳戶同盟伺服器會代表瀏覽器用戶端將權杖簽發給同盟伺服器。  
  
> [!NOTE]  
> 如果您是帳戶夥伴組織中的系統管理員，請確定指派或繫結 SSL 憑證鏈結根憑證的 「 Windows 根憑證計劃的成員，在 IIS 中同盟被動網站\( <ComputerName>\\站台\\Default Web Site\\adfs\\ls\)伺服陣列中的所有帳戶同盟伺服器上。 此動作非常重要，它可以讓您不需要在資源同盟伺服器上手動將 SSL 憑證新增至本機電腦「受信任的人」憑證存放區，以及避免資源同盟伺服器無法解析在您的組織中發佈的成品。  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS-同盟權杖重新執行偵測  
「權杖重新執行」  一詞指的是帳戶夥伴組織中的瀏覽器用戶端嘗試多次傳送接收自帳戶同盟伺服器的相同權杖以向資源同盟伺服器驗證的動作。  當使用者按一下其瀏覽器中的 [上一頁]  按鈕試圖重新傳送驗證頁面時，會發生此動作。  
  
AD FS 提供稱為「權杖重新執行偵測」  的功能，可偵測使用相同權杖的多個權杖要求並予以捨棄。 權杖重新執行偵測啟用這項功能時，保護完整性的驗證要求，在這兩個 WS\-同盟被動設定檔與 SAML WebSSO 設定檔，藉由確定相同的權杖永遠不會使用一次以上。 在安全性要求非常高的情況下 (例如使用資訊站時)，應該啟用此功能。  
  
在資訊站範例中，當使用者登出網站之後，稍後惡意使用者可以嘗試使用瀏覽器歷程記錄來重新傳送前一位使用者載入的同盟驗證頁面。 此功能可以減輕此顧慮，方式是儲存帳戶夥伴組織進行之成功驗證的其他相關資訊，以偵測後續的權杖重新執行並防止惡意使用者進行多次驗證嘗試。  
  

