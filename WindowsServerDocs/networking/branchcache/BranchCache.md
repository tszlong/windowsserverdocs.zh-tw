---
title: BranchCache
description: 本主題概要說明 Windows Server 2016 中的 BranchCache
manager: brianlic
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 934b3d7c774e2a61ceae79e1ddfaa9d0922b7164
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949574"
---
# <a name="branchcache"></a>BranchCache

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題 (對象為資訊技術 (IT) 專家) 提供關於 BranchCache 的概觀資訊，包括 BranchCache 模式、特色、功能以及不同作業系統中可用的 BranchCache 功能。

> [!NOTE]
> 除了本主題之外，還提供下列 BranchCache 文件。
>
> - [BranchCache 網路殼層與 Windows PowerShell 命令](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [BranchCache 部署指南](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**對 BranchCache 感興趣的對象？**

如果您是系統管理員、網路或儲存解決方案架構師或其他 IT 專業人員，下列情況可能會引起您對 BranchCache 的興趣：

- 您為其設計或支援 IT 基礎結構的組織是擁有兩個以上的實體位置和從分公司連線到總公司的廣域網路 (WAN)。

- 您為其設計和支援 IT 基礎結構的組織是已部署雲端技術和員工用以存取在遠端位置的資料與應用程式的 WAN 連線。

- 您想要降低分公司與總公司之間的網路流量，以使 WAN 頻寬使用量達到最佳狀況。

- 您已在總公司部署或規劃要部署符合本主題中所述設定的內容伺服器。

- 分公司中的用戶端電腦正在執行 Windows 10、Windows 8.1、Windows 8 或 Windows 7。

本主題包含下列章節：

-   [什麼是 BranchCache？](#bkmk_what)

-   [BranchCache 模式](#BKMK_2)

-   [啟用 BranchCache 的內容伺服器](#BKMK_3)

-   [BranchCache 與雲端](#BKMK_3a)

-   [內容資訊版本](#bkmk_version)

-   [BranchCache 處理檔案中內容更新的方式](#bkmk_handles)

-   [BranchCache 安裝指南](#BKMK_4)

-   [適用於 BranchCache 的作業系統版本](#bkmk_os)

-   [BranchCache 安全性](#bkmk_security)

-   [內容流程與程序](#bkmk_flow)

-   [快取安全性](#bkmk_cache)

## <a name="what-is-branchcache"></a><a name="bkmk_what"></a>什麼是 BranchCache？

BranchCache 是一種廣域網路 (WAN) 頻寬優化技術，包含在某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及某些 Windows Server 2012 R2 版本、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 的版本中。 為了在使用者存取遠端伺服器的內容時將 WAN 頻寬最佳化，BranchCache 會從總公司或託管的雲端內容伺服器擷取內容，並在分公司快取內容，讓分公司的用戶端電腦可從本機存取內容而非透過 WAN。

在分公司，內容會儲存在設定用來裝載快取的伺服器上，或者，如果分公司沒有任何可用的伺服器，請在執行 Windows 10、Windows 8.1、Windows 8 或 Windows 7 的用戶端電腦上。 用戶端電腦從總公司要求及接收內容之後，就會在分公司快取內容，同一分公司的其他電腦可以從本機取得內容，而非透過 WAN 連結從內容伺服器下載內容。

之後用戶端電腦若要求相同的內容時，用戶端會從伺服器下載 *內容資訊*，而非實際的內容。 內容資訊包含使用原始內容的區塊來計算的雜湊，比原始資料的內容更小。 用戶端電腦接著可以使用內容資訊，在分公司位於用戶端電腦或伺服器的快取中找出該內容。 用戶端電腦與伺服器也使用內容資訊來保護快取的內容，使未經授權的使用者無法存取。

BranchCache 可以改善分公司用戶端與伺服器的內容查詢回應時間，來增加使用者生產力，還可以降低 WAN 連結上的流量來協助改善網路效能。

## <a name="branchcache-modes"></a><a name="BKMK_2"></a>BranchCache 模式
BranchCache 有兩種作業模式：分散式快取模式與託管快取模式。

以分散式快取模式部署 BranchCache 時，分公司的內容快取會分散在用戶端電腦之間。

以託管快取模式部署 BranchCache 時，會在稱為託管快取伺服器的一或多部伺服器電腦上託管分公司的內容快取。

> [!NOTE]
> 您可以使用上述兩種模式來部署 BranchCache，不過每間分公司只能使用一種模式。 例如，若您有兩間分公司，一間有伺服器，而另一間沒有，您可以在有伺服器的辦公室以託管快取模式部署 BranchCache，而在只有用戶端電腦的辦公室以分散式快取模式部署 BranchCache。

在下圖中，BranchCache 是以兩種模式來部署。

![BranchCache 模式](../media/BranchCache/bc_modes.jpg)

沒有本機伺服器可做為託管快取伺服器使用的小型分公司，最適合使用分散式快取模式。 分散式快取模式可讓您部署 BranchCache，同時不需要另外在分公司部署硬體。

如果您想要部署 BranchCache 的分公司包含其他基礎結構，例如執行其他工作負載的一或多部伺服器，以託管快取模式部署 BranchCache 較有利的原因如下：

### <a name="increased-cache-availability"></a>提升快取可用性

託管快取模式可提升快取效率，因為即使原來要求和快取資料的用戶端已經離線，您還是可以使用內容。 因為託管快取伺服器永遠可供使用，所以快取的內容越多，節省的 WAN 頻寬就越多，進而改善 BranchCache 的效率。

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>適用於多個子網路分公司的集中式快取


分散式快取模式可以在單一子網路上運作。 在有多個子網路而設定為分散式快取模式的分公司，下載到某個子網路的檔案無法與其他子網路的用戶端電腦共用。

因此，其他子網路的用戶端無法探索到已經下載的檔案，而會在過程中使用 WAN 頻寬從總公司的內容伺服器來取得檔案。

不過，當您部署託管快取模式時，就不會有這種情況，在多個子網路的分公司中，所有用戶端都可以存取儲存在託管快取伺服器上的單一快取，即使是位於不同子網路上的用戶端。 此外，Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的 BranchCache 能讓您為每個分公司部署多部託管快取伺服器。

> [!CAUTION]
> 如果您使用 BranchCache 對檔案和資料夾進行 SMB 快取，請勿停用離線檔案。 如果您停用離線檔案，BranchCache SMB 快取會無法正常運作。

## <a name="branchcache-enabled-content-servers"></a><a name="BKMK_3"></a>啟用 BranchCache 的內容伺服器

當您部署 BranchCache 時，來源內容會儲存在您總公司或雲端資料中心的啟用 BranchCache 內容伺服器上。 BranchCache 支援下列內容伺服器類型：

> [!NOTE]
> 只有來源內容-也就是用戶端電腦一開始從啟用 BranchCache 的內容伺服器取得的內容，是由 BranchCache 加速。 用戶端電腦或託管快取伺服器不會快取用戶端電腦直接從其他來源取得的內容，例如網際網路上的網頁伺服器或 Windows Update，然後與分公司的其他電腦共用。 不過，如果您想要加速 Windows Update 內容，您可以在總公司或雲端資料中心安裝 Windows Server Update Services (WSUS) 應用程式伺服器，並將它設定為 BranchCache 內容伺服器。

### <a name="web-servers"></a>Web 伺服器

支援的網頁伺服器包括執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的電腦，這些電腦已安裝 Web 服務器 (IIS) 伺服器角色，以及使用超文字傳輸通訊協定 (HTTP) 或 HTTP 安全 (HTTPS) 。

此外，網頁伺服器必須已安裝 BranchCache 功能。

### <a name="file-servers"></a>檔案伺服器

支援的檔案伺服器包括執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 且已安裝「檔案服務伺服器角色」和「網路檔案的 BranchCache」角色服務的電腦。

這些檔案伺服器使用伺服器訊息區 (SMB) 在電腦間交換資訊。 在完成安裝檔案伺服器之後，您還必須共用資料夾以及使用群組原則或本機電腦原則，針對共用資料夾啟用雜湊產生功能，來啟用 BranchCache。

### <a name="application-servers"></a>應用程式伺服器

支援的應用程式伺服器包括執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 且) 安裝及啟用背景智慧型傳送服務 (位的電腦。

此外，應用程式伺服器必須已經安裝 BranchCache 功能。 作為應用程式伺服器的範例，您可以部署 Microsoft Windows Server Update Services (WSUS) ，並 Microsoft Endpoint Configuration Manager 分支發佈點伺服器作為 BranchCache 內容伺服器。

## <a name="branchcache-and-the-cloud"></a><a name="BKMK_3a"></a>BranchCache 與雲端

雲端具有龐大潛力，可以降低操作費用並讓規模達到新的層次，不過讓仰賴進行工作的人員遠離工作負載，會增加網路成本和損害生產力。 使用者預期會有很高的效能，並不在意其應用程式和資料的裝載位置。

BranchCache 可以改善網路應用程式效能和降低共用的資料快取耗用的頻寬。  工作者使用部署在雲端的伺服器，來改善分公司與總公司的生產力。

因為 BranchCache 不需要新硬體或是改變網路拓撲，這是改善辦公室位置與公開與私人雲端之間通訊的最佳解決方案。

> [!NOTE]
> 因為某些 Web proxy 無法處理非標準的內容編碼標頭，所以建議您將 BranchCache 與超文字傳輸通訊協定安全地 (HTTPS) ，而不是 HTTP。

= = = = = = = 如需有關 Windows Server 2016 中雲端技術的詳細資訊，請參閱 [軟體定義的網路 &#40;SDN&#41;](../sdn/software-defined-networking.md)。

## <a name="content-information-versions"></a><a name="bkmk_version"></a>內容資訊版本

內容資訊有兩個版本：

- 與執行 Windows Server 2008 R2 和 Windows 7 的電腦相容的內容資訊稱為第1版或 V1。 如果是 V1 BranchCache 檔案分割，檔案區段比 V2 大，而且為固定大小。 由於大型的固定區段大小，當使用者進行修改檔案長度的變更時，不只變更的區段失效，到檔案結尾的所有區段都會失效。 因此，下次分公司的其他使用者呼叫變更的檔案時，會縮減節省的 WAN 頻寬，因為會透過 WAN 連結傳送變更的內容以及變更後的所有內容。

- 與執行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012 和 Windows 8 的電腦相容的內容資訊稱為第2版或 V2。 V2 內容資訊使用較小且可變動大小的區段，較能容許檔案內的變更。 當使用者存取更新的版本時，這可以提升舊版檔案區段重複使用的可能性，讓他們只從內容伺服器擷取檔案中變更的部分，並使用較少的 WAN 頻寬。

下表根據您在 BranchCache 部署所使用的用戶端、內容伺服器和託管快取伺服器作業系統，提供內容資訊版本的相關資訊。

> [!NOTE]
> 在下表中，縮寫 "OS" 表示作業系統。

|用戶端作業系統|內容伺服器 OS|託管快取伺服器 OS|內容資訊版本|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 與 Windows 7 |Windows Server 2012 或更新版本|Windows Server 2012 或更新版本;分散式快取模式無|V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本|Windows Server 2008 R2 |Windows Server 2012 或更新版本;分散式快取模式無|V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本| Windows Server 2012 或更新版本| Windows Server 2008 R2 |V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本|Windows Server 2012 或更新版本|Windows Server 2012 或更新版本;分散式快取模式無|V2|

當您擁有執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的內容伺服器和託管快取伺服器時，它們會根據要求資訊的 BranchCache 用戶端作業系統，使用適當的內容資訊版本。

當執行 Windows Server 2012 和 Windows 8 或更新版本作業系統的電腦要求內容時，內容和託管快取伺服器會使用 V2 內容資訊;當執行 Windows Server 2008 R2 和 Windows 7 的電腦要求內容時，內容和託管快取伺服器會使用 V1 內容資訊。

>[!IMPORTANT]
>在分散式快取模式部署 BranchCache 時，使用不同內容資訊版本的用戶端不會彼此共用內容。 例如，執行 Windows 7 的用戶端電腦和執行 Windows 10 且安裝在相同分公司的用戶端電腦不會彼此共用內容。

## <a name="how-branchcache-handles-content-updates-in-files"></a><a name="bkmk_handles"></a>BranchCache 處理檔案中內容更新的方式

當分公司使用者修改或更新檔內容時，他們的變更會直接寫入總公司的內容伺服器，而不會有 BranchCache 的介入。 無論使用者是從內容伺服器下載文件，或是從分公司中的託管或分散式快取取得文件，都是如此。

分公司中的不同用戶端要求修改的檔案時，會從總公司伺服器下載檔案的新區段，並將其新增至該分公司的分散式或託管快取中。 因為這個緣故，分公司使用者一定會收到快取內容的最新版本。

## <a name="branchcache-installation-guide"></a><a name="BKMK_4"></a>BranchCache 安裝指南

您可以使用 Windows Server 2016 中的伺服器管理員來安裝 BranchCache 功能或檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務。 您可以使用下表，來決定是否要安裝角色服務或功能。

|功能|電腦位置|安裝這個 BranchCache 元素|
|-----------------|---------------------|------------------------------------|
|以內容伺服器 \( BITS 為基礎的應用程式伺服器\)|總公司或雲端資料中心|BranchCache 功能|
|內容伺服器 \( 網頁伺服器\)|總公司或雲端資料中心|BranchCache 功能|
|\(使用 SMB 通訊協定的內容伺服器檔案伺服器\)|總公司或雲端資料中心|檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務|
|託管快取伺服器|分公司|啟用託管快取伺服器模式的 BranchCache 功能|
|啟用 BranchCache 的用戶端電腦|分公司|不需要安裝;只需要 \( \) 在用戶端上啟用 BranchCache 和 branchcache 模式或託管|

若要安裝角色服務或功能，請開啟 [伺服器管理員]，然後選取您要啟用 BranchCache 功能的電腦。 在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [**新增角色及功能精靈**] 隨即開啟。 在執行精靈時，請選取下列項目：

- 在 [**選取安裝類型**] 精靈頁面上，選取 [**角色型或功能型安裝**]。

- 在嚮導頁面上， **選取 [伺服器角色**]，如果您要安裝啟用 BranchCache 的檔案伺服器，請展開 [檔案 **和存放服務** 和檔案 **和 iSCSI 服務**]，然後選取 [ **網路檔案的 BranchCache**]。  若要節省磁碟空間，您也可以選取 [ **重復資料刪除** ] 角色服務，然後繼續執行嚮導以安裝和完成。 如果您不想要安裝啟用 BranchCache 的檔案伺服器，請勿使用網路檔案角色服務的 BranchCache 安裝檔案和存放服務角色。

- 在 [嚮導] 頁面上， **選取 [功能**]，如果您要安裝非檔案伺服器的內容伺服器，或您正在安裝託管快取伺服器，請選取 [ **BranchCache**]，然後繼續執行嚮導以安裝和完成。 如果您不想要安裝檔案伺服器或託管快取伺服器以外的內容伺服器，請不要安裝 BranchCache 功能。

## <a name="operating-system-versions-for-branchcache"></a><a name="bkmk_os"></a>適用於 BranchCache 的作業系統版本

以下是支援不同 BranchCache 功能類型的作業系統清單。

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>適用於 BranchCache 用戶端電腦功能的作業系統

下列作業系統提供 BranchCache 支援背景智慧型傳送服務 (位) 、超文字傳輸通訊協定 (HTTP) 和伺服器訊息區 (SMB) 。

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 企業版

- Windows 7 Enterprise

- Windows 7 旗艦版

在下列作業系統中，BranchCache 不支援 HTTP 和 SMB 功能，但支援 BranchCache BITS 功能。

-   Windows 10 專業版，僅支援 BITS

-   Windows 8.1 Pro、BITS 僅支援

-   Windows 8 Pro、BITS 僅支援

-   僅限 Windows 7 Pro，BITS 支援

> [!NOTE]
> BranchCache 預設無法在 Windows Server 2008 或 Windows Vista 作業系統中使用。 不過，在這些作業系統上，如果您下載並安裝 Windows Management Framework 更新，BranchCache 功能僅適用于背景智慧型傳送服務 (BITS) 通訊協定。 如需詳細資訊及下載 Windows Management Framework，請參閱 [Windows Management Framework (Windows PowerShell 2.0、WinRM 2.0 和 BITS 4.0) ](https://go.microsoft.com/fwlink/?LinkId=188677) https://go.microsoft.com/fwlink/?LinkId=188677 。

### <a name="operating-systems-for-branchcache-content-server-functionality"></a>適用於 BranchCache 內容伺服器功能的作業系統

您可以使用 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 系列的作業系統作為 BranchCache 內容伺服器。

此外，Windows Server 2008 R2 系列的作業系統可以做為 BranchCache 內容伺服器使用，但有下列例外狀況：

- 在具有 Hyper-v 的 Windows Server 2008 R2 Enterprise 的 Server Core 安裝中不支援 BranchCache。

- 在具有 Hyper-v 的 Windows Server 2008 R2 Datacenter 的伺服器核心安裝中，不支援 BranchCache。

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>適用於 BranchCache 託管快取伺服器功能的作業系統

您可以使用 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 系列的作業系統做為 BranchCache 託管快取伺服器。

此外，下列 Windows Server 2008 R2 作業系統可做為 BranchCache 託管快取伺服器使用：

- Windows Server 2008 R2 Enterprise

- 具有 Hyper-v 的 Windows Server 2008 R2 企業版

- Windows Server 2008 R2 Enterprise Server Core 安裝

- 使用 Hyper-v 的 Windows Server 2008 R2 Enterprise Server Core 安裝

- 適用於 Itanium 型系統的 Windows Server 2008 R2

- Windows Server 2008 R2 Datacenter

- 具有 Hyper-v 的 Windows Server 2008 R2 Datacenter

- 使用 Hyper-v 的 Windows Server 2008 R2 Datacenter Server Core 安裝

## <a name="branchcache-security"></a><a name="bkmk_security"></a>BranchCache 安全性

BranchCache 實作是一種設計保護安全的方法，順利地與現有的網路安全性架構搭配使用，不需要額外的設備或複雜的安全性設定。

BranchCache 屬於非侵入性功能，不會更改任何 Windows 驗證或授權程序。 部署 BranchCache 之後，仍會使用網域認證執行驗證，而且使用存取控制清單 (ACL) 功能授權的方式不變。 此外，其他設定會繼續運作，如同部署 BranchCache 之前的運作方式。

BranchCache 安全性模式是以建立的中繼資料為基礎，採取一系列雜湊的形式。 這些雜湊也稱為內容資訊。

內容資訊建立之後，便可用於 BranchCache 訊息交換，而非使用實際資料，並使用支援的通訊協定 (HTTP、HTTPS 以及 SMB) 來進行交換。

快取的資料會保持加密，用戶端若沒有從原始來源存取內容的權限，就無法加以存取。  用戶端必須先經過原始內容來源驗證和授權，才能抓取內容中繼資料，還必須擁有內容中繼資料，才能存取當地辦公室中的快取。

### <a name="how-branchcache-generates-content-information"></a>BranchCache 如何產生內容資訊

因為內容資訊是從多個元素建立，所以內容資訊的值永遠是獨一無二。 這些元素如下：

- 從中衍生雜湊的實際內容 (例如網頁或共用檔案)。

- 設定參數，例如雜湊演算法與區塊大小。 若要產生內容資訊，內容伺服器會將內容分割成區段，然後再將那些區段分割成區塊。 BranchCache 使用安全的密碼編譯雜湊，來識別和確認每個區塊與區段都支援 SHA256 雜湊演算法。

- 伺服器密碼。 所有內容伺服器都必須使用伺服器密碼 (任意長度的二進位值) 進行設定。

> [!NOTE]
> 使用伺服器密碼可確保用戶端電腦不會自行產生內容資訊。 這樣可以在用戶端擁有舊版存取權，但無法存取目前版本的情況下，避免惡意使用者對啟用 BranchCache 用戶端電腦進行暴力密碼破解攻擊，猜出兩個版本內容間的些微變更。

### <a name="content-information-details"></a>內容資訊詳細資料

BranchCache 使用伺服器密碼做為金鑰，以衍生要傳送給授權用戶端的內容特定雜湊。 將雜湊演算法套用到合併的伺服器密碼，資料雜湊就可以產生這個雜湊。

這個雜湊稱為區段密碼。 BranchCache 使用區段密碼來保護通訊安全。 此外，BranchCache 會建立區塊雜湊清單 (雜湊的資料區塊清單)，以及由雜湊的資料區塊清單產生的資料雜湊。

內容資訊包括下列各項：

- 區塊雜湊清單：

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- 資料雜湊 (HoD)：

    `HoD = Hash(BlockHashList)`

- 區段密碼 (Kp)：

    `Kp = HMAC(Ks, HoD)`

BranchCache 使用同儕節點內容快取通訊協定與抓取架構通訊協定來實作必要的程序，以確保可在內容快取之間安全地快取和抓取資料。

此外，BranchCache 用以處理內容資訊的安全性等級，和它自己在處理和傳輸實際內容時的安全性等級相同。

## <a name="content-flow-and-processes"></a><a name="bkmk_flow"></a>內容流程與程序

內容資訊與實際內容的流程分成下列四個階段：

1.  [BranchCache 程序：要求內容](#BKMK_8)

2.  [BranchCache 程序：找出內容](#BKMK_9)

3.  [BranchCache 程序：抓取內容](#BKMK_10)

4.  [BranchCache 程序：快取內容](#BKMK_11)

下列各節會說明這些階段。

## <a name="branchcache-processes-request-content"></a><a name="BKMK_8"></a>BranchCache 程序：要求內容

在第一個階段中，分公司的用戶端電腦會向遠端位置 (例如總公司) 的內容伺服器要求內容，例如檔案或網頁。 內容伺服器會確認已授權該用戶端電腦接收要求的內容。 如果用戶端電腦已獲授權，而且內容伺服器和用戶端都 \- 啟用 BranchCache，則內容伺服器會產生內容資訊。

內容伺服器接著會使用和傳送實際內容時所使用的相同通訊協定，將內容資訊傳送至用戶端電腦。

例如，如果用戶端電腦透過 HTTP 要求網頁，內容伺服器就會使用 HTTP 來傳送內容資訊。 因此，內容與內容資訊的有線等級安全性保證完全相同。

在收到初始的內容資訊 (資料雜湊 + 區段密碼) 部分之後，用戶端電腦會執行下列動作：

- 使用區段密碼 (Kp) 做為加密金鑰 (Ke)。

- 從 HoD 與 Kp 產生區段識別碼 (HoHoDk)：

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

這層的主要威脅在於區段密碼所面對的風險，不過 BranchCache 會加密內容資料區塊，以保護區段密碼。 BranchCache 的做法是在內容區塊所在的位置內，使用從內容區段的區段密碼所衍生的加密金鑰。

這個方法可確保讓沒有伺服器密碼的實體，無法探索資料區塊中的實際內容。 區段密碼會以和純文字區段本身相同的安全性等級來處理，因為只要知道指定區段的區段密碼，就可以讓該實體從同儕節點取得該區段，然後加以解密。 知道伺服器密碼並不會立即產生任何特定純文字，但可以用來從加密文字衍生特定的資料類型，然後可能將一些部分已知的資料洩漏給暴力密碼破解攻擊。 因此，伺服器密碼應該保密。

## <a name="branchcache-processes-locate-content"></a><a name="BKMK_9"></a>BranchCache 程序：找出內容

在用戶端電腦收到內容資訊之後，該用戶端會使用區段識別碼在分公司快取中找出要求的內容，不論該快取是分散在用戶端電腦之間，或是位於託管快取伺服器上。

如果用戶端電腦設定為託管快取模式，就會設定託管快取伺服器的電腦名稱，並連絡該伺服器來抓取內容。

不過，如果用戶端電腦設定為分散式快取模式，該內容可能會儲存在分公司中多部電腦的多個快取中。 用戶端電腦必須先探索內容的所在位置，才能抓取該內容。

在設定為分散式快取模式時，用戶端電腦會使用以 Web 服務動態探索 (WS-Discovery) 通訊協定為基礎的探索通訊協定，來找出內容。 用戶端會透過網路傳送 WS-Discovery 多點傳送 Probe 訊息，來探索快取的內容。 Probe 訊息包括區段識別碼，讓用戶端檢查要求的內容是否與儲存在快取中的內容相符。 如果區段識別碼符合本機快取的內容，收到初始 Probe 訊息的用戶端，就會使用單點傳播 Probe-Match 訊息來回覆查詢用戶端。

對於要求的內容而言，WS-Discovery 程序的成功與否，取決於執行探索的用戶端是否擁有內容伺服器所提供的正確內容資訊。

在要求內容階段期間，資料的主要威脅是資訊洩漏，因為存取內容資訊意指授權存取內容。 若要減低這項風險，探索程序不可以顯示區段識別碼以外的內容資訊，才不會顯示任何有關包含內容的純文字區段。

此外，惡意使用者在相同網路子網路上執行的其他用戶端電腦，可以看到經過路由器的原始內容來源的 BranchCache 探索流量。

如果在分公司找不到要求的內容，用戶端會透過 WAN 連結直接向內容伺服器要求該內容。

會在收到內容之後，新增至用戶端電腦或是託管快取伺服器上的本機快取。 在這種情況下，內容資訊可以避免用戶端或託管快取伺服器將不符合雜湊的任何內容新增至本機快取。 比對雜湊以確認內容的程序可確保只會將有效的內容新增至快取，並可保護本機快取的完整性。

## <a name="branchcache-processes-retrieve-content"></a><a name="BKMK_10"></a>BranchCache 程序：抓取內容

用戶端電腦在內容主機 (託管快取伺服器或分散式快取模式用戶端電腦) 上找到所需的內容之後，用戶端電腦就會開始進行抓取內容的程序。

首先，用戶端電腦會向內容主機傳送要求，以取得所需的第一個區塊。 該要求包含區段識別碼，以及可識別所需內容的區塊範圍。 因為只會傳回一個區塊，所以區塊範圍只包含單一區段。 目前不支援多個區塊 (要求。 ) 用戶端也會將要求儲存在其本機未處理的要求清單中。

從用戶端收到有效的要求訊息時，內容主機會檢查要求中所指定的區塊是否存在於內容主機的內容快取中。

如果內容主機擁有該內容區塊，接著內容主機就會傳送回應，其中包含該區段識別碼、區塊識別碼、加密的資料區塊，以及用以加密該區塊的初始向量。

如果內容主機沒有該內容區塊，內容主機就會傳送空白的回應訊息。 這樣可通知用戶端電腦，該內容主機並沒有要求的區塊。 空白的回應訊息包含區段識別碼與所要求區塊的區塊識別碼，以及大小為零的資料區塊。

用戶端電腦收到內容主機的回應時，用戶端會確認該訊息對應至其未處理的要求清單中的要求訊息。 (區段識別碼與區塊索引必須與未處理的要求相符。)

如果這個驗證程序未成功，而且用戶端電腦的未處理要求清單中沒有對應的要求訊息，用戶端電腦就會捨棄該訊息。

如果這個驗證程序成功，而且用戶端電腦的未處理要求清單中有對應的要求訊息，用戶端電腦就可以解密該區塊。 接著，用戶端會針對一開始從原始內容伺服器取得之內容資訊中的適當區塊雜湊，來驗證解密的區塊。

如果區塊驗證成功，就會在快取中儲存解密的區塊。

這個程序會重複執行，直到用戶端擁有所需的所有區塊。

> [!NOTE]
> 如果電腦上不存在完整的內容區段，抓取通訊協定就會從來源組合中抓取和組合內容：一組分散式快取模式用戶端電腦、託管快取伺服器，以及總公司中的原始內容伺服器 (如果分公司快取未含有完整的內容)。

資料必須先加密，BranchCache 才能傳送內容資訊或內容。 BranchCache 會在回應訊息中加密區塊。 在 Windows 7 中，BranchCache 使用的預設加密演算法是 AES-128，加密金鑰是 Ke，而金鑰大小是128位，如同加密演算法所規定。

BranchCache 會產生適用於加密演算法的初始化向量，並使用加密金鑰來加密區塊。 然後，BranchCache 會在訊息中記錄加密演算法與初始化向量。

伺服器與用戶端永遠不會交換、共用或彼此傳送加密金鑰。 用戶端會從裝載來源內容的內容伺服器收到加密金鑰。 然後使用加密演算法與從伺服器收到的初始化向量來解密區塊。 下載通訊協定中未內建任何明確的驗證或授權。

### <a name="security-threats"></a>安全性威脅

這層的主要安全性威脅包括：

- 竄改資料：

  *提供資料給要求者的用戶端竄改資料*。 BranchCache 安全性模式使用雜湊確認用戶端或伺服器是否均未更改資料。

- 資訊洩漏：

    *BranchCache 傳送指定適當區段識別碼的加密內容給任何用戶端*。 區段識別碼是公用的，因此任何用戶端都可以接收加密的內容。 不過，如果惡意使用者取得加密內容，他們必須知道加密金鑰才能解密內容。 上層通訊協定會執行驗證，然後指定內容資訊給驗證和授權的用戶端。 內容資訊的安全性等同於提供給內容本身的安全性，而且 BranchCache 絕不會洩露內容資訊。

    *攻擊者窺探連線以取得內容*。  BranchCache 會使用密碼金鑰為 Ke 的 AES128 加密用戶端之間的所有傳輸，避免連線遭到窺探。  以和保護資料本身完全相同的方式，保護從內容伺服器下載的內容資訊，而且保護資訊洩露的程度相當於完全未使用 BranchCache。

-   阻斷服務：

    *資料的要求塞爆用戶端*。 BranchCache 通訊協定併入佇列管理計數器與計時器，避免用戶端超載。

## <a name="branchcache-processes-cache-content"></a><a name="BKMK_11"></a>BranchCache 程序：快取內容

在分散式快取模式上，位於分公司的用戶端電腦與託管快取伺服器，內容快取是隨時間慢慢透過 WAN 連結抓取內容來建置。

用戶端電腦設定為託管快取模式時，會將內容新增至它自己的本機快取，也會將資料提供給託管快取伺服器。 託管快取通訊協定提供機制，讓用戶端通知託管快取伺服器有關內容與區段可用性。

若要將內容上傳至託管快取伺服器，用戶端要通知伺服器有可用的區段。 接著，託管快取伺服器會抓取與提供的區段相關聯的所有內容資訊，並下載區段內實際所需的區塊。 這個程序會重複執行，直到用戶端沒有可提供給託管快取伺服器的區段為止。

若要使用託管快取通訊協定來更新託管快取伺服器，必須符合下列需求：

- 用戶端電腦必須要有區段內的一組區塊，可提供給託管快取伺服器。 用戶端必須要能針對提供的區段提供內容資訊；這是由區段識別碼、區段資料雜湊、區段密碼以及區段內含的所有區塊雜湊清單所構成。

- 若為執行 Windows Server 2008 R2 的託管快取伺服器，則需要託管快取伺服器憑證和相關聯的私密金鑰，而且發行憑證 (CA) 的憑證授權單位單位必須由分公司的用戶端電腦所信任。 這樣可讓用戶端與伺服器成功參與 HTTPS 伺服器驗證。

    > [!IMPORTANT]
    > 執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的託管快取伺服器，不需要託管快取伺服器憑證和相關聯的私密金鑰。

- 用戶端電腦已設定託管快取伺服器的電腦名稱，以及託管快取伺服器接聽 BranchCache 流量的傳輸控制通訊協定 (TCP) 連接埠號碼。 託管快取伺服器的憑證會系結至此埠。 如果託管快取伺服器是網域成員電腦，託管快取伺服器的電腦名稱就可以是完整網域名稱 (FQDN)；如果託管快取伺服器不是網域成員，則可以是電腦的 NetBIOS 名稱。

- 用戶端電腦會主動接聽連入的區塊要求。 正在接聽的連接埠會做為供應項目訊息的一部分，從用戶端傳送至託管快取伺服器。 這樣可讓託管快取伺服器使用 BranchCache 通訊協定，連線到用戶端電腦來抓取區段中的資料區塊。

- 託管快取伺服器在初始化之後，開始接聽連入的 HTTP 要求。

- 如果設定託管快取伺服器要求用戶端電腦驗證，用戶端與託管快取伺服器兩者需要支援 HTTPS 驗證。

### <a name="hosted-cache-mode-cache-population"></a>託管快取模式快取擴展

當用戶端傳送包含區段識別碼的 INITIAL_OFFER_MESSAGE 時，在分公司將內容新增至託管快取伺服器快取的程式隨即開始。 INITIAL_OFFER_MESSAGE 要求中的區段識別碼是用來從託管快取伺服器的區塊快取取出資料的對應區段雜湊、區塊雜湊清單和區段秘密。 如果託管快取伺服器已有特定區段的所有內容資訊，INITIAL_OFFER_MESSAGE 的回應會是 OK，而且不會再有下載區塊的要求。

如果託管快取伺服器還沒有所有提供的資料區塊 (其與區段的區塊雜湊相關聯)，INITIAL_OFFER_MESSAGE 的回應會是 INTERESTED。 接著，用戶端會傳送描述所提供單一區段的 SEGMENT_INFO_MESSAGE。 託管快取伺服器會以 OK 訊息回應，然後開始從提供的用戶端電腦下載缺少的區塊。

區段資料雜湊、區塊雜湊清單以及區段密碼是用來確保下載的內容未經竄改或有所更改。 接著會將下載的區塊新增至託管快取伺服器的區塊快取。

## <a name="cache-security"></a><a name="bkmk_cache"></a>快取安全性
本節提供 BranchCache 如何在用戶端電腦與託管快取伺服器上保護快取資料的相關資訊。

### <a name="client-computer-cache-security"></a>用戶端電腦快取安全性
對於以 BranchCache 儲存的資料而言，最大的威脅就是遭到竄改。 如果攻擊者能夠竄改內容與快取中儲存的內容資訊，就可能會用來對使用 BranchCache 的電腦試圖發動攻擊。 攻擊者可以插入惡意軟體取代其他資料來開始發動攻擊。 BranchCache 可以使用在內容資訊中找到的區塊雜湊來驗證所有內容，藉以降低這項威脅。 如果攻擊者嘗試竄改這個資料，就會予以捨棄，並以原始來源的有效資料取代。

對於以 BranchCache 儲存的資料而言，次要的威脅就是資訊洩露。 在分散式快取模式中，用戶端只會快取它自己要求的內容；不過，該資料是以純文字儲存，而可能有風險存在。 為了協助限制只能快取存取 BranchCache 服務，本機快取是由 ACL 中指定的檔案系統權限保護。

雖然 ACL 在避免未經授權的使用者存取快取這方面很有成效，但擁有系統管理權限的使用者，可能以手動變更 ACL 中指定的權限，來取得快取的存取權。 BranchCache 無法保護惡意使用系統管理帳戶的狀況。

儲存在內容快取的資料並未加密，因此如果擔心資料洩露，您可以使用加密技術，例如 BitLocker 或加密檔案系統 (EFS)。 BranchCache 使用的本機快取不會增加分公司電腦所承受的資訊洩露威脅；該快取只包含以未加密形式存在其他位置的磁碟上的檔案複本。

若難以在環境中確保用戶端的實體安全性，加密整個磁碟是非常重要的。 例如，加密整個磁碟可以協助保護可能從分公司環境中移除的行動電腦上的敏感資料。

### <a name="hosted-cache-server-cache-security"></a>託管快取伺服器快取安全性

在託管快取模式中，對於託管快取伺服器安全性而言，最大的威脅是資訊洩露。 處於託管快取環境中的 BranchCache 行為類似於分散式快取模式的方式，利用系統檔案權限來保護快取的資料。 不同之處在於，託管快取伺服器儲存分公司中任何啟用 BranchCache 的電腦要求的所有內容，而不是只有單一用戶端要求的資料。 未經授權入侵這個快取的後果可能十分嚴重，因為會讓更多資料暴露在風險中。

在託管快取伺服器執行 Windows Server 2008 R2 的託管快取環境中，如果分公司的任何用戶端可以透過 WAN 連結存取敏感性資料，建議使用加密技術（例如 BitLocker 或 EFS）。 還務必要避免實際存取託管快取的狀況，因為當攻擊者取得實際存取權時，只有關閉電腦，磁碟加密才有效用。  如果電腦處於開啟狀態或睡眠模式，磁碟加密所能提供的保護極少。

> [!NOTE]
> 執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的託管快取伺服器，預設會加密快取中的所有資料，因此不需要使用其他加密技術。

即使用戶端設定為託管快取模式，仍會在本機快取資料，而除了託管快取伺服器上的快取，您可能也要採取步驟來保護本機快取。