---
title: 在 Azure 中的異地備援 RDS 資料中心
description: 了解如何建立使用多個資料中心，以提供高可用性跨地理位置的 RDS 部署。
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
ms.openlocfilehash: 7d895b1098c4d8cdf162c77f35209b7308872d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849959"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>建立異地備援、 多重資料中心針對災害復原的 RDS 部署

>適用於：Windows Server （半年通道），Windows Server 2016

您可以運用在 Azure 中的多個資料中心，來啟用遠端桌面服務部署災害復原。 不同於標準的高可用性的 RDS 部署 (如所述[遠端桌面服務架構](desktop-hosting-logical-architecture.md))、 單一 Azure 區域 （例如西歐） 中使用的資料中心、 多重資料中心部署，使用資料中心在多個地理位置，增加您的部署-一個 Azure 資料中心的可用性可能會無法使用，但是太多個區域會當機，在相同的時間。 藉由部署異地備援的 RDS 架構，您可以啟用發生嚴重失敗的整個區域的容錯移轉。

您可以使用下列指示以透過多個租用戶中利用 Microsoft Azure 基礎結構服務和提供異地備援的桌面主機服務和訂戶存取授權 (Sal) 的 RDS [Microsoft 服務提供者授權合約 (SPLA) 方案](https://www.microsoft.com/hosting/licensing/splabenefits.aspx)。 您也可以使用下列步驟來建立您自己的員工使用異地備援的主機服務[延伸權限，透過軟體保證的 RDS 使用者 Cal](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)。

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>邏輯架構的高可用性-單一和多個區域
下圖顯示在單一 Azure 區域中的高可用性部署的架構：

![在單一 Azure 區域中的高可用性部署](media/rds-ha-single-region.png)

部署是由三個層級所組成：

- Azure 服務-Azure 管理介面，包括 Azure 入口網站和 Api，以及公用網路服務的詳細資訊，例如 DNS 和公用 IP 位址。
- 桌面主機服務-虛擬機器、 網路、 儲存體、 Azure 服務和 Windows Server 角色服務
- Azure 網狀架構-Windows Server 作業系統上執行 HYPER-V 角色，用來虛擬化實體伺服器、 儲存體單位、 網路交換器和路由器。 使用 Azure 網狀架構，可讓您建立 Vm、 網路、 儲存和應用程式獨立從基礎硬體。


相較之下，以下是使用多個 Azure 資料中心部署的架構：

![使用多個 Azure 區域的 RDS 部署](media/rds-ha-multi-region.png)

建立異地備援部署的第二個 Azure 區域中複寫整個 RDS 部署。 此架構會使用主動-被動模型中，一次只能有一個 RDS 部署正在執行。 VNet 對 VNet 連線可讓兩個環境之間的通訊方式。 RDS 部署會根據單一 Active Directory 樹系/網域，並跨兩個部署的 AD 伺服器複寫，表示使用者可以登入使用相同的認證可能是部署。 雙節點叢集儲存空間直接存取 (S2D) 向外延展檔案伺服器 (SOFS) 上儲存使用者設定和資料儲存在使用者設定檔磁碟 (UPD)。 第二個相同 S2D 叢集已部署在第二個 （被動） 區域中，並使用儲存體複本複寫到被動的部署使用者設定檔使用中的。 Azure 流量管理員用來自動導向至任何部署的使用者是目前作用中-從使用者觀點來看，他們可存取部署使用單一的 URL 並不知道它們最終會使用的哪一個區域。


您*無法*建立非高可用的 RDS 部署中每個區域，但如果即使單一 VM 重新啟動在一個區域，會發生容錯移轉，因而增加的容錯移轉期間發生的可能性相關聯的效能影響。

## <a name="deployment-steps"></a>部署步驟
在 Azure 中建立的異地備援的多重資料中心的 RDS 部署中建立下列資源：

1. 兩個資源群組中兩個不同 Azure 區域。 例如 RG A （代表 「 資源群組 」 的現用部署，也就是 RG） 和 RG B （被動部署）。
2. RG a 中的高可用性的 Active Directory 部署您可以使用[新的 AD 網域，使用 2 個網域控制站範本](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/)建立部署。
3. 在 RG A.使用高度可用的 RDS 部署[RDS 伺服器陣列部署使用現有的 active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)範本來建立基本的 RDS 部署中，，然後依照中的資訊[遠端桌面服務-高可用性](rds-plan-high-availability.md)設定高可用性的其他 RDS 元件。
4. 請務必使用位址空間不會重疊在 RG A 中的部署的 RG B-中的 VNet
5. A [VNet 對 VNet 連線](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)之間的兩個資源群組。
6. 兩個 AD 虛擬機器的可用性設定組中 RG B-請確定 VM 名稱不同於在 RG A.部署兩個 Windows Server 2016 Vm，在單一可用性設定、 安裝 Active Directory 網域服務角色，以及再將其提升至網域內容中的 AD Vm您在步驟 1 中建立的網域中的滾輪。
7. 第二個高度可用的 RDS 部署中 RG b。 
   1. 使用[RDS 伺服器陣列部署使用現有的 active directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)範本一次，但這次會進行下列變更。 (若要自訂範本，在資源庫中選取它，請按一下**部署至 Azure** ，然後**編輯範本**。)
      1. 調整 DNS 伺服器私人 IP，以對應至 RG B 中的 VNet 位址的空間 
      
         搜尋 「 dnsServerPrivateIp"變數中。 編輯對應至您在 RG B 中的 VNet 中定義的位址空間的預設 IP (10.0.0.4)
   
      2. 編輯電腦名稱，使它們不會衝突與 RG A 中部署
      
         找出中的 Vm**資源**範本區段。 變更**computerName**欄位下**osProfile**。 比方說，「 閘道 」 可能會變得 「 閘道 **-b**";"[concat ('rdsh-'，copyIndex())] 」 可能會變得"[concat （'rdsh-b-'、 copyIndex())]"，和 「 代理者 」 可能會變得 「 訊息代理程式 **-b**"。
      
         （您也可以變更 Vm 的名稱以手動方式之後執行範本。）
   2. 如同在上述步驟 3，使用中的資訊[遠端桌面服務-高可用性](rds-plan-high-availability.md)設定高可用性的其他 RDS 元件。
8. 儲存空間直接存取向外延展檔案伺服器儲存體複本的跨兩個部署。 使用[PowerShell 指令碼](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts)部署[範本](https://github.com/robotechredmond/301-s2d-sr-dr-md)跨資源群組。

   > [!NOTE]
   > 您可以佈建儲存體，以手動方式 （而不是使用 PowerShell 指令碼和範本）： 
   >1. 部署[2 個節點的 S2D SOFS](rds-storage-spaces-direct-deployment.md) RG A 來儲存您的使用者設定檔磁碟 (Upd) 中。
   >2. 部署第二個、 相同的 S2D SOFS，RG B 中-請務必在每個叢集中使用相同的儲存體數量。
   >3. 設定好[非同步複寫的儲存體複本](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)兩者之間。

### <a name="enable-upds"></a>啟用 Upd
儲存體複本會將資料從來源磁碟區 （與主要/主動部署相關聯） 複寫到目的地磁碟區 （和次要/被動部署相關聯）。 根據設計，目的地叢集看似**線上 （沒有存取權）** -儲存體複本 」 會卸載目的地磁碟區和其磁碟機代號或掛接點。 這表示，啟用的次要部署的 Upd，藉由提供檔案共用路徑將會失敗，因為它沒有裝載磁碟區。 

想要深入了解管理複寫嗎？ 請參閱[叢集對叢集儲存體複寫](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

若要啟用這兩個部署的 Upd，請執行下列作業：

1. 執行[組 RDSessionCollectionConfiguration cmdlet](https://technet.microsoft.com/itpro/powershell/windows/remote-desktop/set-rdsessioncollectionconfiguration)啟用主要 （使用中） 部署-使用者設定檔磁碟的來源磁碟區 （您在步驟 7 中建立的部署步驟中） 提供檔案共用的路徑。
2. 使目的地磁碟區變成來源磁碟區 （這會掛接磁碟區並使其可存取次要部署），請反轉儲存體複本方向。 您可以執行**Set-srpartnership** cmdlet 來執行這項操作。 例如: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. 可讓使用者設定檔磁碟，在次要 （被動） 的部署。 您對主要的部署，在步驟 1 中，請使用相同的步驟。
4. 反轉一次儲存體複本方向，因此原始的來源磁碟區是一次 SR 合作關係中的來源磁碟區，主要的部署可以存取檔案共用。 例如: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure 流量管理員 

建立[Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview)程式碼剖析，並請務必選取**優先順序**路由方法。 將兩個端點設定為 每個部署的公用 IP 位址。 底下**組態**，將通訊協定變更為 HTTPS （而非 HTTP) 和連接埠為 443 （而不是 80)。 請記下的**DNS 存留時間**，並適當地設定它，以符合您的容錯移轉需求。 

請注意，流量管理員會需要端點，以傳回 200 確定以回應 GET 要求，以標示為 「 良好 」。 RDS 的範本建立的 publicIP 物件會有作用，但不是新增路徑增補。 相反地，您可以讓使用者使用流量管理員 URL 「 / RDWeb"附加，例如： ```http://deployment.trafficmanager.net/RDWeb```

藉由部署 Azure 流量管理員與優先順序路由方法，您可以防止終端使用者存取被動部署，而作用中的部署正常運作。 如果終端使用者存取被動部署和儲存體複本方向尚未已切換為容錯移轉，請在使用者登入時會停止回應，部署嘗試，並無法存取檔案共用，在被動的 S2D 叢集-最終部署將會放棄，並提供使用者的暫存設定檔中。  

### <a name="deallocate-vms-to-save-resources"></a>若要儲存資源的虛擬機器解除配置 
設定這兩個部署之後，您就可以選擇性地關閉，並解除配置的次要 RDS 基礎結構和 RDSH Vm 來節省成本，在這些 Vm 上。 S2D SOFS 和 AD 伺服器 Vm 必須永遠維持啟用使用者帳戶和設定檔同步處理的次要/被動部署中執行。  

在容錯移轉時，您必須啟動已解除配置的 Vm。 此部署組態的優點就是較低的成本，但代價是容錯移轉時間。 如果在使用中的部署中發生嚴重失敗，您必須以手動方式啟動被動部署，或您將需要自動化指令碼來偵測失敗並會自動啟動被動部署。 在任一情況下，可能需要幾分鐘的時間取得被動部署，執行且可供使用者登入，因而導致服務停機一段時間。 這種停機情形而定的時間量它會啟動 RDS 基礎結構和 RDSH Vm （通常是 2-4 分鐘，如果 Vm 已啟動，以平行方式而不是以序列方式），以及時間讓被動叢集上線 （它相依於叢集的大小通常是 2 的磁碟，每個節點的 2 個節點叢集的 2 到 4 分鐘)。 

### <a name="active-directory"></a>Active Directory 
在每個部署的 Active Directory 伺服器是在相同的樹系/網域內的複本。 Active Directory 有內建的同步處理通訊協定，將四個網域控制站同步。不過，可能有一些延遲，所以如果新的使用者新增至一部 AD 伺服器，可能需要一些時間才能跨兩個部署中的所有 AD 伺服器複寫。 因此，請務必嘗試加入網域之後，立即登入的使用者，即發出警告。 

### <a name="rd-license-server"></a>RD 授權伺服器 
提供[每個使用者 RD CAL](rds-client-access-license.md)為每個具名的使用者獲授權可存取異地備援的部署。 將每個使用者 Cal，平均分散給使用中的部署中的兩個 RD 授權伺服器。 然後，重複這些 Cal 給被動的部署中的兩個 RD 授權伺服器。 Cal 會主動和被動部署之間複製，因為在任何給定時間只能有一個部署可處於作用中與使用者連接;否則，您會違反授權合約。  

### <a name="image-management"></a>映像管理 
當您更新您的 RDSH 映像，來提供軟體更新或新的應用程式時，您將需要個別更新 RDSH 集合中每個部署維護這兩個部署間的共通的使用者經驗。 您可以使用[更新 RDSH 集合範本](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)，但請注意，在被動部署 RDS 基礎結構和 RDSH Vm 必須執行來執行範本。 

## <a name="failover"></a>容錯移轉

在主動-被動部署的情況下容錯移轉會要求您開始次要部署的 Vm。 您可以手動方式或透過自動化指令碼。 在 S2D SOFS 的重大容錯移轉的情況下變更儲存體複本合作關係的方向，使目的地磁碟區會變成來源磁碟區。 例如: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

您可以深入[叢集對叢集儲存體複寫](../../storage/storage-replica/cluster-to-cluster-storage-replication.md)。

Azure 流量管理員會自動識別主要部署失敗，而且次要部署狀況良好 （在 RD 閘道 Vm 已啟動 RG B 中） 並將次要部署的使用者流量導向。 若要繼續其遠端資源，享有一致的體驗，使用者可以使用相同的流量管理員 URL。 請注意，用戶端 DNS 快取不會更新記錄在 Azure 流量管理員組態中設定的 TTL 期間。

### <a name="test-failover"></a>測試容錯移轉
儲存體複本合作關係中只有一個磁碟區 （來源） 可以使用一次。 這表示當您切換 SR 合作關係的方向，主要的部署 (RG A) 中的磁碟區會變成目的地的複寫，並因此隱藏。 因此，連線到 RG 的任何使用者將不再有到他們的 Upd 中 RG A.SOFS 上儲存的存取 

若要測試容錯移轉，同時允許使用者繼續登入：
1. 在 RG B 中啟動的基礎結構 Vm 和 RDSH Vm
2. 切換 SR 合作關係的方向 （叢集 b-s2d c 會變成來源磁碟區）。
3. [停用端點](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint)RG a Azure 流量管理員設定檔中以強制將流量導向至 或者 RG B.ATM、 使用 PowerShell 指令碼：

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B 現在是作用中主要的部署。 若要切換回 RG A 作為主要的部署：

1. 切換 SR 合作關係的方向 （叢集的 s2d c 會變成來源磁碟區）：

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. 重新啟用 RG 的 Azure 流量管理員設定檔中的端點：

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>在內部部署的考量

雖然內部部署無法使用本文所提及的 Azure 快速入門範本，您可以手動實作基礎結構的所有角色。 在內部部署中成本不隨 Azure 耗用量，請考慮使用主動-主動模型更快速的容錯移轉。

您可以使用 Azure Traffic Manager 透過內部部署端點，但它需要 Azure 訂用帳戶。 或者，提供給終端使用者的 dns，讓他們只是將主要的部署使用者導向的 CNAME 記錄。 在容錯移轉的情況下修改重新導向到次要部署的 DNS CNAME 記錄。 如此一來，使用者會使用單一 URL，就像使用 Azure 流量管理員，可引導使用者適當的部署。 

如果您想要建立一個在內部部署-Azure-間模型時，請考慮使用[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)。
