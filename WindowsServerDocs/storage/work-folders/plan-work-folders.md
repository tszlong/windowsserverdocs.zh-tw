---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: 工作資料夾部署的規劃
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: 如何規劃工作資料夾的部署，包括系統需求以及如何準備您的網路環境。
ms.openlocfilehash: 2ac52b15f266fce7202df4c9c76e774fca4098cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824639"
---
# <a name="planning-a-work-folders-deployment"></a>工作資料夾部署的規劃

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10，Windows 8.1，Windows 7

本主題說明設計工作資料夾實作的程序，並假設您已具備下列背景知識：  
  
-   具備工作資料夾的基本知識 (如[工作資料夾概觀](work-folders-overview.md)中所述)  
  
-   具備 Active Directory Domain Services (AD DS) 概念的基本知識  
  
-   具備 Windows 檔案共用和相關技術的基本知識  
  
-   具備 SSL 憑證用法的基本知識  
  
-   具備透過 Web 反向 Proxy 啟用 Web 存取內部資源的基本知識  
  
 下列各節將協助您設計工作資料夾實作。 部署工作資料夾會在下一個主題[部署工作資料夾](deploy-work-folders.md)中討論。  
  
##  <a name="BKMK_SOFT"></a> 軟體需求  

工作資料夾對檔案伺服器和網路基礎結構具有下列軟體需求：  
  
-   執行 Windows Server 2012 R2 或 Windows Server 2016 的伺服器，用於裝載與使用者檔案的同步共用  
  
-   以 NTFS 檔案系統格式化的磁碟區，用來存放使用者檔案  
  
-   若要在 Windows 7 電腦上強制執行密碼原則，您必須使用群組原則密碼原則。 您也必須將 Windows 7 電腦從「工作資料夾」密碼原則排除 (若有使用的話)。

-   每個裝載工作資料夾之檔案伺服器的伺服器憑證。 這些憑證必須來自使用者信任的憑證授權單位 (CA)，最好是公開憑證授權單位。

-   (選用) Windows Server 2012 R2 中 Active Directory Domain Services 樹系具有架構延伸，在使用多部檔案伺服器時，支援自動將電腦和裝置參照到正確的檔案伺服器。 
  
若要讓使用者透過網際網路同步，還有下列其他需求：  
  
-   能夠從網際網路存取伺服器，方法是在組織的反向 Proxy 或網路閘道中建立發佈規則  
  
-   (選用) 公開登錄的網址名稱以及能夠建立網域的其他公用 DNS 記錄  
  
-   (選用) 使用 Active Directory 同盟服務 (AD FS) 驗證時的 AD FS 基礎結構  
  
工作資料夾對用戶端電腦具有下列軟體需求：  
  
-   電腦必須執行下列其中一種作業系統：  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   Android 4.4 KitKat 和更新版本  
  
    -   iOS 10.2 和更新版本  
  
-   Windows 7 電腦必須執行下列其中一個 Windows 版本：  
  
    -   Windows 7 專業版  
  
    -   Windows 7 旗艦版  
  
    -   Windows 7 企業版  
  
-   Windows 7 電腦必須加入您組織的網域 (它們無法加入工作群組)。  
  
-   本機 NTFS 格式的磁碟機上具有足夠可用空間，可用來在工作資料夾中存放所有使用者的檔案，如果工作資料夾位於系統磁碟機，還需要額外 6 GB 的可用空間，如預設的指定。 工作資料夾預設會使用下列位置︰**%USERPROFILE%\Work Folders**  
  
     不過，使用者在設定期間可以變更位置 (支援的位置包括 microSD 記憶卡和使用 NTFS 檔案系統格式化的 USB 磁碟機，如果磁碟機被移除，則會停止同步)。  
  
     個別檔案的預設大小上限為 10 GB。 每個使用者的儲存空間沒有限制，但是系統管理員可以使用檔案伺服器資源管理員的配額功能來實作配額。  
  
-   工作資料夾不支援復原用戶端虛擬機器的虛擬機器狀態。 請改為使用系統映像備份或其他備份應用程式，從用戶端虛擬機器內部執行備份和還原作業。  
  
> [!NOTE]
>  請務必在所有工作資料夾伺服器上和任何執行 Windows 8.1 或 Windows Server 2012 R2 的用戶端電腦上安裝 Windows 8.1 和 Windows Server 2012 R2 一般可用性更新彙總套件。 如需詳細資訊，請參閱 Microsoft 知識庫文章 [2883200](https://support.microsoft.com/kb/2883200)。  
  
## <a name="deployment-scenarios"></a>部署案例  
 工作資料夾可在客戶環境內任意數量的檔案伺服器上實作。 這種方式可允許工作資料夾實作根據客戶需求進行調整，而產生高度個人化部署。 不過，大部分的部署都屬於下列三個基本案例的其中一種。  
  
### <a name="single-site-deployment"></a>單一站台部署  
 在單一站台部署中，檔案伺服器裝載於客戶基礎結構的中央站台內。 這種部署類型最常見於具有高度集中式基礎結構或分公司數量較少而不維護當地檔案伺服器的客戶。 這種部署模型易於 IT 人員管理，因為所有伺服器資產都在本機，而且網際網路入口/出口通常都集中在這個位置。 不過，這種部署模型也依賴中央站台與任何分公司之間良好的 WAN 連線，而分公司的使用者會因為網路狀況而容易發生服務中斷的情形。  
  
### <a name="multiple-site-deployment"></a>多站台部署  
 在多站台部署中，檔案伺服器裝載於客戶基礎結構內的多個位置。 這表示多個資料中心或分公司維護個別的檔案伺服器。 這種部署類型最常見於較大規模的客戶環境或具有多個規模較大的分公司維護當地伺服器資產的客戶。 這種部署模型對於 IT 人員管理而言較為複雜，而且它仰賴謹慎協調的資料存放區與 Active Directory Domain Services (AD DS) 的維護，以確保使用者為工作資料夾使用正確的同步伺服器。  
  
### <a name="hosted-deployment"></a>託管型部署  
 在託管型部署中，同步伺服器是部署於 IAAS (基礎結構做為服務) 解決方案 (如 Windows Azure VM) 中。 這種部署方法的優點在於能夠讓檔案伺服器的可用性，較不依賴客戶企業內的 WAN 連線能力。 如果裝置能夠連線到網際網路，就能連線到它的同步伺服器。 不過，部署在託管型環境中的伺服器，仍然必須能夠連線組織的 Active Directory 網域以便驗證使用者，而且客戶是用內部部署基礎結構需求換取維護該連線的額外複雜性。  
  
## <a name="deployment-technologies"></a>部署技術  
 工作資料夾部署是由數種技術組合而成，它們共同搭配為內部和外部網路上的裝置提供服務。 設計工作資料夾部署之前，客戶應當熟悉下列每種技術的需求。  
  
### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
 AD DS 在工作資料夾部署中提供兩個重要的服務。 第一個，做為 Windows 驗證的後端，AD DS 提供安全性和驗證服務，用來授與使用者資料的存取權。 如果無法連線網域控制站，檔案伺服器就無法驗證傳入的要求，裝置也不能存取存放於該檔案伺服器上同步共用中的任何資料。  
  
 第二個，AD DS (包含 Windows Server 2012 R2 架構更新) 維護每個使用者的 msDS-SyncServerURL 屬性，用來自動將使用者導向到適當的同步伺服器。  
  
### <a name="file-servers"></a>檔案伺服器  
 執行 Windows Server 2012 R2 或 Windows Server 2016 的檔案伺服器裝載了「工作資料夾」角色服務，也裝載了存放使用者「工作資料夾」資料的同步共用。 檔案伺服器也可以裝載在內部網路上操作之其他技術存放的資料 (如檔案共用)，這些檔案伺服器也可以形成叢集，為使用者資料提供容錯。  
  
###  <a name="GroupPolicy"></a> 群組原則  
 若您的環境中有 Windows 7 電腦，我們建議下列方式：  
  
-   使用群組原則來控制已加入網域且使用「工作資料夾」之電腦的密碼原則。  
  
-   在未加入網域的電腦上，請使用「工作資料夾」的 **\[自動鎖定畫面，並要求輸入密碼\]** 原則。  
  
 您也可以使用群組原則將「工作資料夾」伺服器指定到已加入網域的電腦。 這可以稍微簡化工作資料夾的設定，否則使用者必須輸入他們的公司電子郵件地址來查詢設定 (假設工作資料夾已正確設定)，或是輸入您透過電子郵件或其他通訊方式明確提供給他們的工作資料夾 URL。  
  
 您也可以使用群組原則，強制為每個使用者或每個電腦設定工作資料夾，但是這樣做會導致使用者登入的每個電腦進行工作資料夾同步 (使用每個使用者原則設定時)，並阻止使用者為自己電腦上的工作資料夾指定其他位置 (例如，指定 microSD 記憶卡以節省主要磁碟機上的空間)。 建議您強制進行自動設定之前，先審慎評估使用者的需求。  
  
### <a name="windows-intune"></a>Windows Intune  
 Windows Intune 也為未加入網域的裝置提供安全性和管理能力等級，否則將不會顯示這些裝置。 您可以使用 Windows Intune 來設定和管理使用者透過網際網路連線到工作資料夾的個人裝置 (如平板電腦)。 Windows Intune 可提供裝置要使用的同步伺服器 URL-否則使用者必須輸入其工作電子郵件地址，來查詢設定 (如果您發佈公用工作資料夾 URL 的形式 https://workfolders。*contoso.com*)，或直接輸入同步伺服器 URL。  
  
 如果不使用 Windows Intune 部署，使用者必須手動設定外部裝置，這會增加客戶對支援工程師人員的需求。  
  
 您也可以使用 Windows Intune 選擇性從使用者裝置上的工作資料夾清除資料，而不會影響其餘的資料 – 如果使用者從您的公司離職或他們的裝置被偷，這個方式相當方便。  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Web 應用程式 Proxy/Azure AD 應用程式 Proxy  
 工作資料夾是以允許連線網際網路的裝置從內部網路安全地擷取商業資料的概念而建置的，允許使用者在平板電腦和裝置上「隨身攜帶資料」，而這些裝置通常無法存取工作檔案。 若要這樣做，必須使用反向 Proxy 來發佈同步伺服器 URL 並讓它們能夠供網際網路用戶端使用。 
 
工作資料夾支援使用 Web 應用程式 Proxy、Azure AD 應用程式 Proxy 或協力廠商反向 Proxy 解決方案： 

-  Web 應用程式 Proxy 是內部部署反向 Proxy 解決方案。 若要深入了解，請參閱 [Windows Server 2016 的 Web 應用程式 Proxy](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。  
  
-  Azure AD 應用程式 Proxy 是雲端反向 Proxy 解決方案。 若要深入了解，請參閱[如何提供內部部署應用程式的安全遠端存取](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>其他設計考量  
 除了了解以上所述的每個元件以外，客戶還需要花時間在設計上，思考運作的同步伺服器和同步共用的數量，以及是否要利用容錯移轉叢集，以便在這些同步伺服器上提供容錯。  
  
### <a name="number-of-sync-servers"></a>同步伺服器的數量  
 客戶可以在一個環境中運作多個同步伺服器。 這可能是適用的設定，原因有下列幾種：  
  
-   使用者的地理分佈 – 例如，分公司檔案伺服器或區域性資料中心  
  
-   資料存放區需求 – 特定業務部門可能具有特定資料存放區或使用專用伺服器較容易執行的處理需求。  
  
-   負載平衡 – 在大型環境中，將使用者資料存放於多個伺服器可能會增加伺服器的效能和執行時間。  
  
 如需工作資料夾伺服器擴充和效能的相關資訊，請參閱[工作資料夾部署的效能考量](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)。  
  
> [!NOTE]
>  使用多個同步伺服器時，建議為使用者設定自動伺服器探索。 這個程序需要設定 AD DS 中每個使用者帳戶上的屬性。 這個屬性的名稱為 **msDS-SyncServerURL**，將 Windows Server 2012 R2 網域控制站新增到網域或套用 Active Directory 架構更新之後，使用者帳戶上即可使用該屬性。 您應該為每個使用者設定這個屬性，以確保使用者連線至適當的同步伺服器。 藉由使用自動伺服器探索，組織可以發佈工作資料夾的 「 易記 」 URL 後面這類*https://workfolders.contoso.com*，不論在作業中的同步處理伺服器的數目。  
  
### <a name="number-of-sync-shares"></a>同步共用的數量  
 個別同步伺服器可以維護多個同步共用。 這相當有用，原因如下：  
  
-   稽核和安全性需求 – 如果特定部門使用的資料必須經常稽核或保留較長一段時間，個別的同步共用可協助系統管理員將不同稽核層級的使用者資料夾分開。  
  
-   不同的配額或檔案檢測 – 如果您想要針對不同的使用者群組，在工作資料夾中允許的檔案類型 (檔案檢測) 上設定不同的儲存配額或限制，則分開同步共用會有幫助。  
  
-   部門控制 – 如果管理責任是依部門而分配的，為不同的部門利用分開的共用可協助系統管理員強制執行配額或其他原則。  
  
-   不同的裝置原則 – 如果組織需要為不同的使用者群組維護多個裝置原則 (例如，加密工作資料夾)，請使用多個共用來完成這個工作。  
  
-   儲存容量 – 如果檔案伺服器具有多個磁碟區，則可以使用其他共用來利用這些其他磁碟區。 個別共用只能存取裝載它的磁碟區，而且無法利用檔案伺服器上的其他磁碟區。  
  
#### <a name="access-to-sync-shares"></a>存取同步共用  

使用者存取的同步伺服器是由在使用者用戶端上輸入的 URL 來決定的 (或者，如果是使用伺服器自動探索，則由為 AD DS 中使用者發佈的 URL 來決定)，而存取個別同步共用是由共用上存在的權限來決定的。  
  
因此，如果客戶在同一個伺服器上裝載多個同步共用，則必須小心以確保個別使用者具有只能存取這些其中一個共用的權限。 否則，在使用者連線到伺服器時，他們的用戶端可能會連線到錯誤的共用。 只要為每個同步共用建立個別的安全性群組，即可完成這項作業。  
  
不僅如此，存取同步共用內個別使用者資料夾是由資料夾上的擁有權來決定。 建立同步共用時，工作資料夾預設會授與使用者對自己檔案的獨佔存取權 (停用繼承並讓他們成為自己個別資料夾的擁有者)。  
  
## <a name="design-checklist"></a>設計檢查清單  

下列一組設計問題旨在協助客戶設計最適合他們環境的工作資料夾實作。 客戶應該在嘗試部署伺服器之前先閱讀這整份檢查清單。  
  
-   適用的使用者  
  
    -   哪些使用者會使用工作資料夾？  
  
    -   使用者的組織方式為何？ (依地理區域、依辦公室、依部門等等)  
  
    -   有任何使用者對資料存放區、安全性或保留方面有特殊需求嗎？  
  
    -   有任何使用者有特定的裝置原則需求 (例如加密) 嗎？  
  
    -   您需要支援哪些用戶端電腦及裝置？ (Windows 8.1、Windows RT 8.1、Windows 7)  
  
         若您要支援 Windows 7 電腦，且想要使用密碼原則，請將存放其電腦帳戶的網域從「工作資料夾」密碼原則排除，並改為針對該網域中已加入網域的電腦使用群組原則密碼原則。  
  
    -   您需要與其他使用者資料管理解決方案 (如資料夾重新導向) 相互操作或從這些解決方案移轉嗎？  
  
    -   多個網域的使用者需要透過網際網路與單一伺服器同步嗎？  
  
    -   您需要支援在加入網域的電腦上不屬於本機系統管理員群組成員的使用者嗎？ (如果需要，您必須從工作資料夾裝置原則 (如加密和密碼原則) 排除相關網域)  
  
-   基礎結構和容量規劃  
  
    -   同步伺服器應該位於網路上的哪些站台？  
  
    -   有任何同步伺服器會被基礎結構做為服務 (IaaS) 提供者 (如 Azure VM) 託管嗎？  
  
    -   有任何特定使用者群組需要專用的伺服器嗎，如果需要，每個專用伺服器上有多少位使用者？  
  
    -   網路上網際網路入口/出口點在哪裡？  
  
    -   同步伺服器會為容錯而被叢集化嗎？  
  
    -   同步伺服器需要維護多個資料磁碟區來裝載使用者資料嗎？  
  
-   資料安全性  
  
    -   需要在任何同步伺服器上建立多個同步共用嗎？  
  
    -   將使用哪些群組來提供存取同步共用？  
  
    -   如果您使用多個同步伺服器，您要為委派能力建立哪個安全性群組來修改使用者物件的 msDS-SyncServerURL 屬性？  
  
    -   個別同步共用有任何特殊安全性或稽核需求嗎？  
  
    -   需要多因素驗證 (MFA) 嗎？  
  
    -   您需要能夠從電腦和裝置遠端清除工作資料夾的資料嗎？  
  
-   裝置存取  
  
    -   要使用哪個 URL 為以網際網路為基礎的裝置提供存取 (*電子郵件型自動伺服器探索所需的預設 URL 是 workfolders.domainname*)？  
  
    -   如何將 URL 發佈到網際網路？  
  
    -   會使用自動伺服器探索嗎？  
  
    -   會使用群組原則來設定加入網域的電腦嗎？  
  
    -   會使用 Windows Intune 來設定外部裝置嗎？  
  
    -   裝置需要裝置註冊才能連線嗎？  
  
## <a name="next-steps"></a>後續步驟  
 設計工作資料夾實作之後，就該部署工作資料夾了。 如需詳細資訊，請參閱[部署工作資料夾](deploy-work-folders.md)。  
  
## <a name="see-also"></a>另請參閱  
 如需其他相關資訊，請參閱下列資源。  
  
|內容類型|參考|  
|------------------|----------------|  
|**產品評估**|-   [工作資料夾](work-folders-overview.md)<br />-   [工作資料夾適用於 Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) （部落格文章）|  
|**部署**|-   [設計工作資料夾實作](plan-work-folders.md)<br />-   [部署工作資料夾](deploy-work-folders.md)<br />-   [搭配 AD FS 與 Web 應用程式 Proxy (WAP) 部署工作資料夾](deploy-work-folders-adfs-overview.md)<br />- [使用 Azure AD 應用程式 Proxy 的部署工作資料夾](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />-   [工作資料夾部署的效能考量](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [工作資料夾適用於 Windows 7 （64 位元下載）](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [工作資料夾適用於 Windows 7 （32 位元下載）](https://www.microsoft.com/download/details.aspx?id=42559)<br />-   [工作資料夾測試實驗室部署](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx)（部落格文章）|
