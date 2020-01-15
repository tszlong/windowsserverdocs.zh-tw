---
title: 在 Active Directory 中設定的叢集帳戶
ms.date: 11/12/2012
ms.prod: windows-server
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 47f3a515379eb79f628a0ee97ef2c7965c4d8d50
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948159"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>在 Active Directory 中設定的叢集帳戶

適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

在 Windows Server 中，當您建立容錯移轉叢集並設定叢集服務或應用程式時，容錯移轉叢集的嚮導會建立必要的 Active Directory 電腦帳戶（也稱為電腦物件），並提供特定的許可權。 此程式會建立叢集本身的電腦帳戶（此帳戶也稱為叢集名稱物件或 CNO），以及適用于大部分叢集服務和應用程式類型的電腦帳戶，這是 Hyper-v 虛擬機器的例外狀況。 這些帳戶的許可權會由容錯移轉叢集的嚮導自動設定。 如果許可權已變更，必須將它們變更回以符合叢集需求。 本指南描述這些 Active Directory 帳戶和許可權、提供其重要性的背景，並說明設定和管理帳戶的步驟。
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>容錯移轉叢集所需的 Active Directory 帳戶總覽

本節說明對容錯移轉叢集而言很重要的 Active Directory 電腦帳戶（也稱為 Active Directory 電腦物件）。 這些帳戶如下所示：

  - **用來建立叢集的使用者帳戶。** 這是用來啟動「建立叢集嚮導」的使用者帳戶。 此帳戶很重要，因為它提供了為叢集本身建立電腦帳戶的基礎。  
      
  - **叢集名稱帳戶。** （叢集本身的電腦帳戶，也稱為叢集名稱物件或 CNO）。 此帳戶是由「建立叢集」 wizard 自動建立，並具有與叢集相同的名稱。 叢集名稱帳戶非常重要，因為透過此帳戶，當您在叢集上設定新的服務和應用程式時，會自動建立其他帳戶。 如果刪除叢集名稱帳戶或從中移除許可權，則在叢集名稱帳戶還原或復原正確許可權之前，無法依叢集的需要建立其他帳戶。  
      
    例如，如果您建立名為 Cluster1 的叢集，然後嘗試在您的叢集上設定名為 PrintServer1 的叢集列印伺服器，Active Directory 中的 Cluster1 帳戶就必須保留正確的許可權，才能用來建立電腦稱為 PrintServer1 的帳戶。  
      
    叢集名稱帳戶會建立在 Active Directory 中電腦帳戶的預設容器中。 根據預設，這是「電腦」容器，但網域系統管理員可以選擇將它重新導向至另一個容器或組織單位（OU）。  
      
  - **叢集服務或應用程式的電腦帳戶（電腦物件）。** 「高可用性」嚮導會自動建立這些帳戶，這是建立大部分類型的叢集服務或應用程式的過程中，這是 Hyper-v 虛擬機器的例外狀況。 叢集名稱帳戶會被授與控制這些帳戶所需的許可權。  
      
    例如，如果您有一個名為 Cluster1 的叢集，然後建立名為包含 fileserver1 的叢集檔案伺服器，高可用性的 wizard 會建立一個名為包含 fileserver1 的 Active Directory 電腦帳戶。 高可用性的 wizard 也會為 Cluster1 帳戶提供控制包含 fileserver1 帳戶的必要許可權。  
      

下表描述這些帳戶所需的許可權。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>[帳戶]</th>
<th>許可權的詳細資料</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>用來建立叢集的帳戶</p></td>
<td><p>需要將成為叢集節點之伺服器的系統管理許可權。 也需要在用於網域中電腦帳戶的容器中，<strong>建立電腦物件</strong>和<strong>讀取所有屬性</strong>許可權。</p></td>
</tr>
<tr class="even">
<td><p>叢集名稱帳戶（叢集本身的電腦帳戶）</p></td>
<td><p>執行「建立叢集」嚮導時，它會在用於網域中電腦帳戶的預設容器中，建立叢集名稱帳戶。 根據預設，叢集名稱帳戶（如同其他電腦帳戶）可以在網域中建立最多10個電腦帳戶。</p>
<p>如果您在建立叢集之前建立叢集名稱帳戶（叢集名稱物件）（也就是預先設置帳戶），您必須在用於網域中電腦帳戶的容器中，為它提供 [<strong>建立電腦物件</strong>] 和 [<strong>讀取所有屬性</strong>] 許可權。 您也必須停用帳戶，並對安裝叢集的系統管理員將使用的帳戶提供<strong>完整控制權</strong>。 如需詳細資訊，請參閱本指南稍後的預先設置叢集<a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">名稱帳戶的步驟</a>。</p></td>
</tr>
<tr class="odd">
<td><p>叢集服務或應用程式的電腦帳戶</p></td>
<td><p>執行高可用性 wizard （以建立新的叢集服務或應用程式）時，在大多數情況下，會在 Active Directory 中建立叢集服務或應用程式的電腦帳戶。 叢集名稱帳戶會被授與控制此帳戶所需的許可權。 例外狀況是叢集的 Hyper-v 虛擬機器：不會為此建立任何電腦帳戶。</p>
<p>如果您預先設置叢集服務或應用程式的電腦帳戶，您必須使用必要的許可權來設定它。 如需詳細資訊，請參閱本指南稍後的為叢集<a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">服務或應用程式預先設置帳戶的步驟</a>。</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> 在舊版的 Windows Server 中，有叢集服務的帳戶。 不過，由於 Windows Server 2008，叢集服務會自動在特殊內容中執行，以提供服務所需的特定許可權和許可權（類似本機系統內容，但具有較低的許可權）。 但是需要其他帳戶，如本指南中所述。 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>如何透過容錯移轉叢集中的嚮導建立帳戶

下圖說明如何使用和建立上一節所述的電腦帳戶（Active Directory 物件）。 當系統管理員執行「建立叢集」嚮導，然後執行「高可用性」 wizard （以設定叢集服務或應用程式）時，這些帳戶就會開始播放。

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

請注意，上圖顯示的單一系統管理員同時執行「建立叢集」嚮導和「高可用性」 wizard。 不過，如果兩個帳戶都有足夠的許可權，則這可能是兩個不同的系統管理員使用兩個不同的使用者帳戶。 在本指南稍後的與容錯移轉叢集、Active Directory 網域和帳戶相關的需求中，將會更詳細地說明這些許可權。

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>如果叢集所需的帳戶已變更，可能會產生問題的方式

下圖說明如果叢集名稱帳戶（叢集所需的其中一個帳戶）在由「建立叢集嚮導」自動建立後已變更，可能會產生問題的方式。

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

如果圖表中顯示的問題類型發生，則會將特定事件（1193、1194、1206或1207）記錄在事件檢視器中。 如需這些事件的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=118271](https://go.microsoft.com/fwlink/?linkid=118271)。

請注意，如果已達到用來建立電腦物件（預設為10）的全網域配額，則建立叢集服務或應用程式的帳戶時，可能會發生類似的問題。 如果有的話，最好洽詢網域系統管理員來增加配額，雖然這是全網域的設定，而且應該在仔細考慮之後才變更，而且只有在確認上一個圖表沒有描述您的情況。 如需詳細資訊，請參閱本指南稍後的[針對叢集相關 Active Directory 帳戶中的變更所造成的問題疑難排解步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)。

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>與容錯移轉叢集、Active Directory 網域和帳戶相關的需求

如前面三節所述，必須符合特定需求，才能在容錯移轉叢集上成功設定叢集服務和應用程式。 最基本的需求會考慮叢集節點的位置（在單一網域內），以及安裝叢集之人員帳戶的許可權層級。 如果符合這些需求，則容錯移轉叢集的嚮導可以自動建立叢集所需的其他帳戶。 下列清單提供有關這些基本需求的詳細資料。

  - **節點：** 所有節點都必須位於相同的 Active Directory 網域。 （網域不能以 Windows NT 4.0 為基礎，這不包含 Active Directory）。  
      
  - **安裝叢集的人員帳戶：** 安裝叢集的人員必須使用具有下列特性的帳戶：  
      
      - 帳戶必須是網域帳戶。 不需要是網域系統管理員帳戶。 如果它符合此清單中的其他需求，則可以是網域使用者帳戶。  
          
      - 帳戶必須具有將成為叢集節點之伺服器的系統管理許可權。 提供此項的最簡單方式是建立網域使用者帳戶，然後將該帳戶新增到將成為叢集節點之每部伺服器上的本機系統管理員群組。 如需詳細資訊，請參閱本指南稍後的[為安裝](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)叢集的人員設定帳戶的步驟。  
          
      - 帳戶（或帳戶所屬的群組）必須被授與容器中用於網域中電腦帳戶的 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權。 如需詳細資訊，請參閱本指南稍後的[為安裝](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)叢集的人員設定帳戶的步驟。  
          
      - 如果您的組織選擇預先設置叢集名稱帳戶（名稱與叢集相同的電腦帳戶），預先設置的叢集名稱帳戶就必須授與安裝叢集之人員帳戶的「完全控制」許可權。 如需有關如何預先設置叢集名稱帳戶的其他重要詳細資訊，請參閱本指南稍後的預先設置叢集[名稱帳戶的步驟](#steps-for-prestaging-the-cluster-name-account)。  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>針對密碼重設和其他帳戶維護進行提早規劃

容錯移轉叢集的系統管理員有時可能需要重設叢集名稱帳戶的密碼。 此動作需要特定許可權，也就是 [**重設密碼**] 許可權。 因此，最佳作法是編輯叢集名稱帳戶的許可權（藉由使用 [Active Directory 使用者和電腦] 嵌入式管理單元），將叢集名稱帳戶的 [**重設密碼**] 許可權授與叢集的系統管理員。 如需詳細資訊，請參閱本指南稍後的[針對叢集名稱帳戶的密碼問題進行疑難排解的步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)。

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>為安裝叢集的人員設定帳戶的步驟

安裝叢集之人員的帳戶很重要，因為它提供了為叢集本身建立電腦帳戶的基礎。

完成下列程式所需的最小群組成員資格取決於您是否要建立網域帳戶，並為其指派網域中的必要許可權，或您是否只將帳戶（由其他人建立）放入將成為容錯移轉叢集中節點之伺服器上的本機系統**管理員**群組。 若要完成此程式，至少需要**Account Operators**的成員資格或同等許可權。 如果是後者，則為容錯移轉叢集節點之伺服器上本機**Administrators**群組的成員資格或同等許可權，全都是必要的。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>為安裝叢集的人員設定帳戶

1.  建立或取得安裝叢集之人員的網域帳戶。 此帳戶可以是網域使用者帳戶或**帳戶操作員**帳戶。 如果您使用標準使用者帳戶，您必須在此程式稍後授與它一些額外的許可權。

2.  如果在步驟1中建立或取得的帳戶不會自動包含在網域中電腦的本機**administrators**群組中，請將該帳戶新增到將成為容錯移轉叢集中節點之伺服器上的本機系統**管理員**群組：
    
    1.  依序按一下 [開始]、[系統管理工具]，然後按一下 [伺服器管理員]。  
          
    2.  在主控台樹中，依序展開 [設定 **]、[** **本機使用者和群組**]，然後展開 [**群組**]。  
          
    3.  在中央窗格中，以滑鼠右鍵按一下 [系統**管理員**]，按一下 [**加入群組**]，然後按一下 [**新增**]。  
          
    4.  在 **[輸入物件名稱來選取**] 下，輸入在步驟1中建立或取得的使用者帳戶名稱。 如果出現提示，請輸入具有此動作之足夠許可權的帳戶名稱和密碼。 然後按一下 [**確定**]。  
          
    5.  在將成為容錯移轉叢集中節點的每部伺服器上重複這些步驟。  

> [!IMPORTANT]
> 這些步驟必須在將成為叢集中節點的所有伺服器上重複執行。 
<br>


3. 如果在步驟1中建立或取得的帳戶是網域系統管理員帳戶，請略過此程式的其餘部分。 否則，請在用於網域中電腦帳戶的容器中，將 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權提供給帳戶。
    
   1.  在網域控制站上，依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **繼續**]。  
          
   2.  在 [ **View** ] 功能表上，確認已選取 [ **Advanced Features** ]。  
          
       選取 [ **Advanced Features** ] 時，您可以在 [ **Active Directory 使用者和電腦**] 的 [帳戶（物件）] 屬性中看到 [**安全性**] 索引標籤。  
          
   3.  以滑鼠**按右鍵 [** 預設**電腦**] 容器，或在網域中建立電腦帳戶的預設容器，然後按一下 [內容]。 **電腦**位於<b>Active Directory 使用者和電腦/</b><i>網域節點</i><b>/Computers</b>中。  
          
   4.  在 [安全性] 索引標籤上，按一下 [進階]。  
          
   5.  按一下 [**新增**]，輸入在步驟1中建立或取得的帳戶名稱，然後按一下 **[確定]** 。  
          
   6.  在 [* ** * 容器的許可權專案*] 對話方塊中，找出 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權，並確定已針對每一個選項選取 [**允許**] 核取方塊。  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>預先設置叢集名稱帳戶的步驟

如果您未預先設置叢集名稱帳戶，而是改為在執行 [建立叢集嚮導] 時自動建立和設定帳戶，則通常會比較簡單。 不過，如果因為貴組織的需求而必須預先設置叢集名稱帳戶，請使用下列程式。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477)。 請注意，您可以在此程式中使用相同的帳戶，就像您在建立叢集時所使用的一樣。

#### <a name="to-prestage-a-cluster-name-account"></a>若要預先設置叢集名稱帳戶

1.  請確定您知道叢集將擁有的名稱，以及建立叢集的人員將使用的使用者帳戶名稱。 （請注意，您可以使用該帳戶來執行此程式。）

2.  在網域控制站上，依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **繼續**]。

3.  在主控台樹中，以滑鼠右鍵按一下在網域中建立電腦帳戶的 [**電腦**] 或 [預設容器]。 **電腦**位於<b>Active Directory 使用者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

4.  按一下 [**新增**]，然後按一下 [**電腦**]。

5.  輸入將用於容錯移轉叢集的名稱，換句話說，將在 [建立叢集嚮導] 中指定的叢集名稱，然後按一下 **[確定]** 。

6.  在您剛建立的帳戶上按一下滑鼠右鍵，然後按一下 [**停用帳戶**]。 如果系統提示您確認您的選擇，請按一下 **[是]** 。
    
    帳戶必須停用，如此一來，在執行「**建立**叢集」嚮導時，它可以確認網域中的現有電腦或叢集目前不會使用它要用於叢集的帳戶。

7.  在 [ **View** ] 功能表上，確認已選取 [ **Advanced Features** ]。
    
    選取 [ **Advanced Features** ] 時，您可以在 [ **Active Directory 使用者和電腦**] 的 [帳戶（物件）] 屬性中看到 [**安全性**] 索引標籤。

8.  以滑鼠右鍵按一下您在步驟3中按滑鼠右鍵的資料夾，然後按一下 [**屬性**]。

9.  在 [安全性] 索引標籤上，按一下 [進階]。

10. 按一下 [**新增**]，按一下 [**物件類型**]，並確認已選取 [**電腦**]，然後按一下 **[確定]** 。 然後，在 [**輸入物件名稱來選取**] 下，輸入您剛建立的電腦帳戶名稱，然後按一下 **[確定]** 。 如果出現訊息，指出您即將新增已停用的物件，請按一下 **[確定]** 。

11. 在 [**許可權專案**] 對話方塊中，找出 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權，並確定已為每個選項選取 [**允許**] 核取方塊。
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. 按一下 **[確定]** ，直到您回到 [ **Active Directory 使用者和電腦**] 嵌入式管理單元為止。

13. 如果您使用相同的帳戶來執行此程式，將用來建立叢集，請略過其餘的步驟。 否則，您必須設定許可權，讓將用來建立叢集的使用者帳戶完全控制您剛建立的電腦帳戶：
    
    1.  在 [ **View** ] 功能表上，確認已選取 [ **Advanced Features** ]。  
          
    2.  在您剛才建立的電腦帳戶**上按一下滑鼠**右鍵，然後按一下 [內容]。  
          
    3.  在 [安全性] 索引標籤上，按一下 [新增]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **繼續**]。  
          
    4.  使用 [**選取使用者、電腦或群組**] 對話方塊，即可指定建立叢集時將使用的使用者帳戶。 然後按一下 [**確定**]。  
          
    5.  請確定已選取您剛才新增的使用者帳戶，然後選取 [**完全控制**] 旁邊的 [**允許**] 核取方塊。  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>為叢集服務或應用程式預先設置帳戶的步驟

如果您不為叢集服務或應用程式預先設置電腦帳戶，而是改為在執行高可用性嚮導時自動建立和設定帳戶，通常會比較簡單。 不過，如果因為貴組織的需求而需要預先設置帳戶，請使用下列程式。

若要完成此程式，至少需要**Account Operators**群組的成員資格或同等許可權。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>為叢集服務或應用程式預先設置帳戶

1.  請確定您知道叢集的名稱以及叢集服務或應用程式將擁有的名稱。

2.  在網域控制站上，依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **繼續**]。

3.  在主控台樹中，以滑鼠右鍵按一下在網域中建立電腦帳戶的 [**電腦**] 或 [預設容器]。 **電腦**位於<b>Active Directory 使用者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

4.  按一下 [**新增**]，然後按一下 [**電腦**]。

5.  輸入您將用於叢集服務或應用程式的名稱，然後按一下 **[確定]** 。

6.  在 [ **View** ] 功能表上，確認已選取 [ **Advanced Features** ]。
    
    選取 [ **Advanced Features** ] 時，您可以在 [ **Active Directory 使用者和電腦**] 的 [帳戶（物件）] 屬性中看到 [**安全性**] 索引標籤。

7.  在您剛才建立的電腦帳戶**上按一下滑鼠**右鍵，然後按一下 [內容]。

8.  在 [安全性] 索引標籤上，按一下 [新增]。

9.  按一下 [**物件類型**]，並確認已選取 [**電腦**]，然後按一下 **[確定]** 。 然後，在 [**輸入物件名稱來選取**] 下，輸入叢集名稱帳戶，然後按一下 **[確定]** 。 如果出現訊息，指出您即將新增已停用的物件，請按一下 **[確定]** 。

10. 請確定已選取 [叢集名稱帳戶]，然後選取 [**完全控制**] 旁邊的 [**允許**] 核取方塊。
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>疑難排解叢集所用帳戶相關問題的步驟

如本指南稍早所述，當您建立容錯移轉叢集並設定叢集服務或應用程式時，容錯移轉叢集的嚮導會建立必要的 Active Directory 帳戶，並為其提供正確的許可權。 如果刪除了所需的帳戶，或變更了必要的許可權，就會產生問題。 下列小節提供疑難排解這些問題的步驟。

  - [針對叢集名稱帳戶的密碼問題進行疑難排解的步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [針對叢集相關 Active Directory 帳戶中的變更所造成的問題進行疑難排解的步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>針對叢集名稱帳戶的密碼問題進行疑難排解的步驟

如果有關于電腦物件的事件訊息，或包含下列文字的叢集身分識別，請使用此程式。 請注意，此文字會在事件訊息中，而不是在事件訊息的開頭：

`Logon failure: unknown user name or bad password.`

符合先前描述的事件訊息表示叢集名稱帳戶的密碼和叢集軟體所儲存的對應密碼不再相符。

如需確保叢集系統管理員有正確許可權可以視需要執行下列程式的相關資訊，請參閱本指南稍早的針對密碼重設和其他帳戶維護進行規劃。

您至少必須有本機 **Administrators** 群組的成員資格或同等權限，才能完成此程序。 此外，您的帳戶必須具有叢集名稱帳戶的 [**重設密碼**] 許可權（除非您的帳戶是**網域系統管理員**帳戶，或者是叢集名稱帳戶的 Creator 擁有者）。 安裝叢集的人員所使用的帳戶可用於此程式。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>疑難排解叢集名稱帳戶的密碼問題

1.  若要開啟 [容錯移轉叢集] 嵌入式管理單元，請依序按一下 [開始] 及 [系統管理工具]，再按一下 [容錯移轉叢集管理]。 （如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]）。

2.  在 [容錯移轉叢集管理] 嵌入式管理單元中，如果所要設定的叢集並未顯示，請在主控台樹狀目錄中，在 [容錯移轉叢集管理] 上按一下滑鼠右鍵，然後按一下 [管理叢集]，再選取或指定所要的叢集。

3.  在中央窗格中，展開 [叢集**核心資源**]。

4.  在 [叢集**名稱**] 底下，以滑鼠右鍵按一下**名稱**專案，指向 [**其他動作**]，然後按一下 [**修復 Active Directory 物件**]。

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>針對叢集相關 Active Directory 帳戶中的變更所造成的問題進行疑難排解的步驟

如果刪除叢集名稱帳戶，或從它移除許可權，當您嘗試設定新的叢集服務或應用程式時，就會發生問題。 若要疑難排解可能原因的問題，請使用 [Active Directory 使用者和電腦] 嵌入式管理單元來查看或變更叢集名稱帳戶和其他相關帳戶。 如需這類問題發生時所記錄事件的詳細資訊（事件1193、1194、1206或1207），請參閱[https://go.microsoft.com/fwlink/?LinkId=118271](https://go.microsoft.com/fwlink/?linkid=118271)。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱[https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>針對叢集相關 Active Directory 帳戶中的變更所造成的問題進行疑難排解

1.  在網域控制站上，依序按一下 [**開始**] 和 [系統**管理工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [ **繼續**]。

2.  展開 [預設**電腦**] 容器或叢集名稱帳戶（叢集的電腦帳戶）所在的資料夾。 **電腦**位於<b>Active Directory 使用者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

3.  檢查 [叢集名稱] 帳戶的圖示。 它不能有向下箭號，也就是說，帳戶不得停用。 如果似乎已停用，請以滑鼠右鍵按一下它，並尋找 [**啟用帳戶**] 命令。 如果您看到命令，請按一下它。

4.  在 [ **View** ] 功能表上，確認已選取 [ **Advanced Features** ]。
    
    選取 [ **Advanced Features** ] 時，您可以在 [ **Active Directory 使用者和電腦**] 的 [帳戶（物件）] 屬性中看到 [**安全性**] 索引標籤。

5.  在 [預設**電腦**] 容器或叢集名稱帳戶所在的資料夾上按一下滑鼠右鍵。

6.  按一下 **\[屬性\]** 。

7.  在 [安全性] 索引標籤上，按一下 [進階]。

8.  在具有許可權的帳戶清單中，按一下 [叢集名稱] 帳戶，然後按一下 [**編輯**]。
    

> [!NOTE]
> 如果未列出 [叢集名稱] 帳戶，請按一下 [<STRONG>新增</STRONG>]，並將它新增至清單。 
<br>


9. 針對叢集名稱帳戶（也稱為叢集名稱物件或 CNO），確定已針對 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權選取 [**允許**]。
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. 按一下 **[確定]** ，直到您回到 [ **Active Directory 使用者和電腦**] 嵌入式管理單元為止。

11. 請參閱與建立電腦帳戶（物件）相關的網域原則（如有適用的網域系統管理員諮詢）。 請確定每次您設定叢集服務或應用程式時，叢集名稱帳戶都可以建立電腦帳戶。 例如，如果您的網域系統管理員已設定讓所有新電腦帳戶建立在特殊容器中的設定，而不是預設的**電腦**容器，請確定這些設定也允許叢集名稱帳戶在該容器中建立新的電腦帳戶。

12. 展開 [預設**電腦**] 容器或其中一個叢集服務或應用程式的電腦帳戶所在的容器。

13. 在其中一個叢集服務或應用程式的電腦帳戶上按一下滑鼠右鍵，然後按一下 [**屬性**]。

14. 在 [**安全性**] 索引標籤上，確認叢集名稱帳戶已列在具有許可權的帳戶中，然後加以選取。 確認叢集名稱帳戶具有 [**完全控制**] 許可權（已選取 [**允許**] 核取方塊）。 如果沒有，請將叢集名稱帳戶新增到清單中，並授與**完全控制**許可權。
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. 針對叢集中所設定的每個叢集服務和應用程式，重複步驟13-14。

16. 檢查是否已達到用於建立電腦物件的全網域配額（預設為10）（諮詢網域系統管理員（如果適用））。 如果此程式中的先前專案已經過檢查並更正，而且已達到配額，請考慮增加配額。 若要變更配額：
    
   1.  以系統管理員身分開啟命令提示字元，並執行**ADSIEdit**。  
          
   2.  以滑鼠右鍵按一下**ADSI 編輯器**，按一下 **[連線到**]，然後按一下 **[確定]** 。 **預設的命名內容**會新增至主控台樹。  
          
   3.  按兩下 [**預設命名內容**]，以滑鼠右鍵按一下其底下的網域物件，然後按一下 [**屬性**]。  
          
   4.  依序選取 [ **MachineAccountQuota**]、[**編輯**]、[變更]、[值]，然後按一下 **[確定]** 。

