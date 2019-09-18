---
title: Azure 中的異地備援 RDS 資料中心
description: 了解如何建立 RDS 部署，以使用多個資料中心跨地理位置提供高可用性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 39a0d00e64ecab93fe1826726b14969e5e034738
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871044"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>建立異地備援的多資料中心 RDS 部署以進行災害復原

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以利用 Azure 中的多個資料中心，來啟用遠端桌面服務部署的災害復原功能。 不同於標準高可用性 RDS 部署 (如[遠端桌面服務架構](desktop-hosting-logical-architecture.md)中所述) 使用單一 Azure 區域 (例如西歐) 中的資料中心，多資料中心部署使用多個地理位置的資料中心來提高部署的可用性 - 一個 Azure 資料中心可能無法使用，但不太可能多個區域同時停止運作。 藉由部署異地備援 RDS 架構，您就可以在整個區域發生嚴重失敗的情況下啟用容錯移轉功能。

您可以使用下列指示來有效率的調控 Microsoft Azure 基礎結構服務和 RDS，透過 [Microsoft 服務提供者授權合約 (SPLA) 方案](https://www.microsoft.com/hosting/licensing/splabenefits.aspx)將異地備援桌面主機服務和訂閱者存取使用權 (SAL) 提供給多個租用戶。 您也可以使用[透過軟體保證的 RDS 使用者 CAL 延伸權限](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)，為自己的員工建立異地備援主機服務。

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>高可用性的邏輯架構 - 單一和多個區域
下圖顯示單一 Azure 區域中的高可用性部署架構：

![單一 Azure 區域中的高可用性部署](media/rds-ha-single-region.png)

該部署可分為三層：

- Azure 服務 - Azure 管理介面 (包括 Azure 入口網站和 API)，以及公用網路服務 (例如 DNS 和公用 IP 位址)。
- 桌面主機服務 - 虛擬機器、網路、儲存體、Azure 服務和 Windows Server 角色服務
- Azure 網狀架構 - 執行 Hyper-V 角色的 Windows Server 作業系統，用來將實體伺服器、儲存體單位、網路交換器和路由器虛擬化。 使用 Azure 網狀架構可讓您建立獨立於基礎硬體的 VM、網路、儲存體和應用程式。


相較之下，以下是使用多個 Azure 資料中心的部署架構：

![使用多個 Azure 區域的 RDS 部署](media/rds-ha-multi-region.png)

整個 RDS 部署會複寫到次要 Azure 區域以建立異地備援部署。 此架構使用主動-被動模型，一次只會執行一個 RDS 部署。 VNet 對 VNet 連線可讓兩個環境彼此通訊。 RDS 部署是以單一 Active Directory 樹系/網域為基礎，而 AD 伺服器會跨兩個部署複寫，這表示使用者可以使用相同的認證登入任一部署。 儲存在使用者設定檔磁碟 (UPD) 的使用者設定和資料會儲存在雙節點叢集儲存空間直接存取向外延展檔案伺服器 (SOFS) 上。 第二個相同的儲存空間直接存取叢集會部署在第二個 (被動) 區域中，並使用儲存體複本將使用者設定檔從使用中部署複寫到被動部署。 您可以使用 Azure 流量管理員將終端使用者自動導向至目前使用中的任何部署 - 從終端使用者觀點來看，他們可以使用單一 URL 存取部署而不知道最終會使用哪一個區域。


您「可以」  在每個區域中建立非高可用性 RDS 部署，但即使在一個區域中重新啟動單一 VM，也會發生容錯移轉，因此更有可能出現影響相關效能的容錯移轉。

## <a name="deployment-steps"></a>部署步驟
在 Azure 中建立下列資源，以建立異地備援的多資料中心 RDS 部署：

1. 兩個不同 Azure 區域中的兩個資源群組。 例如，RG A (使用中部署，RG 表示「資源群組」) 和 RG B (被動部署)。
2. RG A 中的高可用性 Active Directory 部署。您可以使用 [New AD Domain with 2 Domain Controllers](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) (具有 2 個網域控制站的新 AD 網域) 範本來建立部署。
3. RG A 中的高可用性 RDS 部署。使用 [RDS farm deployment using existing active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) (使用現有 Active Directory 的 RDS 伺服器陣列部署) 範本來建立基本的 RDS 部署，然後遵循[遠端桌面服務 - 高可用性](rds-plan-high-availability.md)中的資訊設定其他 RDS 元件以取得高可用性。
4. RG B 中的 VNet - 請務必使用未與 RG A 中部署重疊的位址空間。
5. 兩個資源群組之間的 [VNet 對 VNet 連線](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)。
6. RG B 中可用性設定組內的兩部 AD 虛擬機器 - 確定 VM 名稱與 RG A 中的 AD VM 不同。在單一可用性設定組中部署兩部 Windows Server 2016 VM、安裝 Active Directory Domain Services 角色，然後將其升階到您在步驟 1 中所建立網域中的網域控制站。
7. RG B 中的第二個高可用性 RDS 部署。 
   1. 再次使用 [RDS farm deployment using existing active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) (使用現有 Active Directory 的 RDS 伺服器陣列部署) 範本，但這次進行下列變更 (若要自訂範本，請在資源庫中選取它，按一下 [部署至 Azure]  ，然後按一下 [編輯範本]  )。
      1. 調整 DNS 伺服器私人 IP 的位址空間，以對應至 RG B 中的 VNet。 
      
         在變數中搜尋 "dnsServerPrivateIp"。 編輯預設 IP (10.0.0.4)，以對應至您在 RG B 中 VNet 定義的位址空間。
   
      2. 編輯電腦名稱，使它們不會與 RG A 中的部署名稱衝突。
      
         在範本的 [資源]  區段中尋找 VM。 變更 [osProfile]  下的 [computerName]  欄位。 例如，"gateway" 可變成 "gateway **-b**"；"[concat('rdsh-', copyIndex())]" 可變成 "[concat('rdsh-b-', copyIndex())]"；而 “broker” 可變成 “broker **-b**”。
      
         (您也可以在執行範本之後手動變更 VM 的名稱。)
   2. 如上述步驟 3 所述，您可以使用[遠端桌面服務 - 高可用性](rds-plan-high-availability.md)中的資訊設定其他 RDS 元件以取得高可用性。
8. 跨兩個部署具有儲存體複本的儲存空間直接存取向外延展檔案伺服器。 使用 [PowerShell 指令碼](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts)跨資源群組部署[範本](https://github.com/robotechredmond/301-s2d-sr-dr-md)。

   > [!NOTE]
   > 您可以手動佈建儲存體 (而不是使用 PowerShell 指令碼和範本)： 
   >1. 部署 RG A 中的[雙節點儲存空間直接存取 SOFS](rds-storage-spaces-direct-deployment.md)，以儲存您的使用者設定檔磁碟 (UPD)。
   >2. 部署 RG B 中第二個相同的儲存空間直接存取 SOFS - 請務必在每個叢集中使用相同的儲存空間量。
   >3. 在兩者之間設定[使用非同步複寫的儲存體複本](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

### <a name="enable-upds"></a>啟用 UPD
儲存體複本會將資料從來源磁碟區 (與主要/使用中部署相關聯) 複寫到目的地磁碟區 (與次要/被動部署相關聯)。 根據設計，目的地叢集會顯示為 [線上 (無存取權)]  - 儲存體複本會卸載目的地磁碟區及其磁碟機代號或掛接點。 這表示透過提供檔案共用路徑來啟用次要部署的 UDP 會失敗，因為未掛接磁碟區。 

想要深入了解如何管理複寫嗎？ 請參閱[叢集對叢集儲存體複寫](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

若要在兩個部署上啟用 UPD，請執行下列動作：

1. 執行 [Set-RDSessionCollectionConfiguration Cmdlet](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) 為主要 (使用中) 部署啟用使用者設定檔磁碟 - 提供來源磁碟區上的檔案共用路徑 (已在部署步驟的步驟 7 中建立)。
2. 反轉儲存體複本方向，讓目的地磁碟區成為來源磁碟區 (這會掛接磁碟區並使它可供次要部署存取)。 您可以執行 **Set-SRPartnership** Cmdlet 來執行這項操作。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. 在次要 (被動) 部署中啟用使用者設定檔磁碟。 使用您在步驟 1 中針對主要部署所執行的相同步驟。
4. 再次反轉儲存體複本方向，讓原始來源磁碟區再次成為 SR 合作關係中的來源磁碟區，且主要部署可以存取檔案共用。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure 流量管理員 

建立 [Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview)，並確定選取 [優先順序]  路由方法。 將兩個端點設定為每個部署的公用 IP 位址。 在 [設定]  下，將通訊協定變更為 HTTPS (而不是 HTTP)，並將連接埠變更為 443 (而不是 80)。 記下 [DNS 存留時間]  ，並根據您的容錯移轉需求適當地進行設定。 

請注意，流量管理員需要端點在 GET 要求的回應中傳回 200 確定，才能標示為「狀況良好」。 從 RDS 範本建立的 publicIP 物件將會運作，但不會新增路徑增補。 相反地，您可以提供附加 “/RDWeb” 的流量管理員 URL 給終端使用者，例如：```http://deployment.trafficmanager.net/RDWeb```

藉由部署使用 [優先順序] 路由方法的 Azure 流量管理員，您就可以防止終端使用者在使用中部署運作時存取被動部署。 如果終端使用者存取被動部署，而且尚未針對容錯移轉切換儲存體複本方向，使用者登入會停止回應，因為部署會嘗試且無法存取被動儲存空間直接存取叢集上的檔案共用 - 最終部署將會放棄並提供暫存設定檔給使用者。  

### <a name="deallocate-vms-to-save-resources"></a>解除配置 VM 以節省資源 
設定這兩個部署之後，您可以選擇性地關閉並解除配置次要 RDS 基礎結構和 RDSH VM 來節省這些 VM 上的成本。 儲存空間直接存取 SOFS 和 AD 伺服器 VM 必須一律在次要/被動部署中持續執行，以啟用使用者帳戶和設定檔同步處理。  

當容錯移轉發生時，您必須啟動已解除配置的 VM。 此部署設定的優點是成本較低，但代價是容錯移轉時間較長。 如果使用中部署發生嚴重失敗，您必須手動啟動被動部署，否則您將需要自動化指令碼來偵測失敗並自動啟動被動部署。 在任一情況下，可能需要幾分鐘的時間，被動部署才會執行並提供給使用者登入，因而導致服務停機一段時間。 此停機時間取決於啟動 RDS 基礎結構和 RDSH VM 所需的時間 (如果平行而非連續啟動 VM，通常為 2-4 分鐘)，以及讓被動叢集上線的時間 (視叢集大小而定，每個節點 2 個磁碟的雙節點叢集通常為 2-4 分鐘)。 

### <a name="active-directory"></a>Active Directory 
每個部署中的 Active Directory 伺服器都是相同樹系/網域內的複本。 Active Directory 具有內建同步處理通訊協定，以保持四個網域控制站同步。不過，可能會有一些延遲；如果新使用者加入一部 AD 伺服器，可能需要一些時間才能複寫到兩個部署中的所有 AD 伺服器。 因此，請務必警告使用者不要在加入網域之後立即嘗試登入。 

### <a name="rd-license-server"></a>RD 授權伺服器 
為獲授權可存取異地備援部署的每位具名使用者，提供[每個使用者的 RD CAL](rds-client-access-license.md)。 將每個使用者的 CAL 平均散發到使用中部署的兩部 RD 授權伺服器。 然後，將這些 CAL 複製到被動部署的兩部 RD 授權伺服器。 由於 CAL 會在使用中部署與被動部署之間複製，因此在任何指定時間，只能有一個與使用者連線的使用中部署，否則會違反授權合約。  

### <a name="image-management"></a>映像管理 
當您更新 RDSH 映像以來提供軟體更新或新的應用程式時，您需要個別更新每個部署中的 RDSH 集合以確保這兩個部署之間有共通的使用者體驗。 您可以使用 [Update RDSH collection](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/) (更新 RDSH 集合) 範本，但請注意，被動部署的 RDS 基礎結構和 RDSH VM 必須正在執行，才能執行範本。 

## <a name="failover"></a>容錯移轉

在主動-被動部署情況下，容錯移轉需要您啟動次要部署的 VM。 您可以手動或透過自動化指令碼來執行這項操作。 在儲存空間直接存取 SOFS 的重大容錯移轉情況下，變更儲存體複本合作關係方向，讓目的地磁碟區成為來源磁碟區。 例如：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

如需詳細資訊，請參閱[叢集對叢集儲存體複寫](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

Azure 流量管理員會自動辨識主要部署失敗且次要部署狀況良好 (在 RG B 中已啟動 RD 閘道 VM)，並將使用者流量導向至次要部署。 使用者可以使用相同的流量管理員 URL 繼續在其遠端資源上工作，並享有一致的體驗。 請注意，用戶端 DNS 快取不會在 Azure 流量管理員設定中設定的 TTL 期間更新記錄。

### <a name="test-failover"></a>測試容錯移轉
在儲存體複本合作關係中，一次只能有一個磁碟區 (來源) 使用中。 這表示當您切換 SR 合作關係方向時，主要部署 (RG A) 中的磁碟區會成為複寫的目的地而被隱藏。 因此，連線到 RG A 的任何使用者將無法再存取儲存在 RG A 中 SOFS 上的 UPD。 

若要測試容錯移轉，同時允許使用者繼續登入：
1. 啟動 RG B 中的基礎結構 VM 和 RDSH VM。
2. 切換 SR 合作關係方向 (cluster-b-s2d-c 變成來源磁碟區)。
3. 在 Azure 流量管理員設定檔中[停用 RG A 的端點](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint)，以強制 ATM 將流量導向至 RG B。或者，使用 PowerShell 指令碼：

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B 現在是使用中的主要部署。 若要切換回 RG A 作為主要部署：

1. 切換 SR 合作關係方向 (cluster-a-s2d-c 變成來源磁碟區)：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. 在 Azure 流量管理員設定檔中重新啟用 RG A 的端點：

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>內部部署的考量

雖然內部部署無法使用本文所提及的 Azure 快速入門範本，但您可以手動實作所有基礎結構角色。 在成本不受 Azure 使用量影響的內部部署中，請考慮使用主動-主動模型以加速容錯移轉。

您可以對內部部署端點使用 Azure 流量管理員，但需要 Azure 訂用帳戶。 或者，對於提供給終端使用者的 DNS，給與直接將使用者導向至主要部署的 CNAME 記錄。 在容錯移轉情況下，修改 DNS CNAME 記錄以重新導向至次要部署。 如此一來，終端使用者就可以使用單一 URL (就像是使用 Azure 流量管理員)，將使用者導向至適當的部署。 

如果您想要建立內部部署對 Azure 網站模型，請考慮使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)。
