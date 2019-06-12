---
Title: 在 Active Directory 中設定的叢集帳戶
ms.date: 11/12/2012
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: c15c33e31bf0bf7261097fbea110f2a0a788dab2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439752"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>在 Active Directory 中設定的叢集帳戶


適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008

在 Windows Server 中，當您建立的容錯移轉叢集，並設定叢集的服務或應用程式中，容錯移轉叢集精靈建立必要的 Active Directory 電腦帳戶 （也稱為電腦物件），並提供特定的權限。 精靈會建立叢集本身的電腦帳戶 （此帳戶也稱為叢集名稱物件或 CNO） 和大部分類型的叢集的服務和應用程式的電腦帳戶在 HYPER-V 虛擬機器的例外狀況。 容錯移轉叢集精靈會自動設定這些帳戶的權限。 如果變更的權限，他們必須變更回符合叢集需求。 本指南會說明這些 Active Directory 帳戶與權限，提供有關背景，為重要的是，並說明設定和管理帳戶的步驟。
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>Active Directory 帳戶所需的容錯移轉叢集概觀

本章節描述 Active Directory 電腦帳戶 （也稱為 Active Directory 電腦物件） 的重要容錯移轉叢集。 這些帳戶如下所示：

  - **用來建立叢集的使用者帳戶。** 這是用來啟動 [建立叢集精靈] 的使用者帳戶。 帳戶很重要的因為它提供叢集本身從中建立電腦帳戶的基礎。  
      
  - **叢集名稱帳戶。** （叢集本身的電腦帳戶也稱為叢集名稱物件或 CNO）。 此帳戶建立叢集精靈 」 會自動建立，而且名稱與叢集的名稱相同。 因為透過此帳戶，其他帳戶會自動建立在叢集上設定新的服務和應用程式，是非常重要的是，叢集名稱帳戶。 如果叢集名稱帳戶已刪除或權限會取走，其他帳戶無法建立為所需的叢集，叢集名稱帳戶還原，或正確的權限會恢復之前。  
      
    比方說，如果您建立叢集，稱為 Cluster1，再重新設定叢集列印伺服器叢集上呼叫 PrintServer1 Cluster1 帳戶在 Active Directory 中的必須保留正確的權限，因此它可以用來建立電腦名為 PrintServer1 的帳戶。  
      
    叢集名稱帳戶會建立 Active Directory 中的電腦帳戶之預設容器中。 根據預設，這是 「 電腦 」 容器，但網域系統管理員可以選擇將它重新導向至另一個容器或組織單位 (OU)。  
      
  - **叢集的服務或應用程式的電腦帳戶 （電腦物件）。** 這些帳戶會自動建立高可用性精靈建立的叢集的服務或應用程式，在 HYPER-V 虛擬機器的例外狀況的大部分類型的程序的一部分。 若要控制這些帳戶的必要權限授與叢集名稱帳戶。  
      
    比方說，如果您有一個稱為 Cluster1 的叢集，然後建立叢集的檔案伺服器，稱為 FileServer1，高可用性 精靈會建立稱為 FileServer1 Active Directory 電腦帳戶。 高可用性 精靈也可讓 Cluster1 帳戶必要的權限來控制 FileServer1 帳戶。  
      

下表描述這些帳戶所需的權限。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>帳戶</th>
<th>關於權限的詳細資料</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>用來建立叢集的帳戶</p></td>
<td><p>需要在將成為叢集節點的伺服器上的系統管理權限。 也需要<strong>建立電腦物件</strong>並<strong>讀取全部內容</strong>用於網域中的電腦帳戶的容器中的權限。</p></td>
</tr>
<tr class="even">
<td><p>叢集名稱帳戶 （叢集本身的電腦帳戶）</p></td>
<td><p>當執行 [建立叢集精靈] 時，它會用於網域中的電腦帳戶之預設容器中建立叢集名稱帳戶。 根據預設，叢集名稱帳戶 （例如其他電腦帳戶） 可以在網域中建立最多十個電腦帳戶。</p>
<p>如果您在建立叢集之前建立的叢集名稱帳戶 （叢集名稱物件） — 也就是預先設置帳戶，您必須提供<strong>建立電腦物件</strong>並<strong>讀取全部內容</strong>在網域中的電腦帳戶用於容器的權限。 您也必須停用帳戶，並且給予<strong>完全控制</strong>它將使用由系統管理員為叢集安裝的帳戶。 如需詳細資訊，請參閱 <<c0> <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)"> 步驟的預先設置叢集名稱帳戶</a>稍後在本指南中。</p></td>
</tr>
<tr class="odd">
<td><p>叢集的服務或應用程式的電腦帳戶</p></td>
<td><p>高可用性精靈時執行 （如果您要建立新的叢集的服務或應用程式），在大部分情況下，叢集服務的電腦帳戶或 Active Directory 中建立應用程式。 叢集名稱帳戶授與必要的權限來控制此帳戶。 例外狀況是叢集的 HYPER-V 虛擬機器： 沒有建立電腦帳戶是這個。</p>
<p>如果您預先設置叢集的服務或應用程式的電腦帳戶，您必須使用必要的權限來進行設定。 如需詳細資訊，請參閱 <<c0> <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)"> 預先設置帳戶的叢集的服務或應用程式的步驟</a>稍後在本指南中。</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> 在舊版的 Windows Server 中，有已叢集服務的帳戶。 自 Windows Server 2008，不過，叢集服務會自動執行提供的特定權限與服務所需權限的特殊內容中 (類似本機系統內容中，但與權限較為精簡)。 其他帳戶所需，不過，本指南中所述。 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>透過容錯移轉叢集精靈會建立的帳戶

下圖說明使用和建立電腦帳戶 （Active Directory 物件） 先前小節中所述。 這些帳戶派上用場時系統管理員執行 建立叢集精靈，然後再執行 高可用性精靈 （若要設定的叢集的服務或應用程式）。

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

請注意，上圖顯示單一系統管理員執行 [建立叢集精靈] 和 [高可用性精靈]。 不過，這可能是兩個不同的系統管理員使用兩個不同的使用者帳戶，如果這兩個帳戶有足夠的權限。 權限所述需求與容錯移轉叢集、 Active Directory 網域和帳戶，稍後在本指南中的更多詳細資料。

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>如果叢集所需的帳戶都會變更的問題可能造成的方式

下圖說明如果叢集名稱帳戶 （其中一個叢集所需的帳戶） 在建立叢集精靈 會自動建立它之後變更的問題可能造成的方式。

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

發生的圖所示的問題類型時，特定事件 （1193年、 1194年、 1206 或 1207年） 會記錄在事件檢視器中。 如需有關這些事件的詳細資訊，請參閱 < [ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271)。

請注意，是否已經到達的全網域的配額 （依預設，10） 建立電腦物件，可能會發生類似的問題建立的叢集的服務或應用程式的帳戶。 如果是，它可能是適當網域系統管理員增加配額，雖然這是全網域的設定，請仔細考慮之後，並確認先前圖表並不會之後，只應該變更，請洽詢描述您的情況。 如需詳細資訊，請參閱 <<c0> [ 疑難排解與叢集相關的 Active Directory 帳戶中的變更所造成的問題的步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)稍後在本指南中。

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>容錯移轉叢集、 Active Directory 網域和帳戶與相關的需求

叢集服務的上述三個區段所述，必須符合特定需求和應用程式可以成功地容錯移轉叢集上設定。 （在單一網域） 的叢集節點的位置，以及為叢集安裝的人員帳戶的權限層級，是關於最基本的需求。 如果符合這些需求的叢集所需的帳戶可以自動建立容錯移轉叢集精靈。 下列清單提供有關這些基本需求的詳細資料。

  - **節點：** 所有節點必須都位於相同的 Active Directory 網域。 （在 Windows NT 4.0，才會包含 Active Directory 無法基礎網域）。  
      
  - **為叢集安裝人員的帳戶：** 叢集安裝人員使用的帳戶必須具有下列特性：  
      
      - 帳戶必須是網域帳戶。 它不必是網域系統管理員帳戶。 如果符合其他需求，這份清單中的，它可以是網域使用者帳戶。  
          
      - 帳戶必須具有將成為叢集節點的伺服器上的系統管理權限。 提供這項服務最簡單的方式是建立網域使用者帳戶，並再將該帳戶新增至每個將成為叢集節點的伺服器上本機 Administrators 群組。 如需詳細資訊，請參閱 <<c0> [ 步驟，為叢集安裝人員設定帳戶](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)稍後在本指南中。  
          
      - 必須指定帳戶 （或群組帳戶的成員）**建立電腦物件**並**讀取全部內容**用於網域中的電腦帳戶的容器中的權限。 另一個替代方式是對帳戶的網域系統管理員帳戶。 如需詳細資訊，請參閱 <<c0> [ 步驟，為叢集安裝人員設定帳戶](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)稍後在本指南中。  
          
      - 如果您的組織選擇預先設置叢集名稱帳戶 （與叢集同名的電腦帳戶），預先設置的叢集名稱帳戶就必須提供 「 完全控制 」 權限給帳戶的叢集安裝人員。 如需如何預先設置叢集名稱帳戶其他重要詳細資訊，請參閱[步驟的預先設置叢集名稱帳戶](#steps-for-prestaging-the-cluster-name-account)稍後在本指南中。  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>事先計劃，密碼重設和其他帳戶維護

容錯移轉叢集的系統管理員有時可能需要重設叢集名稱帳戶的密碼。 這個動作需要特定的權限**重設密碼**權限。 因此，若要編輯的叢集名稱帳戶的權限 （透過使用 [Active Directory 使用者和電腦] 嵌入式管理單元） 的最佳作法是給叢集的系統管理員**重設密碼**叢集的權限名稱的帳戶。 如需詳細資訊，請參閱 <<c0> [ 帳戶的名稱與叢集的密碼問題的疑難排解步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)稍後在本指南中。

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>為叢集安裝人員設定帳戶的步驟

叢集安裝人員的帳戶很重要的因為它提供叢集本身從中建立電腦帳戶的基礎。

完成下列程序所需的最小的群組成員資格取決於您是否建立網域帳戶並將它指派必要的權限，在網域中，或是否會只放入 （由其他人建立） 的帳戶本機**系統管理員**群組會容錯移轉叢集中節點的伺服器上。 如果在前者的成員資格**Account Operators**或是**Domain Admins**，或同等權限，才能完成此程序的最小。 如果在本機的後者，成員資格**系統管理員**群組將會在容錯移轉叢集中節點的伺服器上或同等權限，只需要。 檢閱詳細使用適當帳戶和群組成員[ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>為叢集安裝人員設定帳戶

1.  建立或取得的叢集安裝人員的網域帳戶。 此帳戶可以是網域使用者帳戶或網域系統管理員帳戶 (在**Domain Admins**或對等的群組)。

2.  如果已建立，或在步驟 1 中取得的帳戶是網域使用者帳戶，或在網域中的網域系統管理員帳戶不會自動包含在本機**系統管理員**群組網域中的電腦上，加入將會是本機帳戶**系統管理員**群組會容錯移轉叢集中節點的伺服器上：
    
    1.  依序按一下 [開始]  、[系統管理工具]  ，然後按一下 [伺服器管理員]  。  
          
    2.  在主控台樹狀目錄中，依序展開**組態**，展開**本機使用者和群組**，然後展開**群組**。  
          
    3.  在中央窗格中，以滑鼠右鍵按一下**系統管理員**，按一下**新增到群組**，然後按一下**新增**。  
          
    4.  底下**輸入要選取的物件名稱**，輸入在步驟 1 中的已建立或取得的使用者帳戶的名稱。 出現提示時，輸入帳戶名稱和密碼具有足夠的權限，這項動作。 然後按一下 [**確定**]。  
          
    5.  重複上述步驟，將會容錯移轉叢集中的節點的每部伺服器上。  
          
    

> [!IMPORTANT]
> 必須將會在叢集中節點的所有伺服器上重複這些步驟。 
<br>


3. 如果已建立，或在步驟 1 中取得的帳戶是網域系統管理員帳戶，請略過此程序的其餘部分。 否則，請為帳戶提供**建立電腦物件**並**讀取全部內容**用於網域中的電腦帳戶的容器中的權限：
    
   1.  在網域控制站中，按一下**開始**，按一下**系統管理工具**，然後按一下**Active Directory 使用者和電腦**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。  
          
   2.  在上**檢視**功能表上，請確定**進階功能**已選取。  
          
       當**進階功能**已選取，您可以看到**安全性**索引標籤中的帳戶 （物件） 的屬性**Active Directory 使用者和電腦**。  
          
   3.  以滑鼠右鍵按一下 預設值**電腦**容器或預設容器在哪一台電腦帳戶會在您的網域中建立，然後按一下**屬性**。 **電腦**位於<b>Active Directory 使用者和電腦 /</b><i>網域節點</i><b>/Computers</b>。  
          
   4.  在 [安全性]  索引標籤上，按一下 [進階]  。  
          
   5.  按一下 **新增**，輸入步驟 1 中的建立或取得帳戶的名稱，然後按一下**確定**。  
          
   6.  在 **權限項目 * * * 容器*對話方塊方塊中，找出**建立電腦物件**並**讀取全部內容**權限，並確定**允許**針對每個已選取核取方塊。  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>預先設置叢集名稱帳戶的步驟

如果您不會預先設置叢集名稱帳戶，但改為允許建立並執行建立叢集精靈 時自動設定帳戶時，它會通常比較簡單。 不過，如果有必要為預先設置叢集名稱帳戶因為貴組織的需求，使用下列程序。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 檢閱詳細使用適當帳戶和群組成員[ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477)。 請注意，您可以使用相同的帳戶對於此程序因為您將使用建立叢集時。

#### <a name="to-prestage-a-cluster-name-account"></a>若要預先設置叢集名稱帳戶

1.  請確定您知道名稱，將會有叢集，並將建立叢集的人員所使用的使用者帳戶的名稱。 （請注意，您可以使用該帳戶執行此程序）。

2.  在網域控制站中，按一下**開始**，按一下**系統管理工具**，然後按一下**Active Directory 使用者和電腦**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**電腦**或網域中建立的電腦帳戶的預設容器。 **電腦**位於<b>Active Directory 使用者和電腦 /</b><i>網域節點</i><b>/Computers</b>。

4.  按一下 **的新**，然後按一下**電腦**。

5.  輸入名稱將用於容錯移轉叢集，也就是說，叢集名稱，將指定在建立叢集精靈中，然後按一下**確定**。

6.  以滑鼠右鍵按一下您剛才的帳戶建立，，然後按一下**停用的帳戶**。 如果系統提示您確認您的選擇，請按一下**是**。
    
    必須停用的帳戶以便時**建立叢集**執行精靈時，它可以確認它會在叢集中使用的帳戶不是目前使用現有的電腦或網域中的叢集。

7.  在上**檢視**功能表上，請確定**進階功能**已選取。
    
    當**進階功能**已選取，您可以看到**安全性**索引標籤中的帳戶 （物件） 的屬性**Active Directory 使用者和電腦**。

8.  以滑鼠右鍵按一下在步驟 3 中，以滑鼠右鍵按一下資料夾，然後按一下**屬性**。

9.  在 [安全性]  索引標籤上，按一下 [進階]  。

10. 按一下 **新增**，按一下**物件型別**並確定**電腦**已選取，然後按一下**確定**。 然後，在**輸入要選取的物件名稱**，輸入您剛才的電腦帳戶的名稱建立，，然後按一下**確定**。 如果出現訊息，指出，您將要新增已停用的物件，請按一下**確定**。

11. 在**權限項目**對話方塊方塊中，找出**建立電腦物件**並**讀取全部內容**權限，並確定**允許**針對每個已選取核取方塊。
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. 按一下  **確定**直到您回到**Active Directory 使用者和電腦**嵌入式管理單元。

13. 如果您使用相同的帳戶執行此程序，將用來建立叢集，請略過其餘的步驟。 否則，您必須設定權限，以便將用來建立叢集的使用者帳戶具有您剛才建立的電腦帳戶的 完全控制：
    
    1.  在上**檢視**功能表上，請確定**進階功能**已選取。  
          
    2.  以滑鼠右鍵按一下您剛才的電腦帳戶建立，，然後按一下**屬性**。  
          
    3.  在 [安全性]  索引標籤上，按一下 [新增]  。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。  
          
    4.  使用**選取使用者、 電腦或群組**對話方塊來指定將在建立叢集時使用的使用者帳戶。 然後按一下 [**確定**]。  
          
    5.  請確定已選取您剛才加入的使用者帳戶，並接著下, 一步**完全控制**，選取**允許**核取方塊。  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>預先設置帳戶的叢集的服務或應用程式的步驟

如果您不會預先設置叢集的服務或應用程式的電腦帳戶，而應允許建立及執行高可用性精靈 時自動設定帳戶時，它會通常比較簡單。 不過，如果必須預先設定的帳戶，因為貴組織的需求，使用下列程序。

中的成員資格**Account Operators**群組或對等項目，是要完成此程序，至少需要。 檢閱詳細使用適當帳戶和群組成員[ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>若要預先設置帳戶的叢集的服務或應用程式

1.  請確定您知道叢集的名稱和叢集的服務或應用程式會有的名稱。

2.  在網域控制站中，按一下**開始**，按一下**系統管理工具**，然後按一下**Active Directory 使用者和電腦**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

3.  在主控台樹狀目錄中，以滑鼠右鍵按一下**電腦**或網域中建立的電腦帳戶的預設容器。 **電腦**位於<b>Active Directory 使用者和電腦 /</b><i>網域節點</i><b>/Computers</b>。

4.  按一下 **的新**，然後按一下**電腦**。

5.  輸入名稱，您會使用叢集的服務或應用程式，然後按一下**確定**。

6.  在上**檢視**功能表上，請確定**進階功能**已選取。
    
    當**進階功能**已選取，您可以看到**安全性**索引標籤中的帳戶 （物件） 的屬性**Active Directory 使用者和電腦**。

7.  以滑鼠右鍵按一下您剛才的電腦帳戶建立，，然後按一下**屬性**。

8.  在 [安全性]  索引標籤上，按一下 [新增]  。

9.  按一下 **物件類型**並確定**電腦**已選取，然後按一下**確定**。 然後，在**輸入要選取的物件名稱**，輸入叢集名稱帳戶，然後按一下**確定**。 如果出現訊息，指出，您將要新增已停用的物件，請按一下**確定**。

10. 請確定選取 [叢集名稱帳戶，並接著下, 一步]**完全控制**，選取**允許**核取方塊。
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>疑難排解叢集所使用的帳戶的相關問題的步驟

本指南稍早所述，當您建立的容錯移轉叢集，並設定叢集的服務或應用程式時，容錯移轉叢集精靈會建立必要的 Active Directory 帳戶，並提供正確的權限。 如果所需的帳戶遭到刪除，或所需的權限變更，會造成問題。 下列各節會提供這些問題的疑難排解步驟。

  - [疑難排解與叢集名稱帳戶的密碼問題的步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [疑難排解叢集相關的 Active Directory 帳戶中的變更所造成的問題的步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>疑難排解與叢集名稱帳戶的密碼問題的步驟

如果沒有叢集身分識別，其中包含下列文字或電腦物件相關的事件訊息，請使用此程序。 請注意，此文字會在事件訊息，並不是在事件訊息的開頭：

`Logon failure: unknown user name or bad password.`

符合先前描述的事件訊息指出，叢集名稱帳戶的密碼與叢集軟體所儲存的對應密碼不再相符。

如需確保叢集系統管理員必須執行下列程序所需的正確權限資訊，請參閱重設密碼和其他帳戶維護，稍早在本指南中的繼續進行的規劃。

您至少必須有本機 **Administrators** 群組的成員資格或同等權限，才能完成此程序。 此外，您的帳戶必須具備**重設密碼**叢集名稱帳戶的權限 (除非您的帳戶**Domain Admins**帳戶，或叢集名稱帳戶的建立者擁有者)。 安裝叢集的人員所使用的帳戶可以使用此程序。 檢閱詳細使用適當帳戶和群組成員[ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>若要疑難排解與叢集名稱帳戶的密碼問題

1.  若要開啟 [容錯移轉叢集] 嵌入式管理單元，請依序按一下 [開始]  及 [系統管理工具]  ，再按一下 [容錯移轉叢集管理]  。 (如果**使用者帳戶控制** 對話方塊隨即出現，確認，它會顯示的動作是您想，然後按一下**繼續**。)

2.  在 [容錯移轉叢集管理] 嵌入式管理單元中，如果所要設定的叢集並未顯示，請在主控台樹狀目錄中，在 [容錯移轉叢集管理]  上按一下滑鼠右鍵，然後按一下 [管理叢集]  ，再選取或指定所要的叢集。

3.  在中央窗格中，依序展開**叢集核心資源**。

4.  底下**叢集名稱**，以滑鼠右鍵按一下**名稱**項目，指向**其他動作**，然後按一下**修復 Active Directory 物件**。

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>疑難排解叢集相關的 Active Directory 帳戶中的變更所造成的問題的步驟

如果叢集名稱帳戶遭到刪除，或權限會取走，當您嘗試設定新的叢集的服務或應用程式時，會發生問題。 若要疑難排解的問題，這可能是原因，請使用 Active Directory 使用者和電腦嵌入式管理單元來檢視或變更叢集名稱帳戶和其他相關帳戶。 如需這種問題時 （事件 1193年、 1194年、 1206 或 1207年），請參閱會記錄事件資訊[ http://go.microsoft.com/fwlink/?LinkId=118271 ](http://go.microsoft.com/fwlink/?linkid=118271)。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 檢閱詳細使用適當帳戶和群組成員[ http://go.microsoft.com/fwlink/?LinkId=83477 ](http://go.microsoft.com/fwlink/?linkid=83477)。

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>在 叢集相關的 Active Directory 帳戶的變更所造成的問題進行疑難排解

1.  在網域控制站中，按一下**開始**，按一下**系統管理工具**，然後按一下**Active Directory 使用者和電腦**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

2.  展開 預設值**電腦**容器或叢集名稱帳戶 （叢集的電腦帳戶） 所在的資料夾。 **電腦**位於<b>Active Directory 使用者和電腦 /</b><i>網域節點</i><b>/Computers</b>。

3.  檢查叢集名稱帳戶的圖示。 它不能向下箭號，也就是該帳戶必須不會停用。 如果它已遭停用，以滑鼠右鍵按一下它，並尋找命令**啟用帳戶**。 如果您看到命令中，按一下它。

4.  在上**檢視**功能表上，請確定**進階功能**已選取。
    
    當**進階功能**已選取，您可以看到**安全性**索引標籤中的帳戶 （物件） 的屬性**Active Directory 使用者和電腦**。

5.  以滑鼠右鍵按一下 預設值**電腦**容器或叢集名稱帳戶所在的資料夾。

6.  按一下 [內容]  。

7.  在 [安全性]  索引標籤上，按一下 [進階]  。

8.  在具有權限的帳戶清單中，按一下叢集名稱帳戶，然後**編輯**。
    

> [!NOTE]
> 如果未列出叢集名稱帳戶，請按一下<STRONG>新增</STRONG>並將它新增至清單。 
<br>


9. 叢集名稱帳戶 （也稱為叢集名稱物件或 CNO），請確認**允許**選取**建立電腦物件**並**讀取全部內容**權限。
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. 按一下  **確定**直到您回到**Active Directory 使用者和電腦**嵌入式管理單元。

11. 檢閱網域原則 （如果適用的話，網域系統管理員諮詢） 與建立電腦帳戶 （物件）。 請確定叢集名稱帳戶可以建立電腦帳戶，每次您設定的叢集的服務或應用程式。 例如，如果您的網域系統管理員已設定會導致所有新的電腦帳戶都建立在特製化的容器，而不是預設的設定**電腦**容器，請確定這些設定可讓叢集名稱帳戶，也該容器中建立新的電腦帳戶。

12. 展開 預設值**電腦**容器或其中一個叢集的服務或應用程式的電腦帳戶所在的容器。

13. 以滑鼠右鍵按一下其中一個叢集的服務或應用程式的電腦帳戶，然後按一下**屬性**。

14. 在 **安全性**索引標籤上，確認已列出叢集名稱帳戶的擁有權限，並加以選取的帳戶中。 確認叢集名稱帳戶具備**完全控制**權限 (**允許**核取方塊已選取)。 如果不存在，將叢集名稱帳戶新增至清單，並提供**完全控制**權限。
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. 針對每個叢集的服務和應用程式的叢集中設定重複步驟 13 14。

16. 請檢查，如建立電腦物件 （根據預設，10） 的全網域的配額尚未達到 （洽詢網域系統管理員如果適用的話）。 如果之前的項目，此程序中所有已檢閱並更正，且已達到配額，請考慮增加配額。 若要變更配額：
    
   1.  開啟系統管理員身分執行的命令提示字元**ADSIEdit.msc**。  
          
   2.  以滑鼠右鍵按一下**Adsi**，按一下**連接到**，然後按一下**確定**。 **預設命名內容**加入至主控台樹狀目錄中。  
          
   3.  按兩下**預設命名內容**下方，在網域物件上按一下滑鼠右鍵，然後按一下 **屬性**。  
          
   4.  捲動到  **ms-DS-MachineAccountQuota**，選取它，按一下**編輯**，變更此值，然後按一下 **確定**。

