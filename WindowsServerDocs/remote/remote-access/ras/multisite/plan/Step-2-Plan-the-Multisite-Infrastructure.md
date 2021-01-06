---
title: 步驟2規劃多網站基礎結構
description: 瞭解如何完成多網站基礎結構規劃;包括、Active Directory、安全性群組和群組原則物件。
manager: brianlic
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 460cf6c921e50ae0026f742fa92ed252f9eb773b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941704"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>步驟2規劃多網站基礎結構

>適用於：Windows Server (半年度管道)、Windows Server 2016

在多網站拓撲中部署遠端存取的下一步，是完成多網站基礎結構規劃;包括、Active Directory、安全性群組和群組原則物件。
## <a name="21-plan-active-directory"></a><a name="bkmk_2_1_AD"></a>2.1 規劃 Active Directory
遠端存取多網站部署可在多個拓撲中進行設定：

-   **單一 Active Directory 網站、多重進入點**-在此拓撲中，您的整個組織都有單一 Active Directory 網站，且整個網站都有快速的內部網路連結，但您的整個組織都有部署多個遠端存取服務器，每個都是進入點。 此拓撲的一個地理位置範例，是針對美國的單一 Active Directory 網站，以及位於東部海岸和 West coast 的進入點。

    ![多網站基礎結構](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)

-   **多個 Active Directory 網站，多個進入點**-在此拓撲中，您有兩個或多個 Active Directory 網站，且遠端存取服務器部署為每個網站的進入點。 每部遠端存取服務器都會與網站的 Active Directory 網域控制站相關聯。 此拓撲的一個地理範例是具有適用于美國的 Active Directory 網站，另一個則是每個網站單一進入點的歐洲地區。 請注意，如果您有多個 Active Directory 網站，您不需要有與每個網站相關聯的進入點。 此外，某些 Active Directory 網站可以有一個以上相關聯的進入點。

    ![多網站拓撲](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)

在多網站進入點，您可以設定單一遠端存取服務器、多部遠端存取服務器或遠端存取服務器群集。

### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory 最佳作法和建議
請注意下列適用于多網站案例中 Active Directory 部署的建議和限制：

1.  每個 Active Directory 網站可包含一或多個遠端存取服務器或伺服器叢集，作為用戶端電腦的多網站進入點。 不過，Active Directory 網站不需要有進入點。

2.  多網站進入點只能與單一 Active Directory 網站相關聯。 當執行 Windows 8 的用戶端電腦連線到特定進入點時，系統會將它們視為屬於與該進入點相關聯的 Active Directory 網站。

3.  建議每個 Active Directory 網站都有網域控制站。 網域控制站可以是唯讀的。

4.  如果每個 Active Directory 網站都包含網域控制站，進入點中伺服器的 GPO 是由與端點相關聯 Active Directory 網站中的其中一個網域控制站所管理。 如果該網站中沒有任何已啟用寫入的網域控制站，則會在已啟用寫入的網域控制站上管理伺服器的 GPO，該控制器最接近進入點中設定的第一部遠端存取服務器。 最接近的取決於連結成本計算。 請注意，在此案例中，在進行設定變更後，在管理 GPO 的網域控制站與伺服器 Active Directory 網站中的唯讀網域控制站之間進行複寫時，可能會有延遲。

5.  用戶端 Gpo 和選擇性的應用程式伺服器 Gpo 是在以網域主控站 (PDC) 模擬器的網域控制站上進行管理。 這表示不一定要在包含用戶端所連接之進入點的 Active Directory 網站中管理用戶端 Gpo。

6.  如果無法連線到 Active Directory 網站的網域控制站，則遠端存取服務器會連線到網站中的替代網域控制站（如有提供) ） (。 如果沒有，它會連線到另一個網站的網域控制站，以取得更新的 GPO 並驗證用戶端。 在這兩種情況下，除非有網域控制站可用，否則無法使用遠端存取管理主控台和 PowerShell Cmdlet 來取得或修改設定。 請注意：

    1.  如果以 PDC 模擬器的形式執行的伺服器無法使用，您必須指定一個已更新 Gpo 作為 PDC 模擬器的可用網域控制站。

    2.  如果無法使用管理伺服器 GPO 的網域控制站，請使用 Set-DAEntryPointDC PowerShell Cmdlet，將新的網域控制站與進入點產生關聯。 在執行 Cmdlet 之前，新的網域控制站應該具有最新的 Gpo。

## <a name="22-plan-security-groups"></a><a name="bkmk_2_2_SG"></a>2.2 規劃安全性群組
在具有 advanced settings 的單一伺服器部署期間，透過 DirectAccess 存取內部網路的所有用戶端電腦都會收集到安全性群組中。 在多網站部署中，此安全性群組只會用於 Windows 8 用戶端電腦。 針對多網站部署，Windows 7 用戶端電腦將會針對多網站部署中的每個進入點，收集到不同的安全性群組。 例如，如果您之前將群組中的所有用戶端電腦群組成群組 DA_Clients，您現在必須從該群組移除任何 Windows 7 電腦，並將它們放在不同的安全性群組中。 例如，在多個 Active Directory 網站中，多個進入點拓撲，您會建立美國進入點的安全性群組 (DA_Clients_US) ，而另一個用於歐洲進入點 (DA_Clients_Europe) 。 將位於美國的任何 Windows 7 用戶端電腦放在 DA_Clients_US 群組中，以及位於 DA_Clients_Europe 群組的歐洲地區。 如果您沒有任何 Windows 7 用戶端電腦，則不需要規劃 Windows 7 電腦的安全性群組。

必要的安全性群組如下所示：

-   適用于所有 Windows 8 用戶端電腦的一個安全性群組。 建議為每個網域建立這些用戶端的唯一安全性群組。

-   唯一的安全性群組，其中包含每個進入點的 Windows 7 用戶端電腦。 建議您為每個網域建立唯一的群組。 例如： Domain1 \ DA_Clients_Europe;>domain2.contoso.com \ DA_Clients_Europe;Domain1 \ DA_Clients_US;>domain2.contoso.com \ DA_Clients_US。

## <a name="23-plan-group-policy-objects"></a><a name="bkmk_2_3_GPO"></a>2.3 規劃群組原則物件
在遠端存取部署期間設定的 DirectAccess 設定會收集到 Gpo 中。 您的單一伺服器部署已針對 DirectAccess 用戶端、遠端存取服務器，以及選擇性的應用程式伺服器使用 Gpo。 多網站部署需要下列 Gpo：

-   每個進入點的伺服器 GPO。

-   包含執行 Windows 8 之用戶端電腦的每個網域的 GPO。

-   每個進入點以及包含 Windows 7 用戶端電腦的每個網域都有一個 GPO。

您可以依照下列方式設定 Gpo：

-   **自動**-您可以指定遠端存取會自動建立 gpo。 每個 GPO 都會指定預設名稱，而且可以修改。

-   **手動**-您可以使用 Active Directory 系統管理員手動建立的 gpo。

> [!NOTE]
> 一旦將 DirectAccess 設定為使用特定 Gpo，便無法將它設定為使用不同的 Gpo。

### <a name="231-automatically-created-gpos"></a>2.3.1 自動建立的 Gpo
使用自動建立的 GPO 時，請注意下列事項：

-   自動建立的 GPO 會根據位置和連結目標參數進行套用，如下所示：

    -   針對伺服器 GPO，location 和 link 參數會指向包含遠端存取服務器的網域。

    -   若為用戶端 Gpo，連結目標會設定為 GPO 建立所在網域的根目錄。 系統會為包含用戶端電腦的每個網域建立 GPO，並將 GPO 連結到每個網域的根目錄。 .

-   針對自動建立的 Gpo，若要套用 DirectAccess 設定，遠端存取服務器管理員需要下列許可權：

    -   GPO 建立必要網域的許可權。

    -   所有選取之用戶端網域根目錄的連結權限。

    -   如果 DirectAccess 設定為只在行動電腦上運作，則需要建立 Gpo 之 WMI 篩選器的許可權。

    -   與伺服器 GPO 網域 (的進入點相關聯之網域的根連結許可權) 

    -   建議遠端存取系統管理員具有每個所需網域的 GPO 讀取權限。 這可讓遠端存取在為多網站部署建立 Gpo 時，確認具有重複名稱的 Gpo 不存在。

    -   在與進入點相關聯的網域上 Active Directory 複寫許可權。 這是因為在一開始新增進入點時，會在最接近該進入點的網域控制站上建立進入點的伺服器 GPO。 不過，由於僅限 PDC 模擬器支援連結建立，因此 GPO 必須從建立該 GPO 的網域控制站複寫到作為 PDC 模擬器執行的網域控制站，然後再建立連結。

請注意，如果複寫和連結 Gpo 的正確許可權不存在，就會發出警告。 遠端存取作業將會繼續，但不會進行複寫和連結。 如果已發出連結警告，即使已有許可權，也不會自動建立連結。 系統管理員將必須改為手動建立連結。

### <a name="232-manually-created-gpos"></a>2.3.2 手動建立的 Gpo
使用手動建立的 GPO 時，請注意下列事項：

-   下列 Gpo 應針對多網站部署手動建立：

    -   **伺服器 gpo**-進入點所在網域中 (的每個進入點的伺服器 gpo) 。 此 GPO 會套用到進入點的每部遠端存取服務器上。

    -   **用戶端 GPO (Windows 7)**-每個進入點的 GPO，以及每個包含 Windows 7 用戶端電腦的網域，這些用戶端會連線到多網站部署中的進入點。 例如 Domain1 \ DA_W7_Clients_GPO_Europe;>domain2.contoso.com \ DA_W7_Clients_GPO_Europe;Domain1 \ DA_W7_Clients_GPO_US;>domain2.contoso.com \ DA_W7_Clients_GPO_US。 如果沒有任何 Windows 7 用戶端電腦會連線到進入點，就不需要 Gpo。

-   不需要為 Windows 8 用戶端電腦建立其他 Gpo。 部署單一遠端存取服務器時，已經建立每個包含用戶端電腦之網域的 GPO。 在多網站部署中，這些用戶端 Gpo 將作為 Windows 8 用戶端的 Gpo。

-   Gpo 應該存在，然後按一下 [多網站部署嚮導] 中的 [認可] 按鈕。

-   使用手動建立的 GPO 時，系統會搜尋整個網域來尋找 GPO 連結。 如果網域中未連結 GPO，則會在網域根目錄中自動建立連結。 如果沒有可建立連結的必要權限，系統就會發出警告。

-   使用手動建立的 Gpo 時，若要套用 DirectAccess 設定，遠端存取服務器管理員需要完整 GPO 許可權 (在手動建立的 Gpo 上編輯、刪除、修改安全性) 。

請注意，如果複寫和連結 Gpo 的正確許可權不存在，就會發出警告。 遠端存取作業將會繼續，但不會進行複寫和連結。 如果已發出連結警告，即使已有許可權，也不會自動建立連結。 系統管理員將必須改為手動建立連結。

### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>在多網域控制站環境中管理 Gpo 的2.3。3
每個 GPO 都會由特定的網域控制站管理，如下所示：

-   伺服器 GPO 是由與伺服器相關聯 Active Directory 網站中的其中一個網域控制站來管理，或是如果該網站中的網域控制站是唯讀的，則是由最接近進入點中第一部伺服器的可寫入網域控制站來管理。

-   用戶端 Gpo 將由執行為 PDC 模擬器的網域控制站來管理。

如果您想要手動修改 GPO 設定，請注意下列事項：

-   針對伺服器 Gpo，為了識別與特定進入點相關聯的網域控制站，請執行 PowerShell Cmdlet `Get-DAEntryPointDC -EntryPointName <name of entry point>` 。

-   依預設，當您使用網路 PowerShell Cmdlet 或群組原則管理主控台進行變更時，會使用作為 PDC 模擬器的網域控制站。

-   如果您修改的網域控制站設定不是與伺服器 Gpo 的進入點 (相關聯的網域控制站) 或用戶端 Gpo) 的 PDC 模擬器 (，請注意下列事項：

    1.  在修改設定之前，請先確定已使用最新的 GPO 複寫網域控制站，並在進行變更之前，先 [備份 gpo 設定](https://go.microsoft.com/fwlink/?LinkID=257928)。 如果 GPO 未更新，可能會發生複寫期間的合併衝突，導致遠端存取設定損毀。

    2.  修改設定之後，您必須等候變更複寫到與 Gpo 相關聯的網域控制站。 在複寫完成之前，請不要使用遠端存取管理主控台或遠端存取 PowerShell Cmdlet 進行其他變更。 如果在複寫完成之前在兩個不同的網域控制站上編輯 GPO，則可能會發生合併衝突，而導致設定損毀

-   或者，您可以使用群組原則管理主控台中的 [ **變更網域控制站** ] 對話方塊來變更預設設定，或使用 **NetGPO** PowerShell Cmdlet，以便使用主控台或網路 Cmdlet 所做的變更會使用您指定的網域控制站。

    1.  若要在群組原則管理主控台中這麼做，請以滑鼠右鍵按一下 [網域] 或 [網站] 容器，然後按一下 [ **變更網域控制站**]。

    2.  若要在 PowerShell 中這麼做，請指定 Open-NetGPO Cmdlet 的 [控制器] 參數。 例如，若要在名為 domain1 之 GPO 的 Windows 防火牆中啟用私人和公用設定檔（名為 \ DA_Server_GPO _Europe 使用名為 europe-dc.corp.contoso.com 的網域控制站，請執行下列動作：

        ```
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True
        Save-NetGPO -GpoSession $gpoSession
        ```

#### <a name="modifying-domain-controller-association"></a>修改網域控制站關聯
為了保持多站台部署中的設定一致，確認每一個 GPO 都是由單一網域控制站所管理非常重要。 在某些情況下，可能需要為 GPO 指派不同的網域控制站：

-   **網域控制站維護和停機**-當管理 gpo 的網域控制站無法使用時，可能需要在不同的網域控制站上管理 gpo。

-   設定 **分配的優化**-在網路基礎結構變更之後，可能需要在相同的 Active Directory 網站中，管理網域控制站上進入點的伺服器 GPO 作為進入點。

## <a name="24-plan-dns"></a><a name="bkmk_2_4_DNS"></a>2.4 規劃 DNS
針對多網站部署規劃 DNS 時，請注意下列事項：

1.  用戶端電腦會使用 ConnectTo 位址來連線到遠端存取服務器。 您部署中的每個進入點都需要不同的 ConnectTo 位址。 您必須在公用 DNS 中提供每個進入點 ConnectTo 位址，而您選擇的位址必須符合您針對 IP-HTTPS 連線所部署的 ip-HTTPs 憑證的主體名稱。

2.  此外，遠端存取會強制執行對稱路由;因此，當啟用多網站部署時，只有 Teredo 和 IP-HTTPS 用戶端可以連線。 若要允許原生 IPv6 用戶端連接，ConnectTo 位址 (IP-HTTPS URL) 必須解析為遠端存取服務器上的外部 (網際網路) IPv6 和 IPv4 位址。

3.  「遠端存取」會建立一個預設 Web 探查，供 DirectAccess 用戶端電腦用來確認對內部網路的連線能力。 在單一伺服器的設定期間，必須在 DNS 中註冊下列名稱：

    1.  directaccess-Directaccess-webprobehost-應解析為遠端存取服務器的內部 IPv4 位址，或解析為僅 IPv6 環境中的 IPv6 位址。

    2.  directaccess-Directaccess-corpconnectivityhost-應解析為 localhost (回送) 位址 (：：1或127.0.0.1，取決於是否在公司網路) 中部署 IPv6。

    在多網站部署中，必須為每個進入點建立 directaccess-Directaccess-webprobehost 的額外 DNS 專案。 新增進入點時，遠端存取會嘗試自動建立此額外的 directaccess Directaccess-webprobehost 專案。 但是，如果失敗，將會顯示警告，而且您必須手動建立此專案。

    > [!NOTE]
    > 當 DNS 基礎結構中啟用 DNS 清除時，建議您停用遠端存取自動建立的 DNS 專案上的清除。






