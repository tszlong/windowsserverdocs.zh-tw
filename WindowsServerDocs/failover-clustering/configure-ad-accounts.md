---
description: 深入瞭解：在 Active Directory 中設定叢集帳戶
title: 在 Active Directory 中設定的叢集帳戶
ms.date: 11/12/2012
author: JasonGerend
manager: lizross
ms.author: jgerend
ms.topic: how-to
ms.openlocfilehash: c25bece164d0ef946f2c78afc8a36668b080a40b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947584"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>在 Active Directory 中設定的叢集帳戶

適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

在 Windows Server 中，當您建立容錯移轉叢集並設定叢集服務或應用程式時，容錯移轉叢集的「容錯移轉叢集」會建立必要的 Active Directory 電腦帳戶 (也稱為「電腦) 物件」，並提供他們特定的許可權。 這些嚮導會建立叢集本身的電腦帳戶 (此帳戶也稱為叢集名稱物件或 CNO) ，以及大部分叢集服務和應用程式類型的電腦帳戶，這是 Hyper-v 虛擬機器的例外狀況。 這些帳戶的許可權是由 [容錯移轉叢集] 嚮導自動設定。 如果許可權已變更，則必須將它們變更回以符合叢集需求。 本指南將說明這些 Active Directory 的帳戶和許可權、提供其重要性的背景，以及描述設定及管理帳戶的步驟。


## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>有關容錯移轉叢集所需的 Active Directory 帳戶總覽

本節說明 Active Directory 的電腦帳戶 (也稱為容錯移轉叢集 Active Directory 的電腦物件) 。 這些帳戶如下所示：

  - **用來建立叢集的使用者帳戶。** 這是用來啟動 [建立叢集] 嚮導的使用者帳戶。 此帳戶很重要，因為它會提供針對叢集本身建立電腦帳戶的基礎。

  - **叢集名稱帳戶。**  (叢集本身的電腦帳戶，也稱為叢集名稱物件或 CNO) 。 此帳戶是由 [建立叢集] 嚮導自動建立，且名稱與叢集相同。 叢集名稱帳戶很重要，因為透過此帳戶，當您在叢集上設定新的服務和應用程式時，會自動建立其他帳戶。 如果刪除叢集名稱帳戶或從中移除許可權，則無法在叢集名稱帳戶還原或恢復正確的許可權之前，建立叢集所需的其他帳戶。

    例如，如果您建立名為 Cluster1 的叢集，然後嘗試在您的叢集上設定名為 PrintServer1 的叢集列印伺服器，Active Directory 中的 Cluster1 帳戶將需要保留正確的許可權，才能使用它來建立名為 PrintServer1 的電腦帳戶。

    叢集名稱帳戶會建立在 Active Directory 中電腦帳戶的預設容器中。 根據預設，這是「電腦」容器，但是網域系統管理員可以選擇將它重新導向至另一個容器或組織單位， (OU) 。

  - **電腦帳戶 (叢集服務或應用程式的電腦物件) 。** 這些帳戶會在建立大部分類型的叢集服務或應用程式的過程中自動建立，此為 Hyper-v 虛擬機器的例外狀況。 叢集名稱帳戶會獲得控制這些帳戶所需的許可權。

    例如，如果您有一個名為 Cluster1 的叢集，然後建立名為 FileServer1 的叢集檔案伺服器，高可用性的 wizard 會建立名為 FileServer1 的 Active Directory 電腦帳戶。 高可用性 wizard 也會為 Cluster1 帳戶提供控制 FileServer1 帳戶的必要許可權。


下表說明這些帳戶所需的許可權。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>帳戶</th>
<th>許可權的詳細資料</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>用來建立叢集的帳戶</p></td>
<td><p>需要將成為叢集節點之伺服器的系統管理許可權。 也需要在用於網域中電腦帳戶的容器中 <strong>建立電腦物件</strong> 和 <strong>讀取所有屬性</strong> 許可權。</p></td>
</tr>
<tr class="even">
<td><p>叢集名稱帳戶 (叢集本身的電腦帳戶) </p></td>
<td><p>當 [建立叢集] 嚮導執行時，它會在用於網域中電腦帳戶的預設容器中建立叢集名稱帳戶。 依預設，叢集名稱帳戶 (與其他電腦帳戶相同) 可以在網域中建立最多10個電腦帳戶。</p>
<p>如果您在建立叢集之前 (叢集名稱物件) 建立叢集名稱帳戶，也就是預先設置帳戶，您必須為其提供在網域中用於電腦帳戶之容器中的 [ <strong>建立電腦物件</strong> ] 和 [ <strong>讀取所有屬性</strong> ] 許可權。 您也必須停用帳戶，並將其 <strong>完全控制</strong> 授與安裝叢集的系統管理員將使用的帳戶。 如需詳細資訊，請參閱本指南稍後的預先設置叢集 <a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">名稱帳戶的步驟</a>。</p></td>
</tr>
<tr class="odd">
<td><p>叢集服務或應用程式的電腦帳戶</p></td>
<td><p>當高可用性嚮導執行 (建立新的叢集服務或應用程式) 時，在大部分情況下，會在 Active Directory 中建立叢集服務或應用程式的電腦帳戶。 叢集名稱帳戶會被授與控制此帳戶的必要許可權。 例外狀況是叢集的 Hyper-v 虛擬機器：不會為此建立任何電腦帳戶。</p>
<p>如果您為叢集服務或應用程式預先設置電腦帳戶，您必須使用必要的許可權進行設定。 如需詳細資訊，請參閱本指南稍後的 <a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">針對叢集服務或應用程式預先設置帳戶的步驟</a>。</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> 在舊版的 Windows Server 中，叢集服務有帳戶。 不過，由於 Windows Server 2008，叢集服務會自動在特殊的內容中執行，以提供服務所需的特定許可權和特殊許可權 (類似本機系統內容，但) 的許可權較少。 不過，也需要其他帳戶，如本指南所述。
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>如何透過容錯移轉叢集的嚮導建立帳戶

下圖說明如何使用和建立 (Active Directory 物件) 的電腦帳戶，如上一節所述。 當系統管理員執行 [建立叢集]，然後執行 [高可用性嚮導] (設定叢集服務或應用程式) 時，這些帳戶就會開始播放。

![使用和建立電腦帳戶](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

請注意，上圖顯示同時執行「建立叢集嚮導」和「高可用性嚮導」的單一系統管理員。 但是，如果這兩個帳戶都有足夠的許可權，則這可能是兩個不同的系統管理員使用兩個不同的使用者帳戶。 本指南稍後的「容錯移轉叢集」、「Active Directory 網域」和「帳戶」相關的需求會更詳細地說明這些許可權。

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>如果叢集所需的帳戶有所變更，可能會產生問題的方式

下圖說明當叢集名稱帳戶 (叢集所需的其中一個帳戶時，所產生的問題) 在建立叢集 wizard 自動建立之後變更。

![叢集名稱變更時的問題](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

如果出現在圖表中的問題類型，就會在事件檢視器中記錄特定事件 (1193、1194、1206或 1207) 。 如需這些事件的詳細資訊，請參閱 [https://go.microsoft.com/fwlink/?LinkId=118271](https://go.microsoft.com/fwlink/?linkid=118271) 。

請注意，如果建立電腦物件的全網域配額 (預設為 10) ，就可能會發生與建立叢集服務或應用程式的帳戶類似的問題。 如果有的話，最好洽詢網域系統管理員有關增加配額的資訊，雖然這是整個網域的設定，而且只有在仔細考慮之後才應變更，而且只有在確認上圖未描述您的情況之後才會變更。 如需詳細資訊，請參閱本指南稍後的 [針對叢集相關 Active Directory 帳戶的變更所造成的問題疑難排解步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)。

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>與容錯移轉叢集、Active Directory 網域和帳戶相關的需求

如前面三個章節所述，必須符合特定需求，才能在容錯移轉叢集上成功設定叢集服務和應用程式。 最基本的需求是在單一網域內 (叢集節點的位置，) 以及安裝叢集之人員帳戶的許可權層級。 如果符合這些需求，則 [容錯移轉叢集] 嚮導會自動建立叢集所需的其他帳戶。 下列清單提供這些基本需求的詳細資料。

  - **節點：** 所有節點都必須位於相同的 Active Directory 網域中。  (網域不能以 Windows NT 4.0 為基礎，但不包含 Active Directory。 ) 

  - **安裝叢集的人員帳戶：** 安裝叢集的人員必須使用具有下列特性的帳戶：

      - 帳戶必須是網域帳戶。 它不一定是網域系統管理員帳戶。 如果符合此清單中的其他需求，則可以是網域使用者帳戶。

      - 帳戶必須具有將成為叢集節點之伺服器的系統管理許可權。 提供此功能的最簡單方式是建立網域使用者帳戶，然後將該帳戶新增至將成為叢集節點之每部伺服器上的本機系統管理員群組。 如需詳細資訊，請參閱本指南稍後的針對 [安裝叢集的人員設定帳戶的步驟](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)。

      - 帳戶 (或帳戶所屬的群組) 必須獲得在該網域中用於電腦帳戶之容器中的 [ **建立電腦物件** ] 和 [ **讀取所有屬性** ] 許可權。 如需詳細資訊，請參閱本指南稍後的針對 [安裝叢集的人員設定帳戶的步驟](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)。

      - 如果您的組織選擇預先設置叢集名稱帳戶 (與叢集) 名稱相同的電腦帳戶，預先設置的叢集名稱帳戶必須將「完全控制」許可權授與安裝叢集之人員的帳戶。 如需有關如何預先設置叢集名稱帳戶的其他重要詳細資訊，請參閱本指南稍後的預先設置叢集 [名稱帳戶的步驟](#steps-for-prestaging-the-cluster-name-account)。


### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>預先規劃密碼重設和其他帳戶維護

容錯移轉叢集的系統管理員有時候可能需要重設叢集名稱帳戶的密碼。 此動作需要特定許可權，也就是 [ **重設密碼** ] 許可權。 因此，最佳作法是使用 Active Directory 消費者和電腦嵌入式) 管理單元來編輯叢集名稱帳戶 (的許可權，以授與叢集的系統管理員該叢集名稱帳戶的 [ **重設密碼** ] 許可權。 如需詳細資訊，請參閱本指南稍後的 [針對叢集名稱帳戶的密碼問題進行疑難排解的步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)。

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>為安裝叢集的人員設定帳戶的步驟

安裝叢集之人員的帳戶很重要，因為它會提供建立叢集本身的電腦帳戶基礎。

完成下列程式所需的最小群組成員資格，取決於您是建立網域帳戶並為它指派網域中的必要許可權，還是您只是將帳戶 (建立的帳戶) 到將成為容錯移轉叢集節點的伺服器上的本機 **Administrators** 群組。 如果之前是 **帳戶操作員** 的成員資格或同等許可權，則至少需要有成員資格，才能完成此程式。 如果是後者，將成為容錯移轉叢集中節點的本機 **Administrators** 群組成員資格或同等許可權，就是必要的。 請參閱中有關使用適當帳戶和群組成員資格的詳細資料 [https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477) 。

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>為安裝叢集的人員設定帳戶

1.  為安裝叢集的人員建立或取得網域帳戶。 此帳戶可以是網域使用者帳戶或 **帳戶操作員** 帳戶。 如果您使用標準使用者帳戶，您必須在此程式稍後提供額外的許可權。

2.  如果在步驟1中建立或取得的帳戶不會自動包含在網域電腦上的本機系統 **管理員** 群組中，請將該帳戶新增到將成為容錯移轉叢集中之節點的伺服器上的本機系統 **管理員** 群組：

    1.  依序按一下 [開始]、[系統管理工具]，然後按一下 [伺服器管理員]。

    2.  在主控台樹狀結構中，依序展開 [設定 **]、[****本機使用者和群組**]，然後展開 [**群組**]。

    3.  在中央窗格中，以滑鼠右鍵按一下 [系統 **管理員**]，按一下 [ **加入群組**]，然後按一下 [ **新增**]。

    4.  在 **[輸入要選取的物件名稱**] 底下，輸入在步驟1中建立或取得的使用者帳戶名稱。 如果出現提示，請輸入具有此動作的足夠許可權的帳戶名稱和密碼。 然後按一下 [確定] 。

    5.  在將成為容錯移轉叢集中節點的每部伺服器上重複這些步驟。

> [!IMPORTANT]
> 這些步驟必須在將成為叢集中節點的所有伺服器上重複執行。
<br>


3. 如果在步驟1中建立或取得的帳戶是網域系統管理員帳戶，請略過此程式的其餘步驟。 否則，請為帳戶提供在用於網域中電腦帳戶之容器中的 [ **建立電腦物件** ] 和 [ **讀取所有屬性** ] 許可權：

   1.  在網域控制站上，按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

   2.  在 [ **View** ] 功能表上，確定已選取 [ **Advanced Features** ]。

       當選取 [ **Advanced] 功能** 時，您可以在 [ **Active Directory 消費者和電腦**] 的 [ (物件的帳戶] 屬性) 中看到 [**安全性**] 索引標籤。

   3.  以滑鼠右鍵按一下 [預設 **電腦** ] 容器，或在您的網域中建立電腦帳戶的預設容器 **上按一下滑鼠** 右鍵，然後按一下 [內容]。 **電腦** 位於 <b>Active Directory 消費者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

   4.  在 [安全性] 索引標籤上，按一下 [進階]。

   5.  按一下 [ **新增**]，輸入在步驟1中建立或取得之帳戶的名稱，然後按一下 **[確定]**。

   6.  在 [_容器_ 的 **許可權專案**] 對話方塊中，找出 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權，並確定已針對每個許可權選取 [**允許**] 核取方塊。

       ![顯示 [建立電腦物件] 選項設定為 [允許] 的螢幕擷取畫面。 ](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>預先設置叢集名稱帳戶的步驟

如果您未預先設置叢集名稱帳戶，但改為在您執行「建立叢集嚮導」時自動建立及設定帳戶，通常會比較簡單。 但是，如果您的組織需要預先設置叢集名稱帳戶，請使用下列程式。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 請參閱中有關使用適當帳戶和群組成員資格的詳細資料 [https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477) 。 請注意，您可以在建立叢集時使用相同的帳戶做為此程式。

#### <a name="to-prestage-a-cluster-name-account"></a>預先設置叢集名稱帳戶

1.  確定您知道叢集將擁有的名稱，以及建立叢集的人員將會使用的使用者帳戶名稱。  (請注意，您可以使用該帳戶來執行此程式。 ) 

2.  在網域控制站上，按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

3.  在主控台樹中，以滑鼠右鍵按一下在網域中建立電腦帳戶的 [ **電腦** ] 或 [預設容器]。 **電腦** 位於 <b>Active Directory 消費者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

4.  按一下 [ **新增** ]，然後按一下 [ **電腦**]。

5.  輸入將用於容錯移轉叢集的名稱，亦即，將在 [建立叢集] 嚮導中指定的叢集名稱，然後按一下 **[確定]**。

6.  以滑鼠右鍵按一下您剛才建立的帳戶，然後按一下 [ **停用帳戶**]。 如果系統提示您確認您的選擇，請按一下 **[是]**。

    帳戶必須停用，如此一來，在執行「 **建立** 叢集」嚮導時，該帳戶就可以確認它將用於叢集的帳戶目前未由網域中的現有電腦或叢集使用中。

7.  在 [ **View** ] 功能表上，確定已選取 [ **Advanced Features** ]。

    當選取 [ **Advanced] 功能** 時，您可以在 [ **Active Directory 消費者和電腦**] 的 [ (物件的帳戶] 屬性) 中看到 [**安全性**] 索引標籤。

8.  以滑鼠右鍵按一下您在步驟3中以滑鼠右鍵按一下的資料夾，然後按一下 [ **屬性**]。

9.  在 [安全性] 索引標籤上，按一下 [進階]。

10. 按一下 [ **新增**]，然後按一下 [ **物件類型** ]，確定已選取 [ **電腦** ]，然後按一下 **[確定]**。 然後，在 **[輸入物件名稱來選取**] 下方，輸入您剛才建立的電腦帳戶名稱，然後按一下 **[確定]**。 如果出現訊息，指出您將要新增已停用的物件，請按一下 **[確定]**。

11. 在 [ **許可權專案** ] 對話方塊中，找出 [ **建立電腦物件** ] 和 [ **讀取所有屬性** ] 許可權，並確定已針對每個許可權選取 [ **允許** ] 核取方塊。

    ![[許可權專案] 對話方塊](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. 按一下 **[確定]** ，直到您回到 **Active Directory 消費者和電腦** 嵌入式管理單元為止。

13. 如果您使用相同的帳戶來執行此程式，因為它會用來建立叢集，請略過其餘的步驟。 否則，您必須設定許可權，讓用來建立叢集的使用者帳戶可以完全控制您剛才建立的電腦帳戶：

    1.  在 [ **View** ] 功能表上，確定已選取 [ **Advanced Features** ]。

    2.  在您剛才建立的電腦帳戶 **上按一下滑鼠** 右鍵，然後按一下 [內容]。

    3.  在 [安全性] 索引標籤上，按一下 [新增]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

    4.  使用 [ **選取使用者、電腦或群組** ] 對話方塊，即可指定建立叢集時將使用的使用者帳戶。 然後按一下 [確定] 。

    5.  確定已選取您剛剛新增的使用者帳戶，然後選取 [ **完全控制**] 旁的 [ **允許** ] 核取方塊。

        ![在 [Cluster1 屬性] 對話方塊中顯示 [安全性] 索引標籤的螢幕擷取畫面。](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>為叢集服務或應用程式預先設置帳戶的步驟

如果您沒有為叢集服務或應用程式預先設置電腦帳戶，但改為在您執行「高可用性嚮導」時自動建立及設定帳戶，通常會比較簡單。 不過，如果需要預先設置帳戶，因為您的組織中的需求，請使用下列程式。

若要完成此程式，至少需要 **Account Operators** 群組的成員資格或同等許可權。 請參閱中有關使用適當帳戶和群組成員資格的詳細資料 [https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477) 。

### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>為叢集服務或應用程式預先設置帳戶

1. 請確定您知道叢集的名稱，以及叢集服務或應用程式將擁有的名稱。

2. 在網域控制站上，按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

3. 在主控台樹中，以滑鼠右鍵按一下在網域中建立電腦帳戶的 [ **電腦** ] 或 [預設容器]。 **電腦** 位於 <b>Active Directory 消費者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

4. 按一下 [ **新增** ]，然後按一下 [ **電腦**]。

5. 輸入您將用於叢集服務或應用程式的名稱，然後按一下 **[確定]**。

6. 在 [ **View** ] 功能表上，確定已選取 [ **Advanced Features** ]。

    當選取 [ **Advanced] 功能** 時，您可以在 [ **Active Directory 消費者和電腦**] 的 [ (物件的帳戶] 屬性) 中看到 [**安全性**] 索引標籤。

7.  在您剛才建立的電腦帳戶 **上按一下滑鼠** 右鍵，然後按一下 [內容]。

8.  在 [安全性] 索引標籤上，按一下 [新增]。

9.  按一下 [ **物件類型** ]，確認已選取 [ **電腦** ]，然後按一下 **[確定]**。 然後，在 **[輸入要選取的物件名稱**] 底下，輸入叢集名稱帳戶，然後按一下 **[確定]**。 如果出現訊息，指出您將要新增已停用的物件，請按一下 **[確定]**。

10. 確認已選取 [叢集名稱] 帳戶，然後選取 [ **完全控制**] 旁的 [ **允許** ] 核取方塊。

    ![[安全性] 索引標籤](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>針對叢集所使用帳戶的相關問題進行疑難排解的步驟

如本指南稍早所述，當您建立容錯移轉叢集並設定叢集服務或應用程式時，容錯移轉叢集的嚮導會建立必要的 Active Directory 帳戶，並為其提供正確的許可權。 如果需要刪除所需的帳戶，或變更必要的許可權，可能會產生問題。 下列小節提供針對這些問題進行疑難排解的步驟。

  - [針對叢集名稱帳戶的密碼問題進行疑難排解的步驟](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)

  - [針對叢集相關 Active Directory 帳戶中的變更所造成的問題進行疑難排解的步驟](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>針對叢集名稱帳戶的密碼問題進行疑難排解的步驟

如果有關于電腦物件的事件訊息，或包含下列文字的叢集身分識別，請使用此程式。 請注意，此文字會出現在事件訊息中，而不是在事件訊息的開頭：

`Logon failure: unknown user name or bad password.`

符合先前描述的事件訊息表示叢集名稱帳戶的密碼和叢集軟體所儲存的對應密碼不再相符。

如需確保叢集系統管理員有權視需要執行下列程式的詳細資訊，請參閱本指南稍早的密碼重設和其他帳戶維護的事先規劃。

您至少必須有本機 **Administrators** 群組的成員資格或同等權限，才能完成此程序。 此外，除非您的帳戶是 **Domain Admins** 帳戶，或是叢集名稱帳戶的建立者擁有者) ，否則您的帳戶必須具有叢集名稱帳戶的 [**重設密碼**] 許可權 (。 安裝叢集的人員所使用的帳戶可用於此程式。 請參閱中有關使用適當帳戶和群組成員資格的詳細資料 [https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477) 。

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>針對叢集名稱帳戶的密碼問題進行疑難排解

1.  若要開啟 [容錯移轉叢集] 嵌入式管理單元，請按一下 [**開始**]，然後按 [**系統管理工具**]，再按一下 [**容錯移轉叢集管理**]。  (如果出現 [ **使用者帳戶控制** ] 對話方塊，請確認它所顯示的動作就是您想要的動作，然後按一下 [ **繼續**]。 ) 

2.  如果在 [容錯移轉叢集管理] 嵌入式管理單元中沒有顯示您要管理的叢集，請在主控台樹狀目錄中以滑鼠右鍵按一下 [**容錯移轉叢集管理**]，然後按一下 [**管理叢集**]，再選取或指定所要的叢集。

3.  在中央窗格中，展開 [叢集 **核心資源**]。

4.  在 [叢集 **名稱**] 底下，以滑鼠右鍵按一下 [ **名稱** ] 專案，指向 [ **其他動作**]，然後按一下 [ **修復 Active Directory 物件**]。

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>針對叢集相關 Active Directory 帳戶中的變更所造成的問題進行疑難排解的步驟

如果刪除叢集名稱帳戶，或移除其許可權，則當您嘗試設定新的叢集服務或應用程式時，將會發生問題。 若要針對可能原因的問題進行疑難排解，請使用 Active Directory 消費者和電腦嵌入式管理單元來查看或變更叢集名稱帳戶和其他相關帳戶。 如需有關此類型問題發生時所記錄之事件的資訊 (事件1193、1194、1206或 1207) 的詳細資訊，請參閱 [https://go.microsoft.com/fwlink/?LinkId=118271](https://go.microsoft.com/fwlink/?linkid=118271) 。

若要完成此程序，至少需要 **Domain Admins** 群組的成員資格或同等權限。 請參閱中有關使用適當帳戶和群組成員資格的詳細資料 [https://go.microsoft.com/fwlink/?LinkId=83477](https://go.microsoft.com/fwlink/?linkid=83477) 。

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>針對叢集相關 Active Directory 帳戶的變更所造成的問題進行疑難排解

1.  在網域控制站上，按一下 [ **開始**]，按一下 [系統 **管理工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**繼續**]。

2.  展開 [預設 **電腦** ] 容器或叢集名稱帳戶 (叢集) 的電腦帳戶所在的資料夾。 **電腦** 位於 <b>Active Directory 消費者和電腦/</b><i>網域節點</i><b>/Computers</b>中。

3.  檢查叢集名稱帳戶的圖示。 它不能有向下箭號，也就是帳戶不得停用。 如果看起來已停用，請在該檔案上按一下滑鼠右鍵，然後尋找 [ **啟用帳戶**] 命令。 如果您看到此命令，請按一下它。

4.  在 [ **View** ] 功能表上，確定已選取 [ **Advanced Features** ]。

    當選取 [ **Advanced] 功能** 時，您可以在 [ **Active Directory 消費者和電腦**] 的 [ (物件的帳戶] 屬性) 中看到 [**安全性**] 索引標籤。

5.  在 [預設 **電腦** ] 容器或叢集名稱帳戶所在的資料夾上按一下滑鼠右鍵。

6.  按一下 **[屬性]**。

7.  在 [安全性] 索引標籤上，按一下 [進階]。

8.  在具有許可權的帳戶清單中，按一下 [叢集名稱] 帳戶，然後按一下 [ **編輯**]。


> [!NOTE]
> 如果未列出叢集名稱帳戶，請按一下 [ <STRONG>新增</STRONG> ]，並將它新增至清單。
<br>


9. 針對叢集名稱帳戶 (也稱為叢集名稱物件或 CNO) ，請確定已選取 [**建立電腦物件**] 和 [**讀取所有屬性**] 許可權的 [**允許**]。

   ![顯示 [許可權專案] 對話方塊的螢幕擷取畫面，其中 [建立電腦物件] 選項設定為 [允許]。](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. 按一下 **[確定]** ，直到您回到 **Active Directory 消費者和電腦** 嵌入式管理單元為止。

11. 如果適用) 與建立電腦帳戶 (物件) 相關的 (，請參閱網域系統管理員諮詢網域原則。 確定叢集名稱帳戶可以在您每次設定叢集服務或應用程式時，建立電腦帳戶。 例如，如果您的網域系統管理員已設定為在特殊的容器中建立所有新的電腦帳戶，而不是預設的 **電腦** 容器，請確定這些設定允許叢集名稱帳戶也在該容器中建立新的電腦帳戶。

12. 展開 [預設 **電腦** ] 容器或其中一個叢集服務或應用程式的電腦帳戶所在的容器。

13. 在其中一個叢集服務或應用程式的電腦帳戶 **上按一下滑鼠** 右鍵，然後按一下 [內容]。

14. 在 [ **安全性** ] 索引標籤上，確認 [叢集名稱] 帳戶列在具有許可權的帳戶之間，然後選取它。 確認 [叢集名稱] 帳戶具有 [ **完全控制** ] 許可權 (已選取) 的 [ **允許** ] 核取方塊。 如果不是，請將叢集名稱帳戶新增至清單，並授與它「 **完全控制** 」許可權。

   ![[安全性] 索引標籤](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. 針對叢集中設定的每個叢集服務和應用程式，重複步驟13-14。

16. 檢查建立電腦物件的全網域配額 (預設值為 [10) ]， (諮詢網域系統管理員（如果適用）) 。 如果此程式中先前的專案都已完成審核並已修正，而且如果達到配額，請考慮增加配額。 若要變更配額：

   1.  以系統管理員身分開啟命令提示字元，並執行 **ADSIEdit**。

   2.  以滑鼠右鍵按一下 **ADSI 編輯器**，然後按一下 [ **連接到**]，再按一下 **[確定]**。 **預設的命名內容** 會新增至主控台樹。

   3.  按兩下 [ **預設命名內容**]，以滑鼠右鍵按一下其底下的網域物件，然後按一下 [ **屬性**]。

   4.  滾動至 **MachineAccountQuota**、選取它、按一下 [ **編輯**]、變更值，然後按一下 **[確定]**。
