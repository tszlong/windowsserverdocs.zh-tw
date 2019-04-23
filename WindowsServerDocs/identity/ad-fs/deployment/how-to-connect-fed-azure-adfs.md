---
title: 在 Azure 中的 active Directory Federation Services |Microsoft Docs
description: 在這份文件中，您將學習如何在 Azure 中的 AD FS 部署高可用性。
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 588bc3f87c78feccac47d18d31d37be3b1a02d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835099"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>在 Azure 中部署 Active Directory Federation Services
AD FS 提供簡化、 安全的身分識別同盟和網頁單一登入 (SSO) 功能。 同盟與 Azure AD 或 O365 可讓使用者使用內部部署認證進行驗證，並存取雲端中的所有資源。 如此一來，就一定要有高可用性的 AD FS 基礎結構，以確保資源的存取權，同時對內部部署和雲端中。 Azure 中部署 AD FS 有助於達成執行最低限度的工作所需的高可用性。
部署在 Azure 中的 AD FS 的幾項優點，以下列出其中一些：

* **高可用性**-與能力的 Azure 可用性設定組中，您可以確保高可用性的基礎結構。
* **簡單且縮放自如**– 需要更多效能？ 輕鬆地移轉至更強大的機器在 Azure 中的少數點擊
* **跨異地備援**– 使用 Azure 異地備援，您可以確保您的基礎結構在全球各地為高可用性
* **輕鬆地管理**– 使用 Azure 入口網站中的極簡化的管理選項，管理您的基礎結構是非常容易又省事 

## <a name="design-principles"></a>設計原則
![部署設計](./media/how-to-connect-fed-azure-adfs/deployment.png)

上圖顯示建議的基本拓撲，以開始部署您的 AD FS 基礎結構，在 Azure 中。 以下列出拓撲的各個元件背後的原理：

* **DC / ADFS 伺服器**:如果您有少於 1,000 名使用者，您只可以在您的網域控制站上安裝 AD FS 角色。 如果您不想在網域控制站上的任何效能影響，或如果您有超過 1,000 個使用者，然後部署在不同伺服器上的 AD FS。
* **WAP 伺服器**–，讓使用者可以連線到的 AD FS 時不在公司網路也很必要部署 Web 應用程式 Proxy 伺服器。
* **DMZ**:Web 應用程式 Proxy 伺服器將會置於 DMZ，並允許只有 TCP/443 存取周邊網路與內部子網路之間。
* **負載平衡器**:若要確保 AD FS 和 Web 應用程式 Proxy 伺服器的高可用性，建議使用 AD FS 伺服器和 Azure Load Balancer 的內部負載平衡器的 Web 應用程式 Proxy 伺服器。
* **可用性設定組**:若要提供您的 AD FS 部署備援，建議您分組類似的工作負載的可用性設定組中的兩個或多個虛擬機器。 此組態可確保期間在計劃性或非計劃性維護事件發生，至少一部虛擬機器將會使用
* **儲存體帳戶**:建議您有兩個儲存體帳戶。 具有單一儲存體帳戶可能會導致單一失敗點的建立，而且可能會導致無法在不太可能的情況下，儲存體帳戶會中斷部署。 兩個儲存體帳戶有助於讓每條故障路線的一個儲存體帳戶產生關聯。
* **網路隔離**:應該在不同的 DMZ 網路中部署 web 應用程式 Proxy 伺服器。 您可以將一個虛擬網路分割成兩個子網路，然後再部署隔離的子網路中的 Web 應用程式 Proxy 伺服器。 您只可以設定網路安全性群組設定為每個子網路，並允許兩個子網路所需的通訊。 每種部署案例下會提供更多的詳細資料

## <a name="steps-to-deploy-ad-fs-in-azure"></a>若要部署在 Azure 中的 AD FS 的步驟
在本節中所述的步驟說明部署指南如下所示的 AD FS 基礎結構，在 Azure 中。

### <a name="1-deploying-the-network"></a>1.部署網路
如上面所述，您可以在單一虛擬網路中建立兩個子網路或是建立兩個完全不同虛擬網路 (VNet)。 這篇文章將著重於部署單一虛擬網路，並將它分成兩個子網路。 這是目前比較簡單的方式，因為兩個個別的 Vnet 需要 VNet 對 VNet 閘道進行通訊。

**1.1 建立虛擬網路**

![建立虛擬網路](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

在 Azure 入口網站中，選取虛擬網路，而且您可以部署的虛擬網路和一個子網路，立即與只需要按一下。 INT 子網路也會定義，而且可供現在要加入的 Vm。
下一個步驟是新增另一個子網路，也就是 DMZ 子網路。 只要建立 DMZ 子網路

* 選取新建立的網路
* 在屬性中選取 子網路
* 在 子網路面板中按一下 新增 按鈕
* 提供建立子網路的子網路名稱和位址空間資訊

![子網路](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![子網路 DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2.建立網路安全性群組**

網路安全性群組 (NSG) 包含一份存取控制清單 (ACL) 規則來允許或拒絕虛擬網路中 VM 執行個體的網路流量。 Nsg 可與子網路或該子網路內的個別 VM 執行個體相關聯。 將 NSG 與子網路產生關聯時，ACL 規則會套用至該子網路中的所有 VM 執行個體。
為了進行本指南中，我們將建立兩個 Nsg︰ 分別用來在內部網路和 DMZ。 它們將標籤為 NSG_INT 和 NSG_DMZ 分別。

![建立 NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

建立 NSG 之後，將會是 0 的輸入和 0 輸出規則。 一旦安裝在個別的伺服器上角色和功能，然後輸入和輸出規則可根據所需的層級的安全性。

![初始化 NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

建立 Nsg 之後，請建立 NSG_INT 與子網路 INT 和 NSG_DMZ 與子網路 DMZ。 範例螢幕擷取畫面如下所示：

![NSG 設定](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* 按一下 開啟面板，供子網路的子網路
* 選取要與 NSG 關聯的子網路 

完成設定後，子網路的面板看起來應該像下面：

![NSG 之後的子網路](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3.建立連線，才能在內部部署環境**

我們需要在內部部署環境的連線以便部署在 azure 中的網域控制站 (DC)。 Azure 提供您的內部部署基礎結構連接至 Azure 基礎結構的各種連線選項。

* 點對站
* 虛擬網路網站-網站
* ExpressRoute

建議使用 ExpressRoute。 ExpressRoute 可讓您在 Azure 資料中心與您內部部署或共置環境中的基礎結構之間建立私人連線。 ExpressRoute 連線不經過公用網際網路。 它們提供更多的可靠性、 速度、 延遲更低和較高的安全性，比一般連線，透過網際網路。
雖然建議使用 ExpressRoute，您可以選擇最適合貴組織的任何連線方法。 若要深入了解 ExpressRoute 以及使用 ExpressRoute 的各種連線選項，請閱讀[ExpressRoute 技術概觀](https://aka.ms/Azure/ExpressRoute)。

### <a name="2-create-storage-accounts"></a>2.建立儲存體帳戶
為了維持高可用性，並避免依賴單一儲存體帳戶，您可以建立兩個儲存體帳戶。 每個可用性設定組中的機器分成兩個群組，然後指派每個群組個別的儲存體帳戶。

![建立儲存體帳戶](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3.建立可用性設定組
每個角色 （DC/AD FS 和 WAP），建立可用性設定組將包含 2 部機器每個最小值。 這有助於達到更高的可用性，每個角色。 雖然建立可用性設定組，務必要決定下列：

* **容錯網域**:在相同的容錯網域中的虛擬機器共用相同的電源和實體網路交換器。 建議最少 2 個容錯網域。 預設值為 3，您可以將它保持原狀針對此部署目的
* **更新網域**:屬於相同更新網域的機器會在更新期間一起重新啟動。 您想要有 2 個更新網域的最小值。 預設值為 5，而您可以將它保持原狀針對此部署目的

![可用性設定組](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

建立下列可用性設定組

| 可用性設定組 | [角色] | 容錯網域 | 更新網域 |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4.部署虛擬機器
下一個步驟是部署虛擬機器會裝載您的基礎結構中不同角色。 每個可用性設定組中建議使用至少兩部機器。 建立基本部署的四個虛擬機器。

| 電腦 | [角色] | 子網路 | 可用性設定組 | 儲存體帳戶 | IP 位址 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Static |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Static |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Static |

您可能已經注意到，已指定沒有 NSG。 這是因為 azure 可讓您使用 NSG，子網路層級。 然後，您可以使用個別的子網路或是，NIC 物件相關聯的 NSG 來控制機器的網路流量。 閱讀更多資訊[什麼是網路安全性群組 (NSG)](https://aka.ms/Azure/NSG)。
如果您要管理 DNS，建議使用靜態 IP 位址。 您可以使用 Azure DNS，並改為在您的網域的 DNS 記錄，請參閱新的機器，透過其 Azure Fqdn。
虛擬機器的窗格看起來應該像下面完成部署之後：

![部署虛擬機器](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5.設定網域控制站 / AD FS 伺服器
 若要驗證任何連入要求，AD FS 必須連絡網域控制站。 若要儲存從 Azure 進行驗證的內部部署 DC 的昂貴，建議部署在 Azure 中的網域控制站的複本。 為了達到高可用性，建議建立至少 2 個網域控制站的可用性設定組。

| 網域控制站 | [角色] | 儲存體帳戶 |
|:---:|:---:|:---:|
| contosodc1 |Replica |contososac1 |
| contosodc2 |Replica |contososac2 |

* 將兩部伺服器升階為複本網域控制站的 DNS
* 設定 AD FS 伺服器安裝使用伺服器管理員的 AD FS 角色。

### <a name="6-deploying-internal-load-balancer-ilb"></a>6.部署內部負載平衡器 (ILB)
**6.1.建立 ILB**

若要部署 ILB，Azure 入口網站中，按一下 選取的負載平衡器會新增 （+）。

> [!NOTE]
> 如果您看不見**負載平衡器**在您的功能表中，按一下 **瀏覽**入口網站和捲動，直到您看到的左下角**負載平衡器**。  接著，按一下黃色星號即可將它新增至您的功能表。 現在選取新的負載平衡器圖示，以開啟面板，開始設定負載平衡器。
> 
> 

![瀏覽負載平衡器](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **名稱**：負載平衡器為適合的名稱
* **配置**:由於此負載平衡器會位於 AD FS 伺服器，且適用於僅限內部網路連線，請選取 [內部]
* **虛擬網路**:選擇您要在其中部署 AD FS 的虛擬網路
* **子網路**:選擇內部子網路
* **IP 位址指派**:Static

![內部負載平衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

按一下 之後建立和部署 ILB，您應該會看到它在清單中的負載平衡器：

![ILB 之後的負載平衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

下一個步驟是設定後端集區和後端探查。

**6.2.設定 ILB 後端集區**

在 [負載平衡器] 面板中選取新建立的 ILB。 它會開啟 [設定] 面板。 

1. 從 設定 面板中選取 後端集區
2. 在 新增後端集區 面板中，按一下 新增虛擬機器
3. 您會看到一個面板，您可以在其中選擇可用性設定組
4. 選擇 AD FS 可用性設定組

![設定 ILB 後端集區](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3.設定探查**

在 ILB 設定窗格中，選取 健康情況探查。

1. 按一下 新增
2. 提供探查的詳細資料。 **名稱**：探查名稱 b。 **通訊協定**：HTTP c。 **連接埠**：80 (HTTP) d. **路徑**: / adfs/探查 e。 **間隔**:5 （預設值） – 這是在 ILB 探查後端集區 f 中的機器的間隔。 **狀況不良臨界值限制**:2 （預設值） – 這是連續探查失敗之後，ILB 會宣告在後端集區中的機器沒有回應，並停止將流量傳送給它的臨界值。

![設定 ILB 探查](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

我們會使用 /adfs/probe 端點健康情況檢查的明確建立的 AD FS 環境中完整的 HTTPS 路徑檢查不會發生的位置。  這是本質上優於基本的連接埠 443 檢查，不會精確地反映最新的 AD FS 部署的狀態。  詳細資訊，參閱 https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/。

**6.4.建立負載平衡規則**

為了有效平衡流量，您應該為 ILB 設定負載平衡規則。 若要建立負載平衡規則， 

1. 選取負載平衡規則和 ILB 的 [設定] 面板
2. 按一下 [新增負載平衡規則] 面板中
3. 在 [新增負載平衡規則] 面板中。 **名稱**：提供規則 b 的名稱。 **通訊協定**：選取 [TCP] c。 **連接埠**：443 d。 **後端連接埠**:443 e。 **後端集區**:選取您建立的 AD FS 叢集早 f 的集區。 **探查**:選取稍早為 AD FS 伺服器建立的探查

![設定 ILB 平衡規則](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5.更新 ILB 的 DNS**

請移至您的 DNS 伺服器，並為 ILB 建立 CNAME。 CNAME 應適用於同盟服務指向 ILB 的 IP 位址的 IP 位址。 例如，如果 ILB DIP 位址是 10.3.0.8，而安裝的同盟服務是 fs.contoso.com，然後建立指向 10.3.0.8 的 fs.contoso.com 的 CNAME。
這可確保在 ILB fs.contoso.com 結束相關的所有通訊，並會適當地路由傳送。

### <a name="7-configuring-the-web-application-proxy-server"></a>7.設定 Web Application Proxy 伺服器
**7.1.設定 Web 應用程式 Proxy 伺服器以連線到 AD FS 伺服器**

為了確保 Web 應用程式 Proxy 伺服器能夠連線到 ILB 背後的 AD FS 伺服器，ILB %systemroot%\system32\drivers\etc\hosts 中建立的記錄。 請注意，辨別的名稱 (DN) 應該是同盟服務名稱，例如 fs.contoso.com。 而且 IP 項目應該是 ILB 的 IP 位址 (如範例所示的 10.3.0.8)。

**7.2.安裝 Web 應用程式 Proxy 角色**

確定 Web 應用程式 Proxy 伺服器，都能連線到 ILB 背後的 AD FS 伺服器之後，您可以接著安裝 Web Application Proxy 伺服器。 Web 應用程式 Proxy 伺服器不需要加入網域。 安裝兩個 Web 應用程式 Proxy 伺服器上的 Web 應用程式 Proxy 角色，藉由選取 遠端存取角色。 伺服器管理員 會引導您完成 WAP 安裝。
如需有關如何部署 WAP 的詳細資訊，請參閱[安裝和設定 Web Application Proxy 伺服器](https://technet.microsoft.com/library/dn383662.aspx)。

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.部署網際網路對向 （公用） 負載平衡器
**8.1.建立網際網路對向 （公用） 負載平衡器**

在 Azure 入口網站中，選取負載平衡器，然後按一下 新增。 在 [建立負載平衡器] 面板中，輸入下列資訊

1. **名稱**：負載平衡器的名稱
2. **配置**:公用 – 此選項會告知 Azure，此負載平衡器需要公用位址。
3. **IP 位址**:建立新的 IP 位址 （動態）

![網際網路面向的負載平衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

在部署後，負載平衡器會出現在 負載平衡器清單。

![負載平衡器清單](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2.公用 ip 指派 DNS 標籤**

按一下新建立的負載平衡器項目，以顯示組態的面板的 [負載平衡器] 面板中。 請遵循下列步驟來設定公用 ip 的 DNS 標籤：

1. 按一下 公用 IP 位址。 這會開啟 [面板] 中的公用 IP 和它的設定
2. 按一下 組態
3. 提供的 DNS 標籤。 這會成為您可以從任何地方，例如 contosofs.westus.cloudapp.azure.com 存取的公用 DNS 標籤。 您可以新增一個項目中的同盟服務 （例如 fs.contoso.com) 解析為外部負載平衡器 (contosofs.westus.cloudapp.azure.com) 的 DNS 標籤的外部 DNS。

![設定網際網路對向負載平衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![設定網際網路對向負載平衡器 (DNS)](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3.設定網際網路對向 （公用） 負載平衡器後端集區** 

請遵循相同的步驟建立內部負載平衡器，為網際網路對向 （公用） 負載平衡器的後端集區設定的可用性設定組為 WAP 伺服器。 例如，contosowapset。

![設定網際網路對向負載平衡器後端集區](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4.設定探查**

請遵循相同的步驟和設定內部負載平衡器，來設定 WAP 伺服器後端集區的探查。

![設定網際網路對向負載平衡器的探查](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5.建立負載平衡規則**

遵循和 ILB 設定負載平衡規則的 TCP 443 相同的步驟。

![設定網際網路對向負載平衡器的平衡規則](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9.保護網路
**9.1.保護內部子網路**

整體來說，您需要下列規則來有效率地保護內部子網路 （依順序如下所示）

| 規則 | 描述 | 流程 |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |允許來自 DMZ 的 HTTPS 通訊 |輸入 |
| DenyInternetOutbound |無法存取網際網路 |輸出 |

![INT 存取規則 （輸入）](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

<!--
[comment]: <> (![INT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgintinbound.png))
[comment]: <> (![INT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgintoutbound.png))
-->

**9.2.保護 DMZ 子網路**

| 規則 | 描述 | 流程 |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |允許從網際網路到 DMZ 的 HTTPS |輸入 |
| DenyInternetOutbound |封鎖 HTTPS 以外流向網際網路的任何項目 |輸出 |

![EXT 存取規則 （輸入）](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

<!--
[comment]: <> (![EXT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzinbound.png))
[comment]: <> (![EXT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzoutbound.png))
-->

> [!NOTE]
> 如果用戶端使用者憑證驗證 (clientTLS 驗證使用 X509 使用者憑證) 為必要項，則 AD FS 需要 TCP 連接埠 49443 能讓您輸入存取。
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10.測試 AD FS 登入
最簡單的方式，是要測試 AD FS 是使用 IdpInitiatedSignon.aspx 網頁。 若要能夠執行，並需要啟用 AD FS 屬性上的 IdpInitiatedSignOn。 請遵循下列步驟來確認您的 AD FS 設定

1. 執行下列 cmdlet，在 AD FS 伺服器上，使用 PowerShell，將它設定為啟用。
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. 從任何外部的電腦，來存取 https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx。  
3. 您應該會看到 AD FS 頁面如下所示：

![測試登入頁面](./media/how-to-connect-fed-azure-adfs/test1.png)

在成功登入，它會提供您的成功訊息，如下所示：

![測試成功](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>在 Azure 中的 AD FS 的部署範本
此範本會部署 6 部電腦安裝程式，網域控制站，AD FS 和 WAP 各 2。

[在 Azure 部署範本中的 AD FS](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

您可以使用現有的虛擬網路，或在部署此範本時建立新的 VNET。 適用於自訂部署的各種參數如下所示的部署程序中的參數使用方式的描述。 

| 參數 | 描述 |
|:--- |:--- |
| Location |要部署資源，例如東部我們的區域。 |
| StorageAccountType |建立儲存體帳戶類型 |
| VirtualNetworkUsage |表示新的虛擬網路將會建立或使用現有的帳戶 |
| VirtualNetworkName |要建立、 強制在現有或新的虛擬網路使用方式的虛擬網路的名稱 |
| VirtualNetworkResourceGroupName |指定現有的虛擬網路所在的資源群組名稱。 使用現有的虛擬網路時，這會成為必要的參數以便部署找到現有的虛擬網路的識別碼 |
| VirtualNetworkAddressRange |新的 VNET，則為必要建立新的虛擬網路位址範圍 |
| InternalSubnetName |內部子網路名稱，這兩個虛擬網路使用方式選項 （新的或現有的） 時的必要參數 |
| InternalSubnetAddressRange |如果建立新的虛擬網路包含網域控制站和 ADFS 伺服器，必要的內部子網路位址範圍。 |
| DMZSubnetAddressRange |Dmz 子網路，其包含 Windows 應用程式 proxy 伺服器，必要項目，如果建立新的虛擬網路位址範圍。 |
| DMZSubnetName |內部子網路，這兩個虛擬網路使用方式選項 （新的或現有的） 時的必要參數的名稱。 |
| ADDC01NICIPAddress |第一個網域控制站的內部 IP 位址，此 IP 位址會以靜態方式指派給 DC 和必須是內部子網路內的有效 ip 位址 |
| ADDC02NICIPAddress |第二個網域控制站的內部 IP 位址，此 IP 位址會以靜態方式指派給 DC 和必須是內部子網路內的有效 ip 位址 |
| ADFS01NICIPAddress |第一個 ADFS 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 ADFS 伺服器，必須是內部子網路內的有效 ip 位址 |
| ADFS02NICIPAddress |第二個 ADFS 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 ADFS 伺服器，必須是內部子網路內的有效 ip 位址 |
| WAP01NICIPAddress |第一個 WAP 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 WAP 伺服器和必須是 WAP 子網路內的有效 ip 位址 |
| WAP02NICIPAddress |第二個 WAP 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 WAP 伺服器和必須是 WAP 子網路內的有效 ip 位址 |
| ADFSLoadBalancerPrivateIPAddress |ADFS 負載平衡器的內部 IP 位址，此 IP 位址會以靜態方式指派給負載平衡器，必須是內部子網路內的有效 ip 位址 |
| ADDCVMNamePrefix |網域控制站的虛擬機器名稱前置詞 |
| ADFSVMNamePrefix |ADFS 伺服器的虛擬機器名稱前置詞 |
| WAPVMNamePrefix |WAP 伺服器的虛擬機器名稱前置詞 |
| ADDCVMSize |網域控制站的 vm 大小 |
| ADFSVMSize |ADFS 伺服器的 vm 大小 |
| WAPVMSize |WAP 伺服器的 vm 大小 |
| AdminUserName |虛擬機器的本機系統管理員的名稱 |
| AdminPassword |虛擬機器的本機系統管理員帳戶密碼 |

## <a name="additional-resources"></a>其他資源
* [可用性設定組](https://aka.ms/Azure/Availability) 
* [Azure 負載平衡器](https://aka.ms/Azure/ILB)
* [內部負載平衡器](https://aka.ms/Azure/ILB/Internal)
* [網際網路面向負載平衡器](https://aka.ms/Azure/ILB/Internet)
* [儲存體帳戶](https://aka.ms/Azure/Storage)
* [Azure 虛擬網路](https://aka.ms/Azure/VNet)
* [AD FS 和 Web 應用程式 Proxy 連結](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>後續步驟
* [整合內部部署身分識別與 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [設定和管理 AD FS 使用 Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [在 Azure 與 Azure 流量管理員中的高可用性跨地區 AD FS 部署](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
