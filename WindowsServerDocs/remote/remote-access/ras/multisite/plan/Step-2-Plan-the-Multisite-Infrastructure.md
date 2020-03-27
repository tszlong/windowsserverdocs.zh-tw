---
title: 步驟2規劃多網站基礎結構
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6e23c3c3d22509af46b1a1741b545a787be00bfc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313885"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>步驟2規劃多網站基礎結構

>適用於：Windows Server (半年通道)、Windows Server 2016

在多網站拓撲中部署遠端存取的下一個步驟，是完成多網站基礎結構規劃;包括、Active Directory、安全性群組和群組原則物件。  
## <a name="21-plan-active-directory"></a><a name="bkmk_2_1_AD"></a>2.1 方案 Active Directory  
可以在數種拓撲中設定遠端存取多網站部署：  
  
-   **單一 Active Directory 網站，多個進入點**-在此拓撲中，您可以在整個組織中擁有單一 Active Directory 網站，其中包含整個網站的快速內部網路連結，但您在整個組織中部署了多部遠端存取服務器，每一部都作為進入點。 此拓撲的地理範例為美國地區的單一 Active Directory 網站，並在「東部」和「West coast」上使用進入點。  
  
    ![多網站基礎結構](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **多個 Active Directory 網站，多個進入點**-在此拓撲中，您有兩個或更多個 Active Directory 網站，其中有部署為每個網站進入點的遠端存取服務器。 每部遠端存取服務器都與網站的 Active Directory 網域控制站相關聯。 此拓撲的地理範例為美國地區的 Active Directory 網站，而一個適用于歐洲，每個網站都有單一進入點。 請注意，如果您有多個 Active Directory 網站，則不需要有與每個網站相關聯的進入點。 此外，某些 Active Directory 的網站可以有一個以上的相關聯進入點。  
  
    ![多網站拓撲](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
在多網站進入點中，您可以設定單一遠端存取服務器、多部遠端存取服務器，或遠端存取服務器群集。   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory 最佳做法和建議  
請注意下列在多站案例中 Active Directory 部署的建議和條件約束：  
  
1.  每個 Active Directory 網站可以包含一或多部遠端存取服務器或伺服器叢集，做為用戶端電腦的多網站進入點。 不過，Active Directory 網站不需要有進入點。  
  
2.  多站進入點只能與單一 Active Directory 網站相關聯。 當執行 Windows 8 的用戶端電腦連線到特定的進入點時，系統會將其視為屬於與該進入點相關聯的 Active Directory 網站。  
  
3.  建議每個 Active Directory 的網站都有網域控制站。 網域控制站可以是唯讀的。  
  
4.  如果每個 Active Directory 網站都包含網域控制站，則進入點中伺服器的 GPO 是由與端點相關聯之 Active Directory 網站中的其中一個網域控制站所管理。 如果該網站中沒有可寫入的網域控制站，則會在已啟用寫入的網域控制站上管理伺服器的 GPO，而此伺服器最接近進入點中設定的第一部遠端存取服務器。 最接近的取決於連結成本計算。 請注意，在此案例中，進行設定變更之後，在管理 GPO 的網域控制站與伺服器的 Active Directory 網站中的唯讀網域控制站之間進行複寫時，可能會有延遲。  
  
5.  用戶端 Gpo 和選用的應用程式伺服器 Gpo 是在作為主域控制站（PDC）模擬器執行的網域控制站上進行管理。 這表示用戶端 Gpo 不一定要在包含用戶端所連線之進入點的 Active Directory 網站中進行管理。  
  
6.  如果無法連線到 Active Directory 網站的網域控制站，則遠端存取服務器會連線到網站中的替代網域控制站（如果有的話）。 如果不是，它會連接到另一個網站的網域控制站，以抓取更新的 GPO，並驗證用戶端。 在這兩種情況下，都無法使用「遠端存取管理主控台」和 PowerShell Cmdlet 來抓取或修改設定，直到網域控制站可以使用。 請注意下列事項：  
  
    1.  如果以 PDC 模擬器的形式執行的伺服器無法使用，您必須指定已更新 Gpo 做為 PDC 模擬器的可用網域控制站。  
  
    2.  如果管理伺服器 GPO 的網域控制站無法使用，請使用 DAEntryPointDC PowerShell Cmdlet，將新的網域控制站與進入點產生關聯。 在執行此 Cmdlet 之前，新的網域控制站應該具有最新的 Gpo。  
  
## <a name="22-plan-security-groups"></a><a name="bkmk_2_2_SG"></a>2.2 規劃安全性群組  
在部署具有「高級」設定的單一伺服器期間，所有透過 DirectAccess 存取內部網路的用戶端電腦都會收集到安全性群組中。 在多網站部署中，此安全性群組僅用於 Windows 8 用戶端電腦。 針對多網站部署，會針對多網站部署中的每個進入點，將 Windows 7 用戶端電腦收集到個別的安全性群組。 例如，如果您先前將群組中的所有用戶端電腦分組 DA_Clients，則必須從該群組移除任何 Windows 7 電腦，並將它們放在不同的安全性群組中。 例如，在 [多個 Active Directory 網站]、[多個進入點拓撲] 中，您會建立美國進入點（DA_Clients_US）的安全性群組，以及一個用於歐洲進入點（DA_Clients_Europe）的群組。 將位於美國的任何 Windows 7 用戶端電腦放在 [DA_Clients_US] 群組中，並將任何位於歐洲的 [DA_Clients_Europe] 群組中。 如果您沒有任何 Windows 7 用戶端電腦，則不需要規劃 Windows 7 電腦的安全性群組。  
  
必要的安全性群組如下所示：  
  
-   適用于所有 Windows 8 用戶端電腦的一個安全性群組。 建議為每個網域的這些用戶端建立唯一的安全性群組。  
  
-   唯一的安全性群組，其中包含每個進入點的 Windows 7 用戶端電腦。 建議您為每個網域建立唯一的群組。 例如： Domain1 \ DA_Clients_Europe;Domain2> ... \ DA_Clients_Europe;Domain1 \ DA_Clients_US;Domain2> ... \ DA_Clients_US。  
  
## <a name="23-plan-group-policy-objects"></a><a name="bkmk_2_3_GPO"></a>2.3 計畫群組原則物件  
在遠端存取部署期間設定的 DirectAccess 設定會收集到 Gpo 中。 您的單一伺服器部署已針對 DirectAccess 用戶端、遠端存取服務器以及應用程式伺服器（選擇性）使用 Gpo。 多網站部署需要下列 Gpo：  
  
-   每個進入點的伺服器 GPO。  
  
-   包含執行 Windows 8 之用戶端電腦的每個網域的 GPO。  
  
-   每個進入點和包含 Windows 7 用戶端電腦的每個網域都有一個 GPO。  
  
Gpo 可以設定如下：  
  
-   **自動**-您可以指定由遠端存取自動建立 gpo。 預設名稱會針對每個 GPO 指定，而且可以修改。  
  
-   **手動**-您可以使用由 Active Directory 系統管理員手動建立的 gpo。  
  
> [!NOTE]  
> 一旦將 DirectAccess 設定為使用特定的 Gpo，就無法將其設定為使用不同的 Gpo。  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 自動建立的 Gpo  
使用自動建立的 GPO 時，請注意下列事項：  
  
-   自動建立的 GPO 會根據位置和連結目標參數進行套用，如下所示：  
  
    -   若為伺服器 GPO，location 和 link 參數都會指向包含遠端存取服務器的網域。  
  
    -   針對用戶端 Gpo，連結目標會設定為 GPO 建立所在網域的根目錄。 系統會為每個包含用戶端電腦的網域建立 GPO，而 GPO 會連結至每個網域的根目錄。 。  
  
-   針對自動建立的 Gpo，若要套用 DirectAccess 設定，遠端存取服務器系統管理員需要下列許可權：  
  
    -   必要網域的 GPO 建立許可權。  
  
    -   所有選取之用戶端網域根目錄的連結權限。  
  
    -   如果 DirectAccess 設定為僅在行動電腦上工作，則需要建立 Gpo 之 WMI 篩選器的許可權。  
  
    -   與進入點（伺服器 GPO 網域）相關聯之網域根目錄的連結許可權  
  
    -   建議遠端存取系統管理員具有每個所需網域的 GPO 讀取權限。 這會啟用遠端存取，以確認在建立多網站部署的 Gpo 時，名稱重複的 Gpo 不存在。  
  
    -   Active Directory 與進入點相關聯之網域的複寫許可權。 這是因為在一開始加入進入點時，會在最接近該進入點的網域控制站上建立進入點的伺服器 GPO。 不過，由於只有 PDC 模擬器支援連結建立，因此必須先從建立該 GPO 的網域控制站，將它複製到作為 PDC 模擬器執行的網域控制站，然後再建立連結。  
  
請注意，如果複寫和連結 Gpo 的正確許可權不存在，就會發出警告。 遠端存取作業將會繼續，但不會進行複寫和連結。 如果已發出連結警告，則即使許可權已就緒，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
### <a name="232-manually-created-gpos"></a>2.3.2 手動建立的 Gpo  
使用手動建立的 GPO 時，請注意下列事項：  
  
-   您應該針對多網站部署手動建立下列 Gpo：  
  
    -   **伺服器 gpo**-每個進入點的伺服器 gpo （在進入點所在的網域中）。 這個 GPO 會套用到進入點中的每個遠端存取服務器。  
  
    -   **用戶端 GPO （Windows 7）** -每個進入點的 GPO，以及每個包含將連線到多網站部署進入點的 Windows 7 用戶端電腦的網域。 例如 Domain1 \ DA_W7_Clients_GPO_Europe;Domain2> ... \ DA_W7_Clients_GPO_Europe;Domain1 \ DA_W7_Clients_GPO_US;Domain2> ... \ DA_W7_Clients_GPO_US。 如果沒有 Windows 7 用戶端電腦連接到進入點，就不需要 Gpo。  
  
-   不需要為 Windows 8 用戶端電腦建立額外的 Gpo。 部署單一遠端存取服務器時，已建立每個包含用戶端電腦之網域的 GPO。 在多網站部署中，這些用戶端 Gpo 將作為 Windows 8 用戶端的 Gpo。  
  
-   Gpo 應該先存在，然後再按一下多網站部署嚮導中的 [認可] 按鈕。  
  
-   使用手動建立的 GPO 時，系統會搜尋整個網域來尋找 GPO 連結。 如果網域中未連結 GPO，則會在網域根目錄中自動建立連結。 如果沒有可建立連結的必要權限，系統就會發出警告。  
  
-   使用手動建立的 Gpo 時，若要套用 DirectAccess 設定，遠端存取服務器管理員在手動建立的 Gpo 上需要完整的 GPO 許可權（[編輯]、[刪除]、[修改安全性]）。  
  
請注意，如果複寫和連結 Gpo 的正確許可權不存在，就會發出警告。 遠端存取作業將會繼續，但不會進行複寫和連結。 如果已發出連結警告，則即使許可權已就緒，也不會自動建立連結。 系統管理員將必須改為手動建立連結。  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 在多網域控制站環境中管理 Gpo  
每個 GPO 將由特定的網域控制站管理，如下所示：  
  
-   伺服器 GPO 是由與伺服器相關聯 Active Directory 網站中的其中一個網域控制站所管理，或者，如果該網站中的網域控制站是唯讀的，則是由最接近進入點中第一部伺服器的可寫入網域控制站。  
  
-   用戶端 Gpo 將由作為 PDC 模擬器執行的網域控制站來管理。  
  
如果您想要手動修改 GPO 設定，請注意下列事項：  
  
-   針對 [伺服器 Gpo]，若要識別哪一個網域控制站與特定的進入點相關聯，請執行 PowerShell Cmdlet `Get-DAEntryPointDC -EntryPointName <name of entry point>`。  
  
-   根據預設，當您使用網路 PowerShell Cmdlet 或群組原則管理主控台進行變更時，會使用擔任 PDC 模擬器的網域控制站。  
  
-   如果您在網域控制站上修改的設定不是與進入點（針對伺服器 Gpo）或 PDC 模擬器（適用于用戶端 Gpo）相關聯的網域控制站，請注意下列事項：  
  
    1.  修改設定之前，請先確定已使用最新的 GPO 複寫網域控制站，並在進行變更之前[備份 gpo 設定](https://go.microsoft.com/fwlink/?LinkID=257928)。 如果未更新 GPO，可能會在複寫期間發生合併衝突，而導致遠端存取設定損毀。  
  
    2.  修改設定之後，您必須等候變更複寫到與 Gpo 相關聯的網域控制站。 在複寫完成之前，請不要使用「遠端存取管理主控台」或「遠端存取」 PowerShell Cmdlet 進行其他變更。 如果在複寫完成之前在兩個不同的網域控制站上編輯 GPO，可能會發生合併衝突，而導致設定損毀。  
  
-   或者，您可以使用 [群組原則管理主控台] 中的 [**變更網域控制站**] 對話方塊，或使用 open-netgpo PowerShell Cmdlet 來變更預設設定，如此**一**來，使用主控台或網路 Cmdlet 所做的變更會使用您指定的網域控制站。  
  
    1.  若要在群組原則管理主控台中執行此操作，請以滑鼠右鍵按一下 [網域或網站] 容器，然後按一下 [**變更網域控制站**]。  
  
    2.  若要在 PowerShell 中執行這項操作，請指定 Open-netgpo 指令程式的 [控制器] 參數。 例如，若要在名為 domain1 \ DA_Server_GPO 的 GPO 上啟用 Windows 防火牆中的私用和公用設定檔，_Europe 使用名為 europe-dc.corp.contoso.com 的網域控制站，請執行下列動作：  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>修改網域控制站關聯  
為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 在某些情況下，可能需要為 GPO 指派不同的網域控制站：  
  
-   **網域控制站維護與停機**-當管理 gpo 的網域控制站無法使用時，可能需要在不同的網域控制站上管理 gpo。  
  
-   設定**發佈的優化**-在網路基礎結構變更之後，可能需要在與進入點相同的 Active Directory 網站中，管理網域控制站上進入點的伺服器 GPO。   
  
## <a name="24-plan-dns"></a><a name="bkmk_2_4_DNS"></a>2.4 規劃 DNS  
規劃多網站部署的 DNS 時，請注意下列事項：  
  
1.  用戶端電腦會使用 ConnectTo 位址來連線到遠端存取服務器。 部署中的每個進入點都需要不同的 ConnectTo 位址。 每個進入點 ConnectTo 位址都必須在公用 DNS 中提供，而且您選擇的位址必須符合您為 IP-HTTPS 連線部署的 ip-HTTPs 憑證的主體名稱。   
  
2.  此外，遠端存取會強制執行對稱路由;因此，只有 Teredo 和 IP-HTTPS 用戶端可以在啟用多網站部署時進行連線。 若要允許原生 IPv6 用戶端連線，ConnectTo 位址（IP-HTTPS URL）必須解析為遠端存取服務器上的外部（網際網路對向） IPv6 和 IPv4 位址。  
  
3.  「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 在設定單一伺服器期間，必須在 DNS 中註冊下列名稱：  
  
    1.  directaccess-Directaccess-webprobehost-應解析為遠端存取服務器的內部 IPv4 位址，或解析為僅 IPv6 環境中的 IPv6 位址。  
  
    2.  directaccess Directaccess-corpconnectivityhost-應根據公司網路中是否部署 IPv6，解析為 localhost （回送）位址（可能是：：1或127.0.0.1）。  
  
    在多網站部署中，必須為每個進入點建立 directaccess 的其他 DNS 專案 Directaccess-webprobehost。 新增進入點時，遠端存取會嘗試自動建立此額外的 directaccess-Directaccess-webprobehost 專案。 不過，如果失敗，則會顯示警告，而且您必須手動建立專案。  
  
    > [!NOTE]  
    > 在 DNS 基礎結構中啟用 DNS 清除時，建議您停用 [遠端存取] 自動建立的 DNS 專案上的清除。  
  

  



