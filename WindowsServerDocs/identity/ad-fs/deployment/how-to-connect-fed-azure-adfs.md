---
title: Azure 中的 Active Directory 同盟服務 |Microsoft Docs
description: 在本檔中，您將瞭解如何在 Azure 中部署 AD FS 以獲得高可用性。
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
ms.openlocfilehash: 15ebf21973f78ad705aca77e178005dfa9529cf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868214"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>在 Azure 中部署 Active Directory 同盟服務
AD FS 提供簡化、安全的身分識別同盟和 Web 單一登入（SSO）功能。 與 Azure AD 或 O365 的同盟可讓使用者使用內部部署認證進行驗證，並存取雲端中的所有資源。 因此，具備高可用性的 AD FS 基礎結構，以確保能夠存取內部部署和雲端中的資源，會變得很重要。 在 Azure 中部署 AD FS 可透過最少的工作，協助達到所需的高可用性。
在 Azure 中部署 AD FS 有幾個優點，以下列出其中幾個：

* **高可用性**-透過 Azure 可用性設定組的強大功能，您可以確保高度可用的基礎結構。
* **容易調整**–需要更多效能嗎？ 只要在 Azure 中按幾下滑鼠，就能輕鬆遷移至更強大的機器
* **跨地理位置冗余**–您可以透過 Azure 異地冗余，確保您的基礎結構在全球各地都有高可用性
* **易於管理**–在 Azure 入口網站中使用高度簡化的管理選項，管理您的基礎結構非常簡單且無需麻煩 

## <a name="design-principles"></a>設計原則
![部署設計](./media/how-to-connect-fed-azure-adfs/deployment.png)

上圖顯示建議的基本拓撲，以開始在 Azure 中部署您的 AD FS 基礎結構。 拓撲的各種元件背後的原則如下所示：

* **DC/ADFS 伺服器**：如果您的使用者少於1000個，您可以直接在網域控制站上安裝 AD FS 角色。 如果您不想對網域控制站執行任何效能影響，或如果您有超過1000個使用者，請在不同的伺服器上部署 AD FS。
* **WAP 伺服器**–必須部署 Web 應用程式 Proxy 伺服器，如此一來，當使用者也不在公司網路上時，就可以連接到 AD FS。
* **DMZ**：Web 應用程式 Proxy 伺服器會放在 DMZ 中，而且 DMZ 與內部子網之間只允許 TCP/443 存取。
* **負載平衡**器：為確保 AD FS 和 Web 應用程式 Proxy 伺服器的高可用性，我們建議針對 AD FS 伺服器使用內部負載平衡器，並針對 Web 應用程式 Proxy 伺服器使用 Azure Load Balancer。
* **可用性設定組**：若要提供 AD FS 部署的冗余，建議您針對類似的工作負載，在可用性設定組中，將兩部以上的虛擬機器組成群組。 此設定可確保在規劃或未規劃的維護事件期間，至少有一部虛擬機器可供使用
* **儲存體帳戶**：建議使用兩個儲存體帳戶。 擁有單一儲存體帳戶可能會導致建立單一失敗點，而且可能會導致部署在儲存體帳戶中斷的情況下變成無法使用。 有兩個儲存體帳戶有助於將每個容錯線路的一個儲存體帳戶建立關聯。
* **網路隔離**：Web 應用程式 Proxy 伺服器應該部署在不同的 DMZ 網路中。 您可以將一個虛擬網路分割成兩個子網，然後在隔離的子網中部署 Web 應用程式 Proxy 伺服器。 您可以只為每個子網設定網路安全性群組設定，並且只允許兩個子網之間的必要通訊。 以下提供每個部署案例的詳細資料

## <a name="steps-to-deploy-ad-fs-in-azure"></a>在 Azure 中部署 AD FS 的步驟
本節中所述的步驟概述部署 Azure 中所描述之 AD FS 基礎結構的指南。

### <a name="1-deploying-the-network"></a>1.部署網路
如上面所述，您可以在單一虛擬網路中建立兩個子網，或建立兩個完全不同的虛擬網路（VNet）。 本文將著重于部署單一虛擬網路，並將它分成兩個子網。 這是目前較簡單的方法，因為兩個不同的 Vnet 會要求 vnet 對 VNet 閘道進行通訊。

**1.1 建立虛擬網路**

![建立虛擬網路](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

在 Azure 入口網站中，選取 虛擬網路，您只要按一下就可以立即部署虛擬網路和一個子網。 INT 子網也已定義，現在已可供新增 Vm。
下一個步驟是將另一個子網新增至網路，亦即 DMZ 子網。 若要建立 DMZ 子網，只需要

* 選取新建立的網路
* 在屬性中選取子網
* 在 [子網] 面板中，按一下 [新增] 按鈕
* 提供子網名稱和位址空間資訊，以建立子網

![Subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![子網 DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2。建立網路安全性群組**

網路安全性群組（NSG）包含存取控制清單（ACL）規則的清單，可允許或拒絕虛擬網路中 VM 實例的網路流量。 Nsg 可以與子網或該子網內的個別 VM 實例相關聯。 當 NSG 與子網相關聯時，ACL 規則會套用至該子網中的所有 VM 實例。
基於本指南的目的，我們將建立兩個 Nsg：一個用於內部網路和一個 DMZ。 它們會分別標示為 NSG_INT 和 NSG_DMZ。

![建立 NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

建立 NSG 之後，將會有0個輸入和0個輸出規則。 一旦個別伺服器上的角色已安裝且正常運作，就可以根據所需的安全性層級來建立輸入和輸出規則。

![初始化 NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

建立 Nsg 之後，請將 NSG_INT 與子網 INT 和 NSG_DMZ 關聯至子網 DMZ。 以下提供範例螢幕擷取畫面：

![NSG 設定](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* 按一下 [子網] 以開啟子網的面板
* 選取要與 NSG 建立關聯的子網 

設定之後，子網的面板看起來應該如下所示：

![NSG 後的子網](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3。建立與內部部署的連線**

我們需要連接到內部部署，才能在 azure 中部署網域控制站（DC）。 Azure 提供各種連線選項，可將您的內部部署基礎結構連接到您的 Azure 基礎結構。

* 點對站
* 虛擬網路站對站
* ExpressRoute

建議使用 ExpressRoute。 ExpressRoute 可讓您在 Azure 資料中心與內部部署或共置環境中的基礎結構之間建立私人連線。 ExpressRoute 連線不會經過公用網際網路。 相較于透過網際網路的一般連線，它們提供更可靠、速度更快、延遲更低和更高的安全性。
雖然建議使用 ExpressRoute，但您可以選擇最適合您組織的任何連線方法。 若要深入瞭解 ExpressRoute 以及使用 ExpressRoute 的各種連線選項，請閱讀[ExpressRoute 技術總覽](https://aka.ms/Azure/ExpressRoute)。

### <a name="2-create-storage-accounts"></a>2.建立儲存體帳戶
為了維持高可用性並避免相依于單一儲存體帳戶，您可以建立兩個儲存體帳戶。 將每個可用性設定組中的機器分成兩個群組，然後為每個群組指派個別的儲存體帳戶。

![建立儲存體帳戶](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3.建立可用性設定組
針對每個角色（DC/AD FS 和 WAP），建立每個可用性設定組，其最少會包含2部機器。 這將有助於達到每個角色的更高可用性。 建立可用性設定組時，必須決定下列各項：

* **容錯網域**：相同容錯網域中的虛擬機器會共用相同的電源和實體網路交換器。 建議至少2個容錯網域。 預設值為3，您可以將其保留為用於此部署的目的
* **更新網域**：屬於相同更新網域的機器會在更新期間一起重新開機。 您想要有至少2個更新網域。 預設值為5，而您可以將其保留為用於此部署的目的

![可用性設定組](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

建立下列可用性設定組

| 可用性設定組 | Role | 容錯網域 | 更新網域 |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WPA |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4.部署虛擬機器
下一個步驟是部署虛擬機器，以在您的基礎結構中裝載不同的角色。 在每個可用性設定組中，建議至少有兩部機器。 為基本部署建立四部虛擬機器。

| Machine | Role | Subnet | 可用性設定組 | 儲存體帳戶 | IP 位址 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Static |
| contosowap1 |WPA |是非 |contosowapset |contososac1 |Static |
| contosowap2 |WPA |是非 |contosowapset |contososac2 |Static |

您可能已經注意到，尚未指定任何 NSG。 這是因為 azure 可讓您在子網層級使用 NSG。 然後，您可以使用與子網或 NIC 物件相關聯的個別 NSG 來控制機器網路流量。 如需詳細資訊，請參閱[什麼是網路安全性群組（NSG）](https://aka.ms/Azure/NSG)。
如果您要管理 DNS，建議使用靜態 IP 位址。 您可以使用 Azure DNS，而不是您網域的 DNS 記錄，請依其 Azure Fqdn 參考新機器。
部署完成之後，您的虛擬機器窗格看起來應該如下所示：

![虛擬機器部署](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5.設定網域控制站/AD FS 伺服器
 若要驗證任何連入要求，AD FS 必須聯繫網域控制站。 若要將從 Azure 到內部部署 DC 的昂貴行程儲存在驗證中，建議您在 Azure 中部署網域控制站的複本。 為了達到高可用性，建議您建立至少2個網域控制站的可用性設定組。

| 網域控制站 | Role | 儲存體帳戶 |
|:---:|:---:|:---:|
| contosodc1 |Replica |contososac1 |
| contosodc2 |Replica |contososac2 |

* 使用 DNS 將兩部伺服器升階為複本網域控制站
* 使用伺服器管理員安裝 AD FS 角色，以設定 AD FS 伺服器。

### <a name="6-deploying-internal-load-balancer-ilb"></a>6.部署內部 Load Balancer （ILB）
**6.1。建立 ILB**

若要部署 ILB，請在 Azure 入口網站中選取 [負載平衡器]，然後按一下 [新增] （+）。

> [!NOTE]
> 如果您在功能表中看不到 [**負載平衡**器]，請按一下入口網站左下方的 **[流覽]** 並進行滾動，直到您看到 [**負載平衡**器] 為止。  然後按一下黃色星號，將其新增至您的功能表。 現在，選取 [新增負載平衡器] 圖示以開啟面板，以開始設定負載平衡器。
> 
> 

![流覽負載平衡器](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **名稱**：為負載平衡器提供任何適當的名稱
* **配置**：由於此負載平衡器會放在 AD FS 伺服器前面，而且僅適用于內部網路連線，因此請選取 [內部]
* **虛擬網路**：選擇您要在其中部署 AD FS 的虛擬網路
* **子網**:在此選擇內部子網
* **IP 位址指派**：Static

![內部負載平衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

在您按一下 [建立] 並部署 ILB 之後，您應該會在負載平衡器清單中看到它：

![ILB 後的負載平衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

下一步是設定後端集區和後端探查。

**6.2。設定 ILB 後端集區**

在 [負載平衡器] 面板中選取新建立的 ILB。 它會開啟 [設定] 面板。 

1. 從 [設定] 面板中選取 [後端集區]
2. 在 [新增後端集區] 面板中，按一下 [新增虛擬機器]
3. 您會看到一個可供您選擇可用性設定組的面板
4. 選擇 AD FS 可用性設定組

![設定 ILB 後端集區](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3。正在設定探查**

在 [ILB 設定] 面板中，選取 [健康情況探查]。

1. 按一下 [新增]
2. 提供探查 a 的詳細資料。 **名稱**：探查名稱 b。 **通訊協定**：HTTP c。 **連接埠**：80（HTTP） d。 **路徑**：/adfs/probe e。 **間隔**:5（預設值）–這是 ILB 將在後端集區中探查機器的間隔 f。 **狀況不良閾值限制**：2（預設值）–這是連續探查失敗的臨界值，之後 ILB 會將後端集區中的機器宣告為沒有回應，並停止將流量傳送給它。

![設定 ILB 探查](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

我們會在無法進行完整 HTTPS 路徑檢查的 AD FS 環境中，使用明確所建立的/adfs/probe 端點進行健康情況檢查。  這比基本埠443檢查明顯好，這不會正確反映現代化 AD FS 部署的狀態。  如需詳細資訊， https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/ 請參閱。

**6.4。建立負載平衡規則**

為了有效平衡流量，應使用負載平衡規則來設定 ILB。 若要建立負載平衡規則， 

1. 從 ILB 的 [設定] 面板中選取 [負載平衡規則]
2. 按一下 [負載平衡規則] 面板中的 [新增]
3. 在 [新增負載平衡規則] 面板中。 **名稱**：提供規則 b 的 [名稱]。 **通訊協定**：選取 [TCP c]。 **連接埠**：443 d。 **後端埠**:443 e。 **後端集**區:選取您稍早為 AD FS 叢集建立的集區 f。 **探查**：選取先前為 AD FS 伺服器建立的探查

![設定 ILB 平衡規則](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5。使用 ILB 更新 DNS**

移至您的 DNS 伺服器，並建立 ILB 的 CNAME。 CNAME 應該是同盟服務的 IP 位址指向 ILB 的 IP 位址。 例如，如果 ILB DIP 位址是10.3.0.8，且安裝的 federation service 是 fs.contoso.com，則請為指向10.3.0.8 的 fs.contoso.com 建立 CNAME。
這可確保與 fs.contoso.com 相關的所有通訊都在 ILB 上，並且會適當地路由傳送。

### <a name="7-configuring-the-web-application-proxy-server"></a>7.設定 Web 應用程式 Proxy 伺服器
**7.1。設定 Web 應用程式 Proxy 伺服器以觸及 AD FS 伺服器**

為了確保 Web 應用程式 Proxy 伺服器能夠連線到 ILB 後方的 AD FS 伺服器，請在 ILB 的%systemroot%\system32\drivers\etc\hosts 中建立記錄。 請注意，辨別名稱（DN）應為同盟服務名稱，例如 fs.contoso.com。 而 IP 專案應該是 ILB 的 IP 位址（如範例中的10.3.0.8）。

**7.2。安裝 Web 應用程式 Proxy 角色**

確定 Web 應用程式 Proxy 伺服器能夠連線到 ILB 後方的 AD FS 伺服器之後，您可以接著安裝 Web 應用程式 Proxy 伺服器。 Web 應用程式 Proxy 伺服器不需要加入網域。 藉由選取 [遠端存取] 角色，在兩個 Web 應用程式 Proxy 伺服器上安裝 Web 應用程式 Proxy 角色。 伺服器管理員將會引導您完成 WAP 安裝。
如需如何部署 WAP 的詳細資訊，請參閱[安裝和設定 Web 應用程式 Proxy 伺服器](https://technet.microsoft.com/library/dn383662.aspx)。

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.部署網際網路對向（公用） Load Balancer
**8.1。建立網際網路對向（公用） Load Balancer**

在 Azure 入口網站中，選取 負載平衡器，然後按一下 新增。 在 [建立負載平衡器] 面板中，輸入下列資訊

1. **名稱**：負載平衡器的名稱
2. **配置**：公用–此選項會告知 Azure，此負載平衡器需要公用位址。
3. **IP 位址**：建立新的 IP 位址（動態）

![網際網路面向的負載平衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

部署之後，負載平衡器將會出現在 [負載平衡器] 清單中。

![負載平衡器清單](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2。將 DNS 標籤指派給公用 IP**

在 [負載平衡器] 面板中，按一下新建立的負載平衡器專案，以顯示設定的面板。 請遵循下列步驟來設定公用 IP 的 DNS 標籤：

1. 按一下 [公用 IP 位址]。 這會開啟公用 IP 及其設定的面板
2. 按一下 [設定]
3. 提供 DNS 標籤。 這會變成您可以從任何地方存取的公用 DNS 標籤，例如 contosofs.westus.cloudapp.azure.com。 您可以在同盟服務的外部 DNS （例如 fs.contoso.com）中新增專案，以解析為外部負載平衡器（contosofs.westus.cloudapp.azure.com）的 DNS 標籤。

![設定網際網路面向的負載平衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![設定網際網路對向負載平衡器（DNS）](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3。設定網際網路對向（公用） Load Balancer 的後端集區** 

遵循與建立內部負載平衡器相同的步驟來設定網際網路對向（公用） Load Balancer 的後端集區，做為 WAP 伺服器的可用性設定組。 例如，contosowapset。

![設定網際網路面向 Load Balancer 的後端集區](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4。設定探查**

遵循設定內部負載平衡器中的相同步驟，為 WAP 伺服器的後端集區設定探查。

![設定對網際網路面向 Load Balancer 的探查](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5。建立負載平衡規則**

遵循與 ILB 中相同的步驟來設定 TCP 443 的負載平衡規則。

![設定網際網路面向 Load Balancer 的平衡規則](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9.保護網路安全
**9.1。保護內部子網**

整體來說，您需要下列規則來有效率地保護您的內部子網（依照下列順序）

| 規則 | 描述 | 流程 |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |允許來自 DMZ 的 HTTPS 通訊 |輸入 |
| DenyInternetOutbound |無法存取網際網路 |輸出 |

![INT 存取規則（輸入）](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9.2。保護 DMZ 子網**

| 規則 | 描述 | 流程 |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |允許從網際網路到 DMZ 的 HTTPS |輸入 |
| DenyInternetOutbound |除了 HTTPS 到網際網路以外的任何專案都會遭到封鎖 |輸出 |

![EXT 存取規則（輸入）](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> 如果需要用戶端使用者憑證驗證（使用 X509 使用者憑證的 Clienttls 是 authentication），則 AD FS 需要啟用 TCP 埠49443以進行輸入存取。
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10.測試 AD FS 登入
最簡單的方法是使用 IDPInitiatedsignon.aspx) 來測試 AD FS。 為了能夠這樣做，必須在 AD FS 屬性上啟用 IDPInitiatedsignon.aspx)。 請遵循下列步驟來確認您的 AD FS 設定

1. 使用 PowerShell 在 AD FS 伺服器上執行下列 Cmdlet，將其設定為 [已啟用]。
   Set-adfsproperties-EnableIdPInitiatedSignonPage $true 
2. 從任何外部電腦存取 HTTPs：\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx。  
3. 您應該會看到如下所示的 [AD FS] 頁面：

![測試登入頁面](./media/how-to-connect-fed-azure-adfs/test1.png)

成功登入時，它會提供您成功的訊息，如下所示：

![測試成功](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>在 Azure 中部署 AD FS 的範本
此範本會部署6部電腦安裝程式，其中2個用於網域控制站，AD FS 和 WAP。

[Azure 部署範本中的 AD FS](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

部署此範本時，您可以使用現有的虛擬網路或建立新的 VNET。 以下列出可供自訂部署的各種參數，以及在部署程式中使用參數的描述。 

| 參數 | 描述 |
|:--- |:--- |
| Location |要將資源部署到其中的區域，例如「美國東部」。 |
| StorageAccountType |建立的儲存體帳戶類型 |
| VirtualNetworkUsage |指出將建立新的虛擬網路，或使用現有的 |
| VirtualNetworkName |要建立之虛擬網路的名稱，在現有或新的虛擬網路使用量上都是必要的 |
| VirtualNetworkResourceGroupName |指定現有虛擬網路所在的資源組名。 使用現有的虛擬網路時，這會變成強制參數，因此部署可以尋找現有虛擬網路的識別碼。 |
| VirtualNetworkAddressRange |新 VNET 的位址範圍，如果建立新的虛擬網路，則為必要項 |
| InternalSubnetName |內部子網的名稱，這兩個虛擬網路使用方式選項（新的或現有的）都是必要的 |
| InternalSubnetAddressRange |內部子網的位址範圍，其中包含網域控制站和 ADFS 伺服器，如果要建立新的虛擬網路，則為必要項。 |
| DMZSubnetAddressRange |Dmz 子網的位址範圍，其中包含 Windows 應用程式 proxy 伺服器，如果要建立新的虛擬網路，則為必要項。 |
| DMZSubnetName |內部子網的名稱，這兩個虛擬網路使用方式選項（新的或現有的）都是必要的。 |
| ADDC01NICIPAddress |第一個網域控制站的內部 IP 位址，此 IP 位址會以靜態方式指派給 DC，而且必須是內部子網內的有效 ip 位址 |
| ADDC02NICIPAddress |第二個網域控制站的內部 IP 位址，此 IP 位址會以靜態方式指派給 DC，而且必須是內部子網內的有效 ip 位址 |
| ADFS01NICIPAddress |第一部 ADFS 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 ADFS 伺服器，而且必須是內部子網內的有效 ip 位址 |
| ADFS02NICIPAddress |第二部 ADFS 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 ADFS 伺服器，而且必須是內部子網內的有效 ip 位址 |
| WAP01NICIPAddress |第一部 WAP 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 WAP 伺服器，而且必須是 DMZ 子網內的有效 ip 位址 |
| WAP02NICIPAddress |第二部 WAP 伺服器的內部 IP 位址，此 IP 位址會以靜態方式指派給 WAP 伺服器，而且必須是 DMZ 子網內的有效 ip 位址 |
| ADFSLoadBalancerPrivateIPAddress |ADFS 負載平衡器的內部 IP 位址，此 IP 位址會以靜態方式指派給負載平衡器，而且必須是內部子網內的有效 ip 位址 |
| ADDCVMNamePrefix |網域控制站的虛擬機器名稱前置詞 |
| ADFSVMNamePrefix |ADFS 伺服器的虛擬機器名稱前置詞 |
| WAPVMNamePrefix |WAP 伺服器的虛擬機器名稱前置詞 |
| ADDCVMSize |網域控制站的 vm 大小 |
| ADFSVMSize |ADFS 伺服器的 vm 大小 |
| WAPVMSize |WAP 伺服器的 vm 大小 |
| AdminUserName |虛擬機器的本機系統管理員名稱 |
| adminPassword |虛擬機器的本機系統管理員帳戶密碼 |

## <a name="additional-resources"></a>其他資源
* [可用性設定組](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [內部 Load Balancer](https://aka.ms/Azure/ILB/Internal)
* [網際網路面向的 Load Balancer](https://aka.ms/Azure/ILB/Internet)
* [儲存體帳戶](https://aka.ms/Azure/Storage)
* [Azure 虛擬網路](https://aka.ms/Azure/VNet)
* [AD FS 和 Web 應用程式 Proxy 連結](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>後續步驟
* [整合您的內部部署身分識別與 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [使用 Azure AD Connect 設定和管理您的 AD FS](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [使用 Azure 流量管理員在 Azure 中部署高可用性跨地理位置 AD FS](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
