---
title: 步驟 2 計劃的多站台的基礎結構
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2655f4de83576ef62b113419a69badaacc868f9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281001"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>步驟 2 計劃的多站台的基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

部署 「 遠端存取多站台的拓撲中的下一個步驟是完成多站台的基礎結構計劃;包括 Active Directory、 群組、 安全性群組和群組原則物件。  
## <a name="bkmk_2_1_AD"></a>2.1 規劃 Active Directory  
多站台部署遠端存取可以設定幾種拓撲：  
  
-   **單一 Active Directory 站台、 多個進入點**-您可以在此拓撲中，有單一的 Active Directory 站台來為整個組織在整個網站，快速的內部網路連結，但您有多個部署的遠端存取伺服器在整個組織，每個做為進入點。 此拓撲的地理位置範例是將單一 Active Directory 站台的美國東岸和西岸的進入點。  
  
    ![多站台的基礎結構](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **多個 Active Directory 站台、 多個進入點**-在此拓撲中，您必須有兩個或多個 Active Directory 站台與遠端存取伺服器做為進入點，每個站台部署。 每個遠端存取伺服器會與站台的 Active Directory 網域控制站相關聯。 此拓撲的地理位置範例是具有單一進入點，每個站台的 Active Directory 網站為美國和歐洲的其中一個。 請注意，是否您有多個 Active Directory 站台您不需要有與每個站台相關聯的進入點。 此外，某些 Active Directory 站台可以有多個與其相關聯的進入點。  
  
    ![多站台拓樸](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
在多站台進入點，您可以設定單一遠端存取伺服器，多部遠端存取伺服器，或遠端存取伺服器叢集。   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory 的最佳做法和建議  
請注意下列的建議和多站台的案例中的 Active Directory 部署的條件約束：  
  
1.  每個 Active Directory 站台可以包含一或多個遠端存取伺服器或伺服器叢集，做為用戶端電腦的多站台進入點。 不過將進入點不需要 Active Directory 站台。  
  
2.  只能與單一的 Active Directory 站台相關聯的多站台進入點。 當執行 Windows 8 的用戶端電腦連線到特定的進入點時，它們會被視為屬於該進入點相關聯的 Active Directory 站台。  
  
3.  建議每個 Active Directory 站台具有網域控制站。 網域控制站可以是唯讀狀態。  
  
4.  如果每個 Active Directory 站台包含網域控制站，GPO 中的進入點的伺服器是由其中一個與端點相關聯的 Active Directory 站台中的網域控制站管理。 如果該網站中有沒有可寫入網域控制站，在找到最接近的進入點中設定的第一個遠端存取伺服器的可寫入網域控制站上受管理伺服器的 GPO。 最接近取決於成本計算的連結。 請注意，在此案例中之後進行設定變更可能會有延遲，管理 GPO 的網域控制站和伺服器的 Active Directory 站台的唯讀網域控制站之間複寫時。  
  
5.  Gpo 主要網域控制站 (PDC) 模擬器以執行網域控制站上管理的用戶端 Gpo 和選擇性的應用程式伺服器。 這表示該 Gpo 並不一定是管理在 Active Directory 站台包含進入點的用戶端，用戶端連線。  
  
6.  如果無法連線到 Active Directory 站台的網域控制站，遠端存取伺服器將會連線到網站 （如果有的話） 中的其他網域控制站。 如果沒有，它會連接到另一個站台擷取更新的 GPO，以及驗證用戶端的網域控制站。 這兩種情況下，遠端存取管理主控台和 PowerShell cmdlet 無法用來擷取或修改組態設定，有可用的網域控制站為止。 請注意下列事項：  
  
    1.  如果與 PDC 模擬器執行的伺服器無法使用時，您必須指定已更新 Gpo 與 PDC 模擬器可用網域控制站。  
  
    2.  如果管理伺服器 GPO 的網域控制站無法使用時，使用 Set-daentrypointdc PowerShell cmdlet 來建立新的網域控制站與進入點的關聯。 執行 cmdlet 之前，新的網域控制站應該有最新的 Gpo。  
  
## <a name="bkmk_2_2_SG"></a>2.2 規劃安全性群組  
在部署期間的單一伺服器使用進階設定，所有存取內部部署網路透過 DirectAccess 的用戶端電腦所收集到安全性群組。 在多站台部署中，此安全性群組用於僅限 Windows 8 用戶端電腦。 針對多站台部署中，Windows 7 用戶端電腦會收集到不同的安全性群組，在多站台部署中每個進入點。 比方說，如果您先前分組 DA_Clients 群組中的所有用戶端電腦，您必須立即移除該群組中的任何 Windows 7 電腦並將它們放在不同的安全性群組。 比方說，多個 Active Directory 站台，多個進入點的拓撲，您建立的安全性群組，美國的進入點 (DA_Clients_US)，另一個用於歐洲的進入點 (DA_Clients_Europe)。 將位於 DA_Clients_US 群組中的 United States 和 DA_Clients_Europe 群組中的任何位於的歐洲的任何 Windows 7 用戶端電腦。 如果您沒有任何 Windows 7 用戶端電腦，您不需要規劃 Windows 7 電腦的安全性群組。  
  
必要的安全性群組如下所示：  
  
-   所有 Windows 8 用戶端電腦的一個安全群組。 建議為每個網域的這些用戶端建立唯一的安全性群組。  
  
-   唯一的安全性群組包含每個進入點的 Windows 7 用戶端電腦。 建議您建立的每個網域的唯一群組。 例如: Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 規劃群組原則物件  
在 遠端存取 」 部署期間設定的 DirectAccess 設定會收集到 Gpo。 為 DirectAccess 用戶端，遠端存取伺服器，並選擇性地為應用程式伺服器，您的單一伺服器部署已使用 Gpo。 多站台的部署需要下列 Gpo:  
  
-   以伺服器 GPO 的每個進入點。  
  
-   包含執行 Windows 8 的用戶端電腦的每個網域的 GPO。  
  
-   每個進入點與包含 Windows 7 用戶端電腦每個網域的 GPO。  
  
Gpo 設定如下：  
  
-   **自動**-您可以指定遠端存取 Gpo 會自動建立。 預設名稱指定每個 GPO，並可修改。  
  
-   **以手動方式**-您可以使用已由 Active Directory 系統管理員以手動方式建立的 Gpo。  
  
> [!NOTE]  
> 一旦設定 DirectAccess 使用特定的 Gpo，就無法設定使用不同的 Gpo。  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 自動建立的 Gpo  
使用自動建立的 GPO 時，請注意下列事項：  
  
-   自動建立的 GPO 會根據位置和連結目標參數進行套用，如下所示：  
  
    -   以伺服器 GPO 中，將 位置 與 連結的參數會指向包含遠端存取伺服器的網域。  
  
    -   用戶端 Gpo 中，連結目標設定為在其中建立 GPO 之網域的根目錄。 系統會針對每個包含用戶端電腦的網域建立 GPO 及 GPO 會連結到每個網域的根目錄。 .  
  
-   自動建立的 Gpo 來套用 DirectAccess 設定遠端存取伺服器系統管理員需要下列權限：  
  
    -   GPO 建立權限所需的網域。  
  
    -   所有選取之用戶端網域根目錄的連結權限。  
  
    -   如果 DirectAccess 已設定為行動裝置的電腦上需要建立 gpo 的 WMI 篩選器的權限。  
  
    -   在根目錄的網域相關聯的進入點 （伺服器 GPO 網域） 的連結權限  
  
    -   建議遠端存取系統管理員具有每個所需網域的 GPO 讀取權限。 這可讓遠端存取 」 確認多站台部署中建立 Gpo 時不存在有重複的名稱。  
  
    -   Active Directory 複寫權限進入點相關聯的網域。 這是因為當一開始新增進入點，該進入點最接近的網域控制站上建立伺服器 GPO 的進入點。 不過，PDC 模擬器僅支援連結建立作業，因為 GPO 必須從它建立與 PDC 模擬器執行之前先建立連結的網域控制站所在的網域控制站複寫。  
  
請注意，是否沒有正確的權限，針對複寫及連結 Gpo，會發出警告。 遠端存取作業會繼續進行，但不是會發生複寫和連結。 如果警告發出的連結將不會自動建立連結，即使權限亦同。 系統管理員將必須改為手動建立連結。  
  
### <a name="232-manually-created-gpos"></a>2.3.2 以手動方式建立的 Gpo  
使用手動建立的 GPO 時，請注意下列事項：  
  
-   多站台的部署應該手動建立下列 Gpo:  
  
    -   **伺服器 GPO**-伺服器 GPO 的每個進入點 （在進入點所在的網域）。 在進入點的每個遠端存取伺服器上，將會套用此 GPO。  
  
    -   **用戶端 GPO (Windows 7)** -A GPO 每個進入點與包含 Windows 7 用戶端電腦會連線到多站台部署中的進入點，每個網域。 例如 Domain1\DA_W7_Clients_GPO_Europe;Domain2\DA_W7_Clients_GPO_Europe;Domain1\DA_W7_Clients_GPO_US;Domain2\DA_W7_Clients_GPO_US。 如果沒有 Windows 7 用戶端電腦會連線到進入點，就不需要的 Gpo。  
  
-   沒有建立其他 Gpo 適用於 Windows 8 用戶端電腦的需求。 針對每個包含用戶端電腦的網域 GPO 已經建立了部署單一遠端存取伺服器時。 在多站台部署中這些用戶端 Gpo 將做為 Gpo 適用於 Windows 8 用戶端。  
  
-   Gpo 應該存在後，再按一下 [認可] 按鈕，在多站台部署精靈中。  
  
-   使用手動建立的 GPO 時，系統會搜尋整個網域來尋找 GPO 連結。 如果網域中未連結 GPO，則會在網域根目錄中自動建立連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
-   當使用以手動方式建立的 Gpo 來套用 DirectAccess 設定遠端存取伺服器系統管理員需要完整的 GPO 權限 （編輯、 刪除、 修改安全性） 手動建立的 Gpo。  
  
請注意，是否沒有正確的權限，針對複寫及連結 Gpo，會發出警告。 遠端存取作業會繼續進行，但不是會發生複寫和連結。 如果警告發出的連結將不會自動建立連結，即使權限亦同。 系統管理員將必須改為手動建立連結。  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 多網域控制站的環境中管理 Gpo  
每個 GPO 將受特定網域控制站，如下所示：  
  
-   伺服器 GPO 由其中一個伺服器時，相關聯的 Active Directory 站台中的網域控制站管理，或如果該站台中的網域控制站是唯讀的最接近的項目中第一部伺服器的可寫入的網域控制站點。  
  
-   用戶端 Gpo 將受與 PDC 模擬器執行網域控制站。  
  
如果您想要以手動方式修改 GPO 設定請注意下列：  
  
-   伺服器 Gpo，以識別哪一個網域控制站相關聯的特定進入點，執行 PowerShell cmdlet `Get-DAEntryPointDC -EntryPointName <name of entry point>`。  
  
-   根據預設，當您變更與網路的 PowerShell cmdlet 或 [群組原則管理] 主控台中，會使用做為 PDC 模擬器網域控制站。  
  
-   如果您修改不是 （適用於伺服器 Gpo） 的進入點或 PDC 模擬器 （適用於用戶端 Gpo） 相關聯的網域控制站的網域控制站上的設定，請注意下列各項：  
  
    1.  之前修改的設定，請確定網域控制站會複寫使用最新的 GPO，並[備份 GPO 設定](https://go.microsoft.com/fwlink/?LinkID=257928)，才能進行變更。 如果沒有更新 GPO，在複寫期間可能會發生合併衝突，因而導致損毀的 「 遠端存取 」 設定。  
  
    2.  在修改後的設定，您必須等候變更複寫到與 Gpo 相關聯的網域控制站。 請勿使用 遠端存取管理主控台 或 遠端存取 」 PowerShell cmdlet，在完成複寫之前的其他變更。 如果兩個不同的網域控制站上編輯 GPO，複寫完成之前，可能會發生合併衝突，導致設定損毀  
  
-   或者您可以變更預設會設定使用**變更網域控制站**對話方塊方塊中 [群組原則管理] 主控台中，或使用**Open-netgpo** PowerShell cmdlet，因此，變更對使用主控台] 或 [網路功能的 cmdlet 會使用您指定的網域控制站。  
  
    1.  若要在群組原則管理主控台中執行這項操作，以滑鼠右鍵按一下 [網域或站台] 容器，然後按一下**變更網域控制站**。  
  
    2.  若要在 PowerShell 中這麼做，指定 Open-netgpo cmdlet 的 DomainController 參數。 例如，若要啟用私人和公用設定檔，名為 domain1\DA_Server_GPO _Europe 使用名為歐洲 dc.corp.contoso.com 網域控制站的 GPO 上的 Windows 防火牆中，執行下列作業：  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>修改網域控制站關聯  
為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 在某些情況下，它可能需要將 gpo 指派不同的網域控制站：  
  
-   **網域控制站維護和停機時間**-管理 GPO 的網域控制站無法使用時，可能需要管理不同網域控制站上的 GPO。  
  
-   **設定散發的最佳化**-網路基礎結構變更後，它可能需要管理伺服器 GPO 的進入點網域控制站相同的 Active Directory 站台做為進入點。   
  
## <a name="bkmk_2_4_DNS"></a>2.4 規劃 DNS  
規劃站台部署的 DNS 時請注意下列事項：  
  
1.  用戶端電腦使用 ConnectTo 位址才能連線到遠端存取伺服器。 在您的部署中每個進入點會需要不同的 ConnectTo 位址。 每個項目點的 ConnectTo 位址中必須有公用 DNS 與您所選擇的位址必須符合您部署 IP-HTTPS 連線的 IP-HTTPS 憑證的主體名稱。   
  
2.  此外，遠端存取強制執行對稱式路由;因此，只有 Teredo 和 IP-HTTPS 用戶端可以連線時已啟用多站台部署。 若要讓原生 IPv6 用戶端連線，ConnectTo 位址 (若為 IP-HTTPS URL) 必須解析這兩個外部 （網際網路對向） IPv6 和 IPv4 位址上的遠端存取伺服器。  
  
3.  「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 單一伺服器設定期間的下列名稱應該已登錄在 DNS 中：  
  
    1.  directaccess WebProbeHost 應該解析為內部 IPv4 位址的遠端存取伺服器，或僅支援 IPv6 的環境中的 IPv6 位址。  
  
    2.  directaccess CorpConnectivityHost 應該為 localhost （回送） 位址的解析 (可能是:: 1 或 127.0.0.1，根據公司網路中部署 IPv6)。  
  
    在多站台部署中 Directaccess-webprobehost 其他 DNS 項目都必須建立每個進入點。 當新增進入點，遠端存取會嘗試自動建立此額外 Directaccess-webprobehost 項目。 不過，如果它失敗會顯示警告，而且您必須手動建立項目。  
  
    > [!NOTE]  
    > 在您的 DNS 基礎結構啟用 DNS 清除時，建議停用遠端存取會自動建立 DNS 項目清除。  
  

  



