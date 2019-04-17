---
title: 建立容錯移轉叢集
description: 如何建立適用於 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 容錯移轉叢集。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068976"
---
# 建立容錯移轉叢集

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主題示範如何使用容錯移轉叢集管理員嵌入式管理單元或 Windows PowerShell 建立容錯移轉叢集。 本主題涵蓋典型的部署中，在 Active Directory Domain Services (AD DS) 建立叢集和其相關聯的叢集的角色的電腦物件的位置。 如果您要部署儲存空間直接存取叢集，而是，請參閱[部署儲存空間直接存取](../storage/storage-spaces/deploy-storage-spaces-direct.md)。

您也可以部署 Active Directory 分離的叢集。 這種部署方法可讓您建立容錯移轉叢集，而不建立 AD DS 中的電腦物件或來要求該物件 prestaged AD DS 中的電腦需要的權限。 此選項僅可透過 Windows PowerShell 中，且只會建議您針對特定案例。 如需詳細資訊，請參閱 <<c0>部署 Active Directory-Detached 叢集。

#### 檢查清單︰ 建立容錯移轉叢集

|狀態|工作|參考資料|
|:---:|---|---|
|☐|確認必要條件|[確認必要條件](#verify-the-prerequisites)|
|☐|您想要作為叢集節點新增每個伺服器上安裝容錯移轉叢集功能|[安裝容錯移轉叢集功能](#install-the-failover-clustering-feature)|
|☐|執行叢集驗證精靈來驗證組態|[驗證設定](#validate-the-configuration)|
|☐|執行建立叢集精靈以建立容錯移轉叢集|[建立容錯移轉叢集](#create-the-failover-cluster)|
|☐|建立叢集的角色主機叢集工作負載|[建立叢集的角色](#create-clustered-roles)|

## 確認必要條件

在您開始之前，請確認下列先決條件：

- 請確定您想要新增為叢集節點的所有伺服器都執行相同版本的 Windows Server。
- 檢閱硬體需求，以確定支援您的設定。 如需詳細資訊，請參閱[容錯移轉叢集的硬體需求及存放選項](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11))。 如果您正在建立儲存空間直接存取叢集，請參閱[儲存空間直接存取硬體需求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)。
- 若要新增叢集的存放裝置叢集建立期間，請確定所有伺服器都可以都存取存放裝置。 （您也可以新增叢集儲存空間在建立叢集之後。）
- 請確定您想要新增為叢集節點的所有伺服器都加入相同的 Active Directory 網域。
- （選擇性）建立組織單位 (OU)，並移動您要做為叢集節點新增到 OU 的伺服器的電腦帳戶。 最佳做法是，我們建議您在他們自己的 OU AD DS 中放置容錯移轉叢集。 這可協助您更準確地控制哪些群組原則設定或安全性範本設定會影響在叢集節點。 藉由隔離在他們自己的 OU 中的叢集，它也有助於避免意外刪除叢集電腦物件。

此外，請確認下列帳戶需求：

- 請確定您要用來建立叢集的帳戶是網域使用者在您想要新增為叢集節點的所有伺服器上擁有系統管理員權限。
- 請確定下列其中一項為 true:
    - 建立叢集的使用者有 OU 或容器將形成叢集的伺服器所在的位置**建立電腦物件**權限。
    - 如果使用者沒有**建立電腦物件**的權限，要求網域系統管理員預先設置叢集電腦物件的叢集。 如需詳細資訊，請參閱[在 Active Directory Domain Services Prestage 叢集電腦物件](prestage-cluster-adds.md)。

>[!NOTE]
>如果您想要建立 Windows Server 2012 R2 中 Active Directory 分離的叢集，不適用此需求。 如需詳細資訊，請參閱 <<c0>部署 Active Directory-Detached 叢集。

## 安裝容錯移轉叢集功能

您必須在您想要做為容錯移轉叢集節點新增每個伺服器上安裝容錯移轉叢集的功能。

### 安裝容錯移轉叢集功能

1. 啟動 [伺服器管理員]。
2. 在 [**管理**\] 功能表中，選取 [**新增角色及功能**。
3. **開始之前，先**在頁面上，選取 [**下一步**。
4. **選取安裝類型**在頁面上，選取**角色型或功能型安裝**，然後再選取**下一步**。
5. 在 [**選取目的地伺服器**頁面，選取您想要用來安裝此功能，的伺服器，然後選取**下一步**。
6. **選取伺服器角色**在頁面上，選取 [**下一步**。
7. 在**選取功能**的頁面上，選取**容錯移轉叢集**的核取方塊。
8. 若要安裝容錯移轉叢集管理工具，選取 [**新增功能**，，然後選取 [**下一步**。
9. 在**確認安裝選項**頁面上，選取 [**安裝**]。
<br>重新啟動伺服器不需要容錯移轉叢集功能。

10. 安裝完成時，選取 [**關閉**]。
11. 重複這個程序，在您想要做為容錯移轉叢集節點新增每個伺服器上。

>[!NOTE]
>安裝容錯移轉叢集功能之後，我們建議您從 Windows Update 套用最新的更新。 此外，Windows Server 2012 為主的容錯移轉叢集，檢閱[建議 hotfix 與更新的 Windows Server 2012 為主的容錯移轉叢集，則](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove)Microsoft 支援文件，並安裝適用於任何更新。

## 驗證設定

建立容錯移轉叢集之前，我們強烈建議您先驗證的設定，以確定硬體與硬體設定是容錯移轉叢集與相容。 Microsoft 支援叢集解決方案，只有當完成設定通過所有驗證測試，以及如果認證的叢集節點正在執行的 Windows Server 版本的所有硬體。

>[!NOTE]
>您必須擁有至少兩個節點，若要執行所有測試。 如果您有只能有一個節點，許多重要的存放裝置測試無法執行。

### 執行叢集驗證測試

1. 已從 「 遠端伺服器管理工具，安裝容錯移轉叢集管理工具的電腦上或在您已安裝的容錯移轉叢集功能的伺服器上，啟動容錯移轉叢集管理員。 為此伺服器上，啟動 [伺服器管理員，然後選取 [**工具**] 功能表上的 [**容錯移轉叢集管理員**。
2. 在**容錯移轉叢集管理員 \]** 窗格中，在**管理**] 下選取**驗證設定**。
3. 在**您開始前**的頁面上，選取 [**下一步**。
4. 在**選取的伺服器或叢集**頁面上，在**輸入名稱**] 方塊中輸入的 NetBIOS 名稱或您打算將做為容錯移轉叢集節點中，新增的伺服器的完整的網域名稱，然後選取**新增**。 重複此步驟，針對每個您想要新增的伺服器。 若要在此同時新增多部伺服器，請以逗號或分號分隔名稱。 例如，格式*server1.contoso.com、 server2.contoso.com*中輸入名稱。 當您完成時，請選取 [**下一步**。
5. **測試選項**] 頁面上選取**執行所有測試 （建議選項）**，然後再選取**下一步**。
6. 在**確認**頁面上，選取 [**下一步**。

    Validating 頁面會顯示執行測試的狀態。
7. 在**摘要**頁面上，執行下列其中一項：
    
      - 如果結果會指出，已成功完成測試並設定適用於叢集，與您想要立即建立叢集，請確定已選取 [**建立現在使用已驗證的節點的叢集**核取方塊，然後選取 [**完成 \]**。 然後，繼續[建立容錯移轉叢集](#create-the-failover-cluster)程序的步驟 4。
      - 如果結果表示警告或失敗，請選取 [**檢視報告**以檢視詳細資料，並判斷必須修正的問題。 了解適用於特定的驗證測試一則警告指出這方面的容錯移轉叢集，也可支援，但可能不符合建議的最佳做法。
        
        >[!NOTE]
        >如果您收到警告，以驗證儲存空間持續保留測試，請參閱部落格文章[Windows 容錯移轉叢集驗證警告，指出您的磁碟不支援持續性的保留區的儲存空間](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/)如需詳細資訊。

如需硬體驗證測試的詳細資訊，請參閱[驗證容錯移轉叢集的硬體](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## 建立容錯移轉叢集

若要完成此步驟，請確定身分登入的使用者帳戶符合本主題的[確認必要條件](#verify-the-prerequisites)一節所述的需求。

1. 啟動 [伺服器管理員]。
2. [**工具**] 功能表中，選取 [**容錯移轉叢集管理員**。
3. 在**容錯移轉叢集管理員 \]** 窗格中，在**管理**] 下選取**建立叢集**。
    
    建立叢集精靈會開啟。
4. 在**您開始前**的頁面上，選取 [**下一步**。
5. 如果**選取的伺服器**頁面隨即顯示，在**輸入名稱**] 方塊中輸入的 NetBIOS 名稱或您打算將做為容錯移轉叢集節點中，新增的伺服器的完整的網域名稱，然後選取 [**新增**。 重複此步驟，針對每個您想要新增的伺服器。 若要在此同時新增多個伺服器，分隔逗號或分號名稱。 例如，格式*server1.contoso.com; server2.contoso.com*中輸入名稱。 當您完成時，請選取 [**下一步**。
    
    >[!NOTE]
    >如果您選擇建立叢集之後立即[設定驗證程序](#validate-the-configuration)中執行驗證，不會看到 [**選取的伺服器**] 頁面。 因此，您不需要重新輸入，都進行驗證的節點會自動新增到 \ [建立叢集精靈。
6. 如果您略過驗證較舊版本，**驗證警告**頁面隨即顯示。 我們強烈建議您執行叢集驗證。 Microsoft 支援通過所有驗證測試的叢集。 若要執行驗證測試，選取 [ **]**，，然後選取 [**下一步**。 [驗證組態](#validate-the-configuration)中所述，請完成驗證設定精靈。
7. **管理叢集的存取點**在頁面上，執行下列動作：
    
    1. 在**叢集名稱**] 方塊中，輸入您想要管理叢集使用的名稱。 之前您這麼做，請檢閱下列資訊：
        
          - 叢集建立期間，此名稱會登錄為叢集電腦物件 （也稱為*叢集名稱物件*或*CNO*） 在 AD DS 中。 如果您指定叢集的 NetBIOS 名稱，CNO 被建立叢集節點的電腦物件所在的相同位置。 這可以是預設的電腦容器或 OU。
          - 若要針對 CNO 指定不同的位置，您可以**叢集名稱**] 方塊中輸入 OU 的分辨的名稱。 例如： *CN = ClusterName，OU = 叢集，DC = Contoso，DC = com*。
          - 如果網域系統管理員已 prestaged CNO 比叢集節點的所在位置不同的 OU 中，指定網域系統管理員提供辨別的名稱。
    2. 如果伺服器不需要設定為使用 DHCP 的網路介面卡，您必須設定一或多個靜態 IP 位址為容錯移轉叢集。 選取您想要使用適用於叢集管理每個網路的旁邊的核取方塊。 選取在選取的網路，旁邊的**地址**欄位，然後輸入您想要指派至叢集 IP 位址。 此 IP 位址 （或地址） 將會與叢集名稱在網域名稱系統 (DNS) 相關聯。
    3. 當您完成時，請選取 [**下一步**。
8. 在**確認**頁面上，檢閱設定。 根據預設，會選取 [**新增至叢集所有符合資格的儲存體**核取方塊。 如果您想要執行下列其中一項動作，請清除此核取方塊：
    
      - 您想要稍後設定存放裝置。
      - 您計劃建立叢集的儲存空間透過容錯移轉叢集管理員或透過容錯移轉叢集的 Windows PowerShell cmdlet，而您尚未建立儲存空間中的檔案和存放服務。 如需詳細資訊，請參閱[部署叢集儲存空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。
9. 選取 [**下一步**就能建立容錯移轉叢集。
10. 在**摘要**頁面上，確認已成功建立容錯移轉叢集。 如果有任何警告或錯誤，檢視摘要的輸出，或選取 [檢視完整報告**檢視報告**。 選取 [**完成 \]**。
11. 若要確認已建立叢集，確認叢集名稱列在**容錯移轉叢集管理員**中瀏覽樹狀結構。 您可以展開叢集名稱，然後選取 [下方**節點**、**儲存**或檢視相關聯的資源**網路**項目。
    
    了解，這可能需要一些時間來成功複寫 DNS 中的叢集名稱。 DNS 註冊成功並之後複寫，如果您在伺服器管理員中，選取**所有伺服器**的叢集名稱應該會列與**線上**的**管理性**狀態的伺服器。

在建立叢集之後，您可以執行事項例如確認叢集中的仲裁設定，以及選擇性地建立叢集共用磁碟區 (CSV)。 如需詳細資訊，請參閱[了解儲存空間直接存取中的仲裁](../storage/storage-spaces/understand-quorum.md)和[使用容錯移轉叢集的叢集共用磁碟區](failover-cluster-csvs.md)。

## 建立叢集的角色

建立容錯移轉叢集之後，您可以建立叢集的角色主機叢集工作負載。

>[!NOTE]
>適用於叢集角色需要用戶端存取點，AD DS 中建立虛擬電腦物件 (VCO)。 根據預設，所有適用於叢集建立 Vco 相同的容器或 OU 中為 CNO。 了解建立叢集之後，您可以移動 CNO 到任何 OU。

以下說明如何建立叢集的角色：

1. 使用伺服器管理員或 Windows PowerShell 安裝角色或功能所需的每個容錯移轉叢集節點上的叢集角色。 例如，如果您想要建立叢集的檔案伺服器，在所有叢集節點上安裝的檔案伺服器角色。
    
    下表顯示您在高可用性精靈的相關聯的伺服器角色或功能，您必須安裝作為先決條件，也可以設定的叢集的角色。
    
    <table>
    <thead>
    <tr class="header">
    <th>叢集的角色</th>
    <th>角色或功能的必要條件</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>DFS 命名空間伺服器</td>
    <td>DFS 命名空間 （檔案伺服器角色的一部分）</td>
    </tr>
    <tr class="even">
    <td>DHCP 伺服器</td>
    <td>DHCP 伺服器角色</td>
    </tr>
    <tr class="odd">
    <td>分散式的交易協調器 (DTC)</td>
    <td>無</td>
    </tr>
    <tr class="even">
    <td>檔案伺服器</td>
    <td>檔案伺服器角色</td>
    </tr>
    <tr class="odd">
    <td>一般應用程式</td>
    <td>不適用</td>
    </tr>
    <tr class="even">
    <td>一般的指令碼</td>
    <td>不適用</td>
    </tr>
    <tr class="odd">
    <td>一般服務</td>
    <td>不適用</td>
    </tr>
    <tr class="even">
    <td>HYPER-V 複本代理程式</td>
    <td>HYPER-V 角色</td>
    </tr>
    <tr class="odd">
    <td>iSCSI 目標伺服器</td>
    <td>iSCSI 目標伺服器 （檔案伺服器角色的一部分）</td>
    </tr>
    <tr class="even">
    <td>iSNS 伺服器</td>
    <td>iSNS 伺服器服務功能</td>
    </tr>
    <tr class="odd">
    <td>訊息佇列</td>
    <td>訊息佇列服務的功能</td>
    </tr>
    <tr class="even">
    <td>其他伺服器</td>
    <td>無</td>
    </tr>
    <tr class="odd">
    <td>虛擬機器</td>
    <td>HYPER-V 角色</td>
    </tr>
    <tr class="even">
    <td>WINS 伺服器</td>
    <td>WINS 伺服器功能</td>
    </tr>
    </tbody>
    </table>
2. 在容錯移轉叢集管理員中，展開的叢集名稱、**角色**，以滑鼠右鍵按一下，然後選取 [**設定角色**。
3. 依照高可用性精靈來建立叢集的角色中的步驟。
4. 若要確認叢集的角色已建立，在 [**角色**] 窗格中，請確定角色有**執行**中的狀態。 角色窗格也會指出擁有者節點。 若要測試容錯移轉，以滑鼠右鍵按一下角色，指向**移動**，然後選取**\ [選取節點**。 在**移動叢集角色**] 對話方塊中，選取所需的叢集節點，，然後選取 **[確定]**。 在**擁有者 」 節點**欄中，確認擁有者節點變更。

## 使用 Windows PowerShell 建立容錯移轉叢集

下列 Windows PowerShell cmdlet 執行本主題中的相同的函式做為上述程序。 輸入每個 cmdlet 單行，即使它們可能會顯示 word 包裝跨數個行因為格式限制式。

>[!NOTE]
>您必須使用 Windows PowerShell 來建立在 Windows Server 2012 R2 中 Active Directory 分離的叢集。 如需語法的詳細資訊，請參閱 <<c0>部署 Active Directory-Detached 叢集。

下列範例會安裝容錯移轉叢集的功能。

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

下列範例會名為*Server1*和*Server2*的電腦上執行所有叢集驗證測試。

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>**測試叢集**cmdlett 會輸出結果，以在目前的工作目錄中的記錄檔。 例如： < 使用者名稱 > c:\users\<user \AppData\Local\Temp。

下列範例會建立容錯移轉叢集，並且節點*Server1* *MyCluster*和*Server2*名為、 指派靜態 IP 位址*192.168.1.12*，以及將所有符合資格的存放裝置新增至容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

下列範例會建立相同的容錯移轉叢集，如先前範例中，所示，但它不會符合資格的存放裝置新增至容錯移轉叢集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

下列範例會建立名為*MyCluster* *叢集*網域*Contoso.com*的 OU 中的叢集。

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

如需如何加入叢集的角色的範例，請參閱[新增 ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps)和[新增 ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps)等主題。

## 更多資訊

  - [容錯移轉叢集](failover-clustering.md)
  - [部署 HYPER-V 叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [向外延展檔案伺服器應用程式資料](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [部署 Active Directory 分離叢集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [使用客體叢集以提供高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Cluster-Aware Updating](cluster-aware-updating.md)
  - [新叢集](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [測試叢集](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
