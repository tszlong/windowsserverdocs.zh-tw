---
title: 預先設置叢集電腦物件，Active Directory 網域服務中
description: 如何預先設置叢集電腦物件，Active Directory 網域服務中。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869719"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>預先設置叢集電腦物件，Active Directory 網域服務中

>適用於：Windows Server 2012 R2 中，Windows Server 2012 中，Windows Server 2016

本主題說明如何在 Active Directory 網域服務 (AD DS) 中預先設定叢集電腦物件。 當使用者或群組沒有在 AD DS 中建立電腦物件的權限時，您可以使用這個程序讓他們建立容錯移轉叢集。

使用建立叢集精靈或使用 Windows PowerShell 建立容錯移轉叢集時，您必須指定叢集的名稱。 如果您建立叢集時具備充分的權限，叢集建立程序會自動在 AD DS 建立符合叢集名稱的電腦物件。 此物件稱為「叢集名稱物件」或 CNO。 透過 CNO，當您設定使用用戶端存取點的叢集角色時，會自動建立虛擬電腦物件 (VCO)。 例如，如果您建立一個包含名稱為 *FileServer1*用戶端存取點的高可用性檔案伺服器，CNO 會在 AD DS 建立對應的 VCO。

>[!NOTE]
>Windows Server 2012 r2 中沒有的選項來建立已中斷連結 Active Directory 的叢集，其中在 AD DS 中建立任何 CNO 或 Vco。 這是以特定的叢集部署類型為目標。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>)。

若要自動建立 CNO，建立容錯移轉叢集的使用者必須擁有組織單位 (OU) 或是將構成叢集的伺服器所在容器的 [建立電腦物件]  權限。 若要讓沒有此權限的使用者或群組能夠建立叢集，在 AD DS 中擁有適當權限的使用者 (通常是網域系統管理員) 可以在 AD DS 中預先設定 CNO。 這也讓網域系統管理員更能掌控叢集所使用的命名慣例，並控管建立叢集物件的 OU。

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>步驟 1：預先設定 CNO 中 AD DS

在開始之前，請確定您已知道：

- 您想要指派叢集名稱
- 使用者帳戶或您想要授與權限來建立叢集群組的名稱

我們建議最好的做法是為叢集物件建立一個 OU。 如果您要使用的 OU 已經存在，至少需要 **Account Operators** 群組的成員資格才能完成此步驟。 如果您需要為叢集物件建立 OU，則至少需要 **Domain Admins** 群組的成員資格或同等權限才能完成此步驟。

>[!NOTE]
>如果您是在預設電腦容器而不是 OU 中建立 CNO，不需要完成本主題的步驟 3。 在這個案例中，叢集系統管理員最多可以建立 10 個 VCO，不需任何額外的設定。

### <a name="prestage-the-cno-in-ad-ds"></a>預先設定 CNO 中 AD DS

1. 在從遠端伺服器管理工具安裝 AD DS 工具的電腦或在網域控制站，開啟 [Active Directory 使用者和電腦] 。 若要在伺服器上這樣做，請啟動 [伺服器管理員]，然後在**工具**功能表上，選取**Active Directory 使用者和電腦**。
2. 若要建立 OU 的叢集電腦物件，以滑鼠右鍵按一下網域名稱或現有的 OU，指向**的新**，然後選取**組織單位**。 在 **名稱**方塊中，輸入 OU 的名稱，然後選取**確定**。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要建立 CNO，的 OU 指向**的新**，然後選取**電腦**。
4. 在 **電腦名稱**方塊中，輸入名稱，將用於容錯移轉叢集，然後選取**確定**。

  >[!NOTE]
  >這是建立叢集的使用者將在建立叢集精靈中 [用於管理叢集的存取點]  頁面指定或做為 *–Name* Windows PowerShell Cmdlet 的 **New-Cluster** 參數值的叢集名稱。

5. 最佳做法，請以滑鼠右鍵按一下您剛才建立的電腦帳戶，請選取**屬性**，然後選取**物件**] 索引標籤。在 [**物件**索引標籤上，選取**保護物件以防止被意外刪除**核取方塊，然後按**確定**。
6. 以滑鼠右鍵按一下您剛才的電腦帳戶建立，然後按**停用的帳戶**。 選取  **是**以確認，然後選取**確定**。

  >[!NOTE]
  >您必須停用帳戶，這樣在叢集建立期間，叢集建立程序才能確認目前沒有現有的電腦或網域中的叢集使用該帳戶。

![範例叢集 OU 中停用的 CNO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**圖 1。範例叢集 OU 中停用的 CNO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>步驟 2：授與使用者權限，來建立叢集

您必須設定權限，讓將用來建立容錯移轉叢集的使用者帳戶具有 CNO 的完全控制權限。

若要完成此步驟，至少需要 **Account Operators** 群組的成員資格。

以下是如何授與使用者權限，來建立叢集：

1. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
2. 找出並接著以滑鼠右鍵按一下 CNO，然後再選取**屬性**。
3. 在 **安全性**索引標籤上，選取**新增**。
4. 在 **選取使用者、 電腦或群組**對話方塊方塊中，指定的使用者帳戶或您想要授與權限，然後選取的群組**確定**。
5. 選取您剛才新增的使用者帳戶或群組，然後選取 [完全控制] 旁邊的 [允許]  核取方塊。
  
  ![將完全控制授與將要建立叢集的使用者或群組](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **圖 2。完整控制權授與使用者或群組，以建立叢集**
6. 選取 [確定]。

完成此步驟之後，獲得您授與權限的使用者就可以建立容錯移轉叢集。 不過，如果 CNO 位於 OU，必須等到您完成步驟 3，使用者才能建立需要用戶端存取點的叢集角色。

>[!NOTE]
>如果 CNO 位於預設電腦容器，叢集系統管理員最多可以建立 10 個 VCO，不需任何額外的設定。 若要新增 10 個以上的 VCO，您必須將 [建立電腦物件] 權限明確地授與電腦容器的 CNO。

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>步驟 3：CNO 權限授與 OU 或預先設定 Vco，叢集角色

當您建立含有用戶端存取點的叢集角色時，叢集會在與 CNO 相同的 OU 中建立 VCO。 若要自動執行此動作，CNO 必須在 OU 中擁有建立電腦物件的權限。

如果您在 AD DS 中預先設定 CNO，可以執行下列任一項以建立 VCO：

- 選項 1：[CNO 權限授與 OU](#grant-the-cno-permissions-to-the-ou)。 如果使用此選項，叢集可以在 AD DS 自動建立 VCO。 因此，容錯移轉叢集的系統管理員不必要求您在 AD DS 預先設定 VCO，就可以建立叢集角色。

>[!NOTE]
>若要完成此選項的步驟，至少需要 **Domain Admins** 群組的成員資格或同等權限。

- 選項 2：[預先設置叢集角色的 VCO](#prestage-a-vco-for-the-clustered-role)。 如果因為組織的需求而必須預先設定叢集角色的帳戶，請使用此選項。 例如，您可能需要控制命名慣例，或控制要建立哪些叢集角色。

>[!NOTE]
>若要完成此選項的步驟，至少需要 **Account Operators** 群組的成員資格。

### <a name="grant-the-cno-permissions-to-the-ou"></a>CNO 權限授與 OU

1. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
2. 以滑鼠右鍵按一下 建立中的 CNO 的 OU[步驟 1:預先設定 CNO 中 AD DS](#step-1:-prestage-the-CNO-in-ad-ds)，然後選取**屬性**。
3. 在 **安全性**索引標籤上，選取**進階**。
4. 在 **進階安全性設定**對話方塊中，選取**新增**。
5. 旁**主體**，選取**選取一個主體**。
6. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，選取**物件型別**，選取**電腦**核取方塊，然後按 **[確定]**.
7. 底下**輸入要選取的物件名稱**，輸入 CNO 的名稱，選取**檢查名稱**，然後選取**確定**。 為了回應指出您將要新增已停用的物件的警告訊息，請選取**確定**。
8. 在 [權限項目] 對話方塊中，確定 [類型] 清單設定為 [允許]，且 [套用到] 清單設定為 [此物件及所有子系物件]。
9. 在 [權限] 底下，選取 [建立電腦物件]  核取方塊。

  ![將建立電腦物件權限授與 CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **圖 3。建立電腦物件權限授與 CNO**
10. 選取 **確定**直到您返回 Active Directory 使用者和電腦 嵌入式管理單元。

容錯移轉叢集的系統管理員現在可以建立含有用戶端存取點的叢集角色，並將資源上線。

### <a name="prestage-a-vco-for-a-clustered-role"></a>預先設置叢集角色的 VCO

1. 開始之前，請先確定您知道叢集的名稱以及叢集角色將擁有的名稱。
2. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
3. 在 Active Directory 使用者和電腦，以滑鼠右鍵按一下 OU CNO 所在的叢集，指向**的新**，然後選取**電腦**。
4. 在 **電腦名稱**方塊中，輸入名稱，您將用於叢集的角色，然後選取**確定**。
5. 最佳做法，請以滑鼠右鍵按一下您剛才建立的電腦帳戶，請選取**屬性**，然後選取**物件**] 索引標籤。在 [**物件**索引標籤上，選取**保護物件以防止被意外刪除**核取方塊，然後按**確定**。
6. 以滑鼠右鍵按一下您剛才的電腦帳戶建立，然後按**屬性**。
7. 在 **安全性**索引標籤上，選取**新增**。
8. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，選取**物件型別**，選取**電腦**核取方塊，然後按 **[確定]**.
9. 底下**輸入要選取的物件名稱**，輸入 CNO 的名稱，選取**檢查名稱**，然後選取**確定**。 如果您收到警告訊息，指出您將要新增已停用的物件，選取將要**確定**。
10. 確定已選取 CNO，然後選取 [完全控制] 旁邊的 [允許] 核取方塊。
11. 選取 [確定]。

容錯移轉叢集的系統管理員現在可以建立含有符合預先設定 VCO 名稱的用戶端存取點的叢集角色，並將資源上線。

## <a name="more-information"></a>詳細資訊

- [容錯移轉叢集](failover-clustering.md)