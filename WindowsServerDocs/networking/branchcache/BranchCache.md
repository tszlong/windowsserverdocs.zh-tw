---
title: BranchCache
description: 本主題提供 Windows Server 2016 中 BranchCache 的概觀
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5540d89827b80a21bf23f6a2aa8f54f09dfde67a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache"></a>BranchCache

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題，適用於資訊技術 (IT) 專業人員，提供概觀 BranchCache，包括 BranchCache 模式、的功能、功能，以及 BranchCache 功能，可在不同的作業系統資訊。

> [!NOTE]
> 本主題中，除了下列 BranchCache 文件會提供。
> 
> - [殼層 BranchCache 網路及 Windows PowerShell 命令](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [BranchCache 部署指南](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**誰會想要 BranchCache？**

如果您是系統管理員、網路、儲存方案架構或其他 IT 專業人員，BranchCache 您可能感興趣下列下底下：

- 設計，或從分公司具有兩個或更多的所在位置和寬區域 (wan) 連接到主要辦公室組織的支援 IT 基礎結構。

- 您設計或組織已部署雲端技術支援 IT 基礎架構與 WAN 連接使用遠端應用程式存取的資料與工作人員。

- 您想要最佳化 WAN 的頻寬，來減少分公司和主要辦公室間網路流量。

- 您有部署或計畫部署內容伺服器主要辦公室符合本主題中所述的設定。

- 在您分公司 client 的電腦執行 Windows 10、Windows 8.1、Windows 8 或 Windows 7。

本主題包含下列的區段：

-   [BranchCache 為何？](#bkmk_what)

-   [BranchCache 模式](#BKMK_2)
  
-   [BranchCache 式內容伺服器](#BKMK_3)
  
-   [BranchCache 和雲端](#BKMK_3a)
  
-   [內容資訊版本](#bkmk_version)  
  
-   [如何 BranchCache 處理 content 更新中的檔案](#bkmk_handles)  
  
-   [BranchCache 安裝指南](#BKMK_4)  
  
-   [作業系統版本 BranchCache](#bkmk_os)  
  
-   [BranchCache 安全性](#bkmk_security)  
  
-   [內容流程和處理程序](#bkmk_flow)  
  
-   [快取安全性](#bkmk_cache)  
  
## <a name="bkmk_what"></a>BranchCache 為何？

BranchCache 是隨附於某些版本的 Windows Server 2016 和 Windows 10 作業系統、以及於某些版本的 Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 的寬形區域網路 (WAN) 頻寬最佳化技術。 若要最佳化 WAN 的頻寬，當使用者存取 content 遠端伺服器上的，BranchCache 從您的主要辦公室擷取 content 或裝載的雲端內容伺服器和快取 content 分公司的位置，在電腦本機而不是透過 WAN 存取 content 分公司允許 client。
  
分公司，在 content 會儲存到快取的主機或，不伺服器提供分公司 client 電腦正在執行 Windows 10、Windows 8.1、Windows 8 或 Windows 7 中設定伺服器上。 Client 電腦要求 content 接收主要辦公室及 content 分公司快取之後，相同分公司其他電腦就可以取得 content 本機而不是從內容伺服器下載 content WAN 連結。

當 client 電腦所做的相同 content 後續的要求時，戶端下載*內容資訊*來自而不是實際 content 伺服器。 使用的原始 content，區塊計算，而且非常小相較於在原始資料 content hashes 所組成內容資訊。 位於 client 的電腦上或在伺服器上的快取是否 client 電腦然後中找不到快取中的 content 分公司使用內容資訊。 Client 電腦」和「伺服器也保護快取的 content，使其無法存取未經授權的使用者使用內容資訊。

BranchCache 增加使用者生產力改善內容查詢回應時間戶端和中分公司伺服器，也可以協助改善網路效能降低流量透過 WAN 連結。

## <a name="BKMK_2"></a>BranchCache 模式
BranchCache 有兩種模式的作業：散發快取模式和裝載快取模式。

當您在分散式快取模式部署 BranchCache 時，讓分公司內容快取分散在 client 的電腦。

當您在裝載快取模式部署 BranchCache 時，裝載內容快取分公司在上一個或更多伺服器電腦，稱為「裝載快取的伺服器。

> [!NOTE]
> 您可以部署 BranchCache 使用這兩種模式，但只能有一個模式可以使用每個分公司。 例如，如果您有兩個分公司一個擁有伺服器，其中不，這您就可以裝載快取模式包含伺服器，同時部署 BranchCache 分散式快取 office，其中包含只有 client 電腦模式中的 office 部署 BranchCache。

下圖，BranchCache 部署在這兩種模式。  

![BranchCache 模式](../media/BranchCache/bc_modes.jpg)

快取分散式的模式最適合小分公司不包含作為裝載快取伺服器本機伺服器。 快取分散式的模式可讓您在分公司額外的硬體與部署 BranchCache。

您想要用來部署 BranchCache 分公司包含額外的基礎結構，例如一或多部執行其他工作負載，部署 BranchCache 模式裝載快取中有幫助的原因如下：

### <a name="increased-cache-availability"></a>增加快取的可用性

裝載快取模式增加效率快取因為 content 使用即使離線 client 原始要求而且快取的資料。 因為都可使用裝載快取伺服器，更多 content 快取，可提供更大節省 WAN 的頻寬，並改進 BranchCache 效率。

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>集中多子網路分公司快取


快取分散式的模式單一子網路上的運作方式。 在多個子網路分公司已設定為 [快取分散式的模式，無法與 client 電腦上其他子網路共用檔案下載至一個子網路。 

因為戶端上其他子網路，無法探索您的已下載的檔案，檔案從主要辦公室內容伺服器取得，使用此程序 WAN 頻寬。

在部署裝載快取模式時，但不是-分公司多子網路中的所有戶端可以都存取儲存在裝載快取伺服器上，即使戶端不同子網路上的單一快取。 此外，Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 中的 BranchCache 提供部署分公司每一部以上的裝載快取伺服器的能力。

> [!CAUTION]
> 如果您使用 BranchCache SMB 快取的檔案和資料夾，做停用 Offline 檔案。 如果您停用 Offline 檔案，BranchCache SMB 快取運作不正常運作。

## <a name="BKMK_3"></a>BranchCache 式內容伺服器

當您部署 BranchCache 時，來源儲存 BranchCache 式內容伺服器中主要辦公室或雲端的資料中心。 BranchCache 所支援的內容伺服器下列類型：

> [!NOTE]
> 僅限來源 content-也就是內容 client 電腦最初獲得 BranchCache 式內容伺服器-是 BranchCache 來加速。 Content client 電腦取得直接從其他來源，例如 Windows Update 或網際網路上的網頁伺服器可 client 的電腦不會快取裝載快取伺服器或與其他電腦中分公司然後共用。 如果您想要加速 content Windows 更新，但是，您可以安裝應用程式在您的主要辦公室或雲端的資料中心 Windows Server Update Services (WSUS) 伺服器並將其設定為 BranchCache 內容伺服器。

### <a name="web-servers"></a>網頁伺服器

支援的網頁伺服器包含的電腦正在執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的已安裝的網頁伺服器 (IIS) 伺服器角色，也會使用超傳輸通訊協定 (HTTP) 或 HTTP 安全 (HTTPS)。

此外，網頁伺服器必須安裝 BranchCache 功能。

### <a name="file-servers"></a>檔案伺服器

支援的檔案伺服器包括的電腦是執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 檔案服務伺服器角色與 BranchCache 適用於已安裝的角色服務網路的檔案。 

這些檔案伺服器換貨資訊電腦之間，使用伺服器訊息區 (SMB)。 完成您的檔案伺服器的安裝之後，您必須也共用資料夾，以及 hash 代的共用資料夾，可讓 BranchCache 使用群組原則」或「本機電腦的原則。

### <a name="application-servers"></a>應用程式伺服器

支援的應用程式伺服器包含電腦正在執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012，或安裝 Windows Server 2008 R2 的背景聰明傳輸服務（位元），並支援。 

此外，應用程式伺服器必須安裝 BranchCache 功能。 做為範例的應用程式的伺服器，您可以為 BranchCache 內容伺服器部署 Microsoft Windows Server Update Services (WSUS) 和 Microsoft 系統 Center Configuration Manager 分支 Distribution 點伺服器。

## <a name="BKMK_3a"></a>BranchCache 和雲端

雲端龐大可能降低費用，並達到新層級的縮放比例，但移動工作負載原位依賴這些人可以增加網路成本和傷害生產力。 使用者預期高效能並不保養放置應用程式和資料。 

BranchCache 可以改進的應用程式網路效能，並減少頻寬耗用量的共用快取的資料。  它可以改善生產力分公司及總部，背景工作的位置在雲端中使用伺服器上的部署。

因為 BranchCache 不需要新的硬體或網路拓撲變更，是很好方案改善辦公室位置與公開和私人雲朵之間的通訊。

> [!NOTE]
> 因為某些 Web proxy 無法處理標準 Content 編碼標頭，建議使用 BranchCache 不 HTTP 與超文字傳輸通訊協定安全 (HTTPS)。
  
=== 適用於 Windows Server 2016 中的雲端技術的詳細資訊，請查看[軟體定義網路與 #40;SDN 與 #41;](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>內容資訊版本

有兩個版本的內容資訊：

- 與執行 Windows Server 2008 R2 或 Windows 7 相容內容資訊稱為版本 1 或 V1。 V1 BranchCache 檔案區段，以檔案區段大於 V2，且有修正的大小。 大型修正的區段大小，而當使用者修改檔案長度的變更，這項變更區段失效，不僅失效所有的區段結尾的檔案。 下一步通話的另一位使用者分公司中變更檔案因此會導致降低 WAN 頻寬節省因為變更的 content 與所有 content 變更後的傳送 WAN 連結。

- 版本 2 或 V2，稱為內容與執行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012，以及 Windows 8 電腦的資訊。 V2 內容資訊使用較小、變數大小更具彈性變更檔案中的區段。 這可以重複使用較舊版本的檔案從區段的可能性，當使用者存取更新的版本，造成這些變更的部分檔案的擷取內容伺服器，並使用較少 WAN 的頻寬增加。

下表提供使用的 client，內容伺服器，而定內容資訊版本的相關資訊，並裝載快取伺服器作業系統您使用 BranchCache 部署。

> [!NOTE]
> 下表中的縮寫」OS」表示作業系統。

|Client 作業系統|內容伺服器作業系統|裝載快取伺服器作業系統|內容資訊版本|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 和 Windows 7 |Windows Server 2012 或更新版本|Windows Server 2012 或更新版本;無分散式快取模式|V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本|Windows Server 2008 R2 |Windows Server 2012 或更新版本;無分散式快取模式|V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本| Windows Server 2012 或更新版本| Windows Server 2008 R2 |V1|
|Windows Server 2012 或更新版本;Windows 8 或更新版本|Windows Server 2012 或更新版本|Windows Server 2012 或更新版本;無分散式快取模式|V2|

有內容伺服器裝載快取伺服器正在執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 時，他們使用內容資訊版本適合根據要求資訊 BranchCache client 的作業系統。 

內容和裝載快取伺服器當電腦執行的是 Windows Server 2012 和 Windows 8 或更新版本作業系統要求 content 時，使用 V2 內容資訊。當要求時 content 執行 Windows Server 2008 R2 和 Windows 7 的電腦，內容和裝載快取伺服器使用 V1 內容資訊。

>[!IMPORTANT]
>當您 BranchCache 部署分散式快取模式時，使用不同的內容資訊版本戶端不要共用 content 彼此。 例如，client 電腦執行的是 Windows 7 與 client 電腦執行相同分公司在已安裝 Windows 10 不分享 content 彼此。
  
## <a name="bkmk_handles"></a>如何 BranchCache 處理 content 更新中的檔案

時分公司使用者修改或更新的文件到，他們的變更會直接寫入中主要辦公室 BranchCache 的參與程度而內容伺服器。 使用者是否從內容伺服器下載文件或取得的其中一個裝載或分散快取中分公司也是如此。

修改的檔案，在分公司不同 client 要求時，檔案的新的區段中主要辦公室伺服器下載並新增至該新分支」中分散式或裝載快取。 因為，分公司使用者永遠收到快取 content 的最新版本。

## <a name="BKMK_4"></a>BranchCache 安裝指南

您可以使用在 Windows Server 2016 伺服器管理員安裝 BranchCache 功能或服務檔案伺服器角色網路檔案角色服務 BranchCache。 若要判斷是否要安裝的角色服務或功能，您可以使用下表。

|功能|電腦的位置|安裝這個 BranchCache 項目|
|-----------------|---------------------|------------------------------------|
|內容伺服器 \ (位元為基礎的應用程式 server\)|主要辦公室或雲端的資料中心|BranchCache 功能|
|內容伺服器 \(Web server\)|主要辦公室或雲端的資料中心|BranchCache 功能|
|內容伺服器 \（使用 SMB protocol\ 檔案伺服器）|主要辦公室或雲端的資料中心|BranchCache 網路檔案角色服務檔案服務伺服器角色|
|伺服器裝載快取|分公司|BranchCache 功能的支援裝載快取伺服器模式|
|BranchCache 式 client 電腦|分公司|不需; 安裝只要 BranchCache 與 BranchCache 模式可讓 \(distributed or hosted\) client 上|

若要安裝的角色服務或功能，打開伺服器管理員並選取您想要讓 BranchCache 功能的電腦。 在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 **新增角色與功能**開啟精靈。 為您執行精靈中，進行下列選項：

- 精靈頁面上的**選擇安裝類型**，請選取**以角色為基礎，或為基礎的功能的安裝**。

- 精靈頁面上的**選取伺服器角色**，如果您正在安裝功能 BranchCache 檔案伺服器，展開 [**檔案與儲存空間服務**和**檔案和 iSCSI 服務**，，然後選取 [ **BranchCache 網路的檔案**。  若要節省磁碟空間，您也可以選取**資料 Deduplication**角色服務，然後再安裝並完成精靈。 如果您不要安裝伺服器 BranchCache 支援的檔案，不要安裝檔案與儲存空間服務角色 BranchCache 網路檔案角色服務使用。

- 精靈頁面上的**選擇功能**，如果您要安裝的不是檔案伺服器內容伺服器，或您要安裝裝載快取伺服器、選取**BranchCache**，然後繼續安裝並完成精靈。 如果您不要安裝內容伺服器檔案伺服器或裝載快取伺服器以外，無法安裝 BranchCache 功能。
  
## <a name="bkmk_os"></a>作業系統版本 BranchCache

以下是作業系統的一份不同類型的 BranchCache 功能的支援。

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>作業系統 BranchCache client 電腦功能

在下列作業系統提供 BranchCache 背景聰明傳輸服務（位元）、超文字傳輸通訊協定 (HTTP) 和伺服器訊息區 (SMB) 的支援。

- Windows 10 企業版

- Windows 10 教育版

- Windows 8.1 企業版

- Windows 8 企業版

- Windows 7 企業版

- Windows 7 旗艦版

在下列作業系統中，BranchCache 不支援 HTTP 和 SMB 功能，但支援 BranchCache 位元的功能。

-   Windows 10 專業版、位元支援只

-   Windows 8.1 專業版，位元支援只

-   Windows 8 專業版、位元支援只

-   Windows 7 專業版，位元支援只

> [!NOTE]
> 無法使用 Windows Server 2008 或 Windows Vista 的作業系統中的預設 BranchCache。 在下列作業系統中，但是，如果您下載並安裝 Windows 管理架構的更新，BranchCache 功能適用於只背景聰明傳輸服務（位元）通訊協定。 如需詳細資訊，並下載 Windows Management Framework，請查看[Windows Management Framework（Windows PowerShell 2.0，WinRM 2.0，以及 4.0 位元）](https://go.microsoft.com/fwlink/?LinkId=188677)，https://go.microsoft.com/fwlink/?LinkId=188677。
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>作業系統 BranchCache 內容伺服器功能

您可以使用 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 系列的作業系統為 BranchCache 內容伺服器。

此外，Windows Server 2008 R2 家庭的作業系統可做為 BranchCache 內容伺服器，使用下列例外：

- 不支援的 Windows Server 2008 R2 企業版與 HYPER-V Server Core 安裝 BranchCache。

- 不支援的 Windows Server 2008 R2 Datacenter 與 HYPER-V Server Core 安裝 BranchCache。

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>作業系統 BranchCache 裝載快取伺服器功能

您可以使用作業系統的 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 系列 BranchCache 裝載快取的伺服器。

此外，BranchCache 裝載快取的伺服器，可以使用下列 Windows Server 2008 R2 作業系統：

- Windows Server 2008 R2 企業版

- HYPER-V 與 Windows Server 2008 R2 企業版

- Windows Server 2008 R2 企業 Server Core 安裝

- Windows Server 2008 R2 企業 Server Core 安裝 HYPER-V 使用

- 適用於 Itanium 型系統的 Windows Server 2008 R2

- Windows Server 2008 R2 Datacenter

- HYPER-V 與 Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter Server Core 安裝 HYPER-V 使用

## <a name="bkmk_security"></a>BranchCache 安全性

BranchCache 實作完美地搭配您現有的網路安全性架構，而不需要額外的設備或其他複雜的安全性設定的安全-設計的方法。
  
非侵入式 BranchCache 且不會變更任何 Windows 驗證或授權處理程序。 部署 BranchCache 之後，驗證仍然會使用網域憑證，並不變功能的授權的存取清單控制項 (Acl) 中的方式。 此外，繼續運作，就像往常 BranchCache 部署其他設定。

BranchCache 安全性模型根據建立 hashes 一系列的形式中繼資料。 這些 hashes 也稱為內容資訊。

建立內容資訊之後，用於 BranchCache 訊息交換而不是實際的資料，以及交換使用支援的通訊協定（HTTP、HTTPS 和 SMB）。

快取的資料會將保持在加密，並無法用無權存取 content 原始來源的存取。  戶端必須驗證，且可以擷取內容中繼資料和必須擁有存取的快取中本機 office 內容中繼資料之前授權原始內容來源。

### <a name="how-branchcache-generates-content-information"></a>如何 BranchCache 產生內容資訊

因為建立多個項目從內容資訊，都唯一內容資訊的值。 這些項目包括：

- （例如網頁或共用的檔案）要衍生 hashes 實際的 content。  

- 設定參數，例如 hashing 演算法並封鎖大小。 若要產生內容資訊，內容伺服器 content 分為區段，然後在區塊細分區段。 BranchCache 使用安全密碼編譯 hashes 以找出並確認每個封鎖區段，支援 SHA256 hash 演算法。

- 伺服器密碼。 必須設定伺服器的密碼，也就是二進位任意長度的值與所有內容伺服器。

> [!NOTE]
> 伺服器秘密使用確保 client 電腦不能本身產生內容資訊。 如此可防止惡意使用者暴力攻擊使用猜測 content 變更次要版本情形，client 存取先前的版本，但不一定目前的版本存取所有 BranchCache 式 client 的電腦。

### <a name="content-information-details"></a>內容資訊的詳細資料

BranchCache 做按鍵以衍生會傳送至授權戶端 content 特定 hash 伺服器密碼。 申請 hashing 演算法組合的伺服器機密資料的 Hash 與產生這個 hash。

這個 hash 稱為區段密碼。 BranchCache 使用區段機密安全通訊。 此外，BranchCache 建立封鎖 Hash 清單中，這是 hashed 的資料封鎖清單，並 Hash 的資料，這由 hashing 封鎖 Hash 清單。

內容資訊包含：

- 封鎖 Hash 清單：

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- Hash 的資料 (HoD):

    `HoD = Hash(BlockHashList)`

- 區段密碼 (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache 會使用等 Content 快取的通訊協定與擷取架構通訊協定來執行所需確保安全的快取，並擷取之間內容快取的資料的處理程序。

此外，BranchCache 控點的內容具有相同程度的處理和傳送本身的實際 content 時，它所使用的安全性資訊。

## <a name="bkmk_flow"></a>內容流程和處理程序

流程內容資訊和實際 content 分為四個階段中：

1.  [BranchCache 處理程序：要求 content](#BKMK_8)

2.  [BranchCache 處理程序：找出 content](#BKMK_9)

3.  [BranchCache 處理程序：擷取 content](#BKMK_10)

4.  [BranchCache 處理程序：content 快取](#BKMK_11)

下列區段描述這些階段。

## <a name="BKMK_8"></a>BranchCache 處理程序：要求 content

第一階段，分公司 client 電腦從遠端的位置，例如主要辦公室的內容伺服器要求 content，例如檔案或網頁。 內容伺服器驗證 client 電腦已授權接收要求的 content。 如果 client 電腦已獲得授權與同時內容伺服器 client BranchCache\ 功能，內容伺服器產生內容資訊。

內容伺服器接著會內容資訊傳送到 client 電腦使用相同的通訊協定時使用的實際 content。 

例如，如果 client 電腦透過 HTTP 要求網頁，內容伺服器內容會將資訊傳送使用 HTTP。 因此，花朵層級的安全性保證 content 的並內容資訊都一樣。

收到內容資訊（Hash 的資料 + 區段密碼）的初始部分之後，client 電腦執行下列動作：

- 使用區段密碼 (Kp) 做為加密金鑰 (Ke)。

- 從 HoD Kp 產生區段 ID (HoHoDk):

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

在這個層級的主要威脅是區段密碼、的風險，但是 BranchCache 加密保護區段機密內容資料區塊。 BranchCache 會利用加密金鑰衍生從區段內容區塊是位於內容區段的密碼。

這種方式可確保您擁有的伺服器密碼的不是實體，就無法探索實際 content 資料區塊中。 區段機密被視為具有相同程度的安全性純文字區段，因為知道特定的區段密碼可從同儕取得該區段並加以解密實體。 不會立即產生任何特定純文字知道伺服器密碼，但可以使用密碼文字，然後到 [可能部分部分已知暴力猜測攻擊資料公開衍生特定類型的資料。 伺服器密碼，因此，則會保留機密。
  
## <a name="BKMK_9"></a>BranchCache 處理程序：找出 content

內容資訊收到 client 電腦之後，client 使用區段 ID 中找不到要求的 content 本機分支 office 快取，不論該快取散發 client 電腦之間，或位於裝載快取的伺服器上。

Client 電腦設定為裝載快取模式時，如果它設定伺服器裝載快取的電腦名稱，並連絡人擷取 content 至該伺服器。

如果 client 電腦設定為 [快取分散式的模式時，不過，可能會儲存 content 跨多個分公司在多部電腦上的快取。 Client 的電腦必須探索 content 所在擷取 content 之前。

當他們的設定分散式快取模式時，client 電腦使用找出 content 為基礎的 Web 服務動態探索（WS 探索）通訊協定探索通訊協定。 戶端傳送 WS 探索發現在網路上的快取的 content 多點的探查訊息。 檢查簡訊包含的區段 ID，讓用來檢查是否要求的 content 是否符合儲存在他們的快取 content 戶端。 用單點比對探查訊息查詢 client 的初始探查訊息回覆收到區段 ID 符合 content 本機快取。  

成功 WS-探索程序的事實執行探索 client 已正確內容資訊，所提供的內容伺服器，它要求 content 而定。

主要要求內容階段資料威脅，所以資訊洩漏、 內容資訊的存取權表示 content 授權的存取。 若要降低此風險，探索程序不會顯示內容的資訊，不顯示任何項目相關包含 content 純文字區段區段 ID 以外。

此外，惡意的使用者來執行相同子網路上的另一部 client 電腦可以查看原始內容來源後續路由器 BranchCache 探索傳輸。

如果分公司中找不到要求的 content，client content 直接從內容伺服器要求透過 WAN 連結。

收到 content 之後，就被新增到本機快取上或裝載快取的伺服器上。 若是如此，內容資訊會防止 client 或裝載快取伺服器本機快取中新增任何不符合 hashes content。 藉由符合 hashes 驗證 content 程序確保只有有效 content 快取中新增了，而且本機快取的完整性受保護。

## <a name="BKMK_10"></a>BranchCache 處理程序： 擷取 content

Client 電腦內容主機會裝載快取伺服器或分散式快取模式 client 的電腦上尋找您想要的 content 之後 client 電腦開始擷取 content 的程序。

第一次 client 電腦會傳送至內容主機，需要先封鎖要求。 要求包含找出您想要的 content 區段 ID 和封鎖範圍。 因為傳回只有一個封鎖，則封鎖範圍包括單一區塊。 （要求的多個區塊目前不支援。）Client 也會儲存在本機未完成要求清單中的要求。  

在 client 從收到有效無法接受要求訊息，內容主機檢查內容主機內容快取中是否有封鎖指定在要求中。

如果握有內容封鎖內容主機，則內容主機傳送包含區段 ID、 封鎖 ID、 加密的資料區塊，以及適用於加密封鎖初始化向量的回應。

如果內容主機不是處於擁有內容封鎖，內容主機傳送空白回應訊息。 這會通知 client 電腦內容主機，不需要要求的封鎖。 空回應訊息包含的區段 ID 和封鎖 ID 要求封鎖，以及零大小資料封鎖。

當 client 電腦收到回應從內容主機時，client 確認訊息對應要求簡訊未完成要求清單中。 （區段 ID 和封鎖索引必須符合未完成的要求。）

如果此驗證程序未成功，且 client 的電腦不是對應要求訊息未完成要求清單中，client 電腦會捨棄訊息。

如果此驗證程序成功，且 client 的電腦未完成要求清單中有對應要求訊息，client 電腦解密封鎖。 Client 然後驗證解密的封鎖針對適當封鎖湊從內容原始內容伺服器的初次取得 client 的資訊。

如果成功驗證，解密的封鎖快取中儲存。

重複此程序，直到 client 具有區塊所需的所有為止。

> [!NOTE]
> 如果上一部電腦不存在完成 content 的區段，擷取通訊協定擷取，並從來源組合組合 content： 一組散發快取模式 client 電腦裝載快取伺服器，並-如果分公司快取不包含完整 content-中主要辦公室的原始內容伺服器。

BranchCache 傳送內容資訊或 content 之前，請先加密資料。 BranchCache 加密回應訊息區塊。 在 Windows 7 BranchCache 使用的預設加密演算法為好一段-128、 加密金鑰是 Ke，而金鑰大小 128 的位元，依照加密演算法。 

BranchCache 產生初始化向量適用於加密演算法，並使用加密金鑰來加密區塊。 BranchCache 然後記錄加密演算法與初始化向量訊息中。 

伺服器戶端從未交換、 分享，或傳送彼此加密金鑰。 Client 從主控來源 content 內容伺服器接收加密金鑰。 然後，它會使用收到來自伺服器的它加密演算法和初始化向量，解密封鎖。 還有其他明確驗證或建置下載通訊協定的授權。

### <a name="security-threats"></a>安全性威脅的攻擊

在這個層級的主要安全性威脅包括：

- 竄改資料：

  *波資料，為要求者 client 竄改資料*。 BranchCache 安全性型號使用 hashes 確認 client 都伺服器變更資料。  

- 資訊洩漏：  

    *BranchCache 傳送加密的 content 至任何 client 指定區段 ID 適當的*。 區段 Id 是公用成員，讓任何 client 可以接收 content 加密。 不過，如果惡意的使用者會取得加密的 content，他們必須知道解密 content 加密金鑰。 上層通訊協定執行驗證，驗證，授權 client 會再提供內容資訊。 內容資訊的安全性與 content 本身提供的安全性，而且 BranchCache 從未公開內容資訊。  

    *攻擊者探查取得 content 網路*。  BranchCache 加密所有傳輸之間戶端使用 AES128 私密金鑰所在 Ke，從網路步阻止資料。  資料本身了，因此不較多或較少保護比如果 BranchCache 必須未使用的所有資訊洩漏內容下載內容伺服器的資訊受完全相同的方式。

-   阻斷服務：

    *Client 已被要求資料的*。 BranchCache 通訊協定整合佇列管理計數器及戶端避免過載計時器。

## <a name="BKMK_11"></a>BranchCache 處理程序： content 快取

在分散式快取模式 client 電腦及位於分公司裝載快取伺服器，內容快取建立隨著時間的 content 擷取透過 WAN 連結。

Client 電腦的設定裝載快取模式時，它們 content 加入自己本機快取，並也提供伺服器裝載快取的資料。 裝載快取通訊協定提供 content 及區段可用性的相關通知的裝載快取伺服器戶端機制。

上傳 content，裝載快取伺服器，client 通知伺服器它有可用的區段。 然後裝載快取伺服器擷取的所有資訊內容相關聯的非區段，並下載的區塊實際需要區段中。 重複此程序，直到 client 有提供裝載快取伺服器不需區段為止。

若要使用裝載快取通訊協定更新裝載快取伺服器，您必須符合下列需求：

- Client 電腦需要有一組區塊裝載快取伺服器提供區段中。 Client 必須提供內容資訊所提供的區段。這被組成區段 ID、 區段 Hash 的資料、 區段機密和所有封鎖 hashes 區段中所包含的清單。

- 裝載快取的伺服器上執行 Windows Server 2008 R2 的裝載快取伺服器的憑證和相關的私密金鑰，然後在分公司 client 電腦必須信任的憑證授權單位發行憑證。 這可讓 client 以及伺服器成功參與 HTTPS 伺服器的驗證。

    > [!IMPORTANT]
    > 裝載快取伺服器的憑證及相關的私密金鑰不需要執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的裝載快取伺服器。  

- Client 的電腦設定裝載快取伺服器及傳輸控制項通訊協定 (TCP) 連接埠號碼裝載快取伺服器聆聽 BranchCache 流量所在的電腦名稱。 此連接埠結合裝載快取伺服器的憑證。 伺服器裝載快取的電腦名稱的完整的網域名稱 (FQDN)，如果可能裝載快取伺服器成員網域的電腦。或如果裝載快取伺服器不網域成員可以是 NetBIOS 電腦的名稱。

- 輸入封鎖要求積極接聽 client 的電腦。 連接埠它用心聆聽傳遞提供訊息的一部分從 client 裝載快取伺服器。 這可讓用於 BranchCache 通訊協定連接至 client 電腦擷取區段中的資料區塊裝載快取伺服器。

- 開始聆聽傳入 HTTP 要求初始化時裝載快取伺服器。

- 如果需要 client 電腦驗證設定裝載快取伺服器，client 和裝載快取伺服器所需的支援 HTTPS 驗證。

### <a name="hosted-cache-mode-cache-population"></a>裝載快取模式快取擴展

將 content 新增至分公司裝載快取伺服器的快取的一開始 client 傳送 INITIAL_OFFER_MESSAGE，包括區段編號。 區段中的 ID INITIAL_OFFER_MESSAGE 要求用於對應區段 Hash 的資料、 封鎖 hashes 的清單及區段機密擷取裝載快取伺服器封鎖快取。 如果裝載快取伺服器已經有特定的區段內容的所有資訊，以 INITIAL_OFFER_MESSAGE 回應將 [確定] 並下載封鎖不要求。

如果裝載快取伺服器未包含所有封鎖 hashes 區段中相關聯的資料提供組塊，感 INITIAL_OFFER_MESSAGE 的回應。 Client 再傳送 SEGMENT_INFO_MESSAGE 描述提供是單一區段。 裝載快取伺服器回應出現 [確定] 訊息，並開始遺失區塊所提供的下載 client 的電腦。

區段 Hash 的資料、 封鎖 hashes 的清單及區段機密用來確保正在下載 content，未經竄改或變更。 然後加入下載的區塊裝載快取伺服器封鎖快取。

## <a name="bkmk_cache"></a>快取安全性  
本章節提供 BranchCache 如何保護快取的資料和裝載快取的伺服器上 client 電腦相關資訊。

### <a name="client-computer-cache-security"></a>Client 電腦快取安全性
最大的威脅資料儲存在 BranchCache 竄改。 如果攻擊可以竄改內容和內容快取中儲存的資訊，然後可能會使用此嘗試及上市的電腦使用 BranchCache 的攻擊。 攻擊者可以起始攻擊插入惡意軟體來其他資料取代。 BranchCache 藉由驗證使用封鎖 hashes 內容資訊中找到所有 content 降低這項重大威脅。 如果攻擊嘗試竄改這項資料，它會捨棄，並會取代有效資料來源。

資料儲存在 BranchCache 次要威脅是洩露。 快取分散式模式，client 快取只 content 它已要求。不過，該資料儲存在清除文字，和可能會有風險。 為了 BranchCache 服務只限制快取的存取，詳列於 ACL 檔案系統權限受本機快取。 

雖然 ACL 是有效的防止未經授權的使用者存取快取，可能是系統管理員權限的使用者存取快取的手動變更 ACL 中所指定的權限。 BranchCache 無法防護惡意管理 account 使用。

未加密內容快取中儲存的資料，因此如果資料流失的問題，您可以使用例如 BitLocker 或加密檔案系統 (EFS) 加密技術。 使用 BranchCache 本機快取不會增加傳播分公司; 在的電腦透過資訊洩漏威脅快取包含複本存放磁碟上的其他地方未加密的檔案。 

加密整個磁碟是重要的環境中的用實體安全性難確保中。 例如，加密整個磁碟協助重要可能會移除分公司環境中的行動裝置版電腦上的資料的安全。

### <a name="hosted-cache-server-cache-security"></a>裝載快取伺服器快取安全性

裝載快取模式，最大的裝載快取伺服器安全性威脅是洩露。 在裝載快取的環境中 BranchCache 行為分散式快取模式時，類似的方式與檔案系統的權限保護快取的資料。 不同的是裝載快取伺服器會儲存所有 content 要求分公司任何 BranchCache 式電腦，而非只單一 client 要求的資料。 未經授權入侵此快取的結果可能更多嚴重，因為資料更多風險。  
  
在裝載快取伺服器執行 Windows Server 2008 R2 裝載快取環境中，使用加密技術，例如 BitLocker 或 EFS 建議如果的分公司用的任何可跨 WAN 連結來存取敏感的資料。 磁碟加密運作只有當電腦已關閉攻擊者取得實體的存取權時，因為還有必要裝載快取，使實體存取。  電腦已或處於睡眠模式時，如果磁碟加密提供一些保護。

> [!NOTE]
> 執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的裝載快取伺服器以便使用其他加密技術不需要根據預設，所有的快取的資料加密。

即使 client 設定裝載快取模式，仍會在本機快取的資料，您可能想要保護除了裝載快取伺服器上的快取本機快取的步驟執行。
