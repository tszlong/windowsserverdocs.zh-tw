---
title: 在 Active Directory Domain Services 中預先設置叢集電腦物件
description: 如何在 Active Directory Domain Services 中預先設置叢集電腦物件。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.openlocfilehash: b14561a05778ed30e71363a2cd3b3b6fdf24f78e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827471"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>在 Active Directory Domain Services 中預先設置叢集電腦物件

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明如何在 Active Directory 網域服務 (AD DS) 中預先設定叢集電腦物件。 當使用者或群組沒有在 AD DS 中建立電腦物件的權限時，您可以使用這個程序讓他們建立容錯移轉叢集。

使用建立叢集精靈或使用 Windows PowerShell 建立容錯移轉叢集時，您必須指定叢集的名稱。 如果您建立叢集時具備充分的權限，叢集建立程序會自動在 AD DS 建立符合叢集名稱的電腦物件。 此物件稱為「叢集名稱物件」或 CNO。 透過 CNO，當您設定使用用戶端存取點的叢集角色時，會自動建立虛擬電腦物件 (VCO)。 例如，如果您建立一個包含名稱為 *FileServer1*用戶端存取點的高可用性檔案伺服器，CNO 會在 AD DS 建立對應的 VCO。

>[!NOTE]
>有選項可以建立 Active Directory 卸離的叢集，在 AD DS 中不會建立任何 CNO 或 Vco。 這是以特定的叢集部署類型為目標。 如需詳細資訊，請參閱[部署已中斷連結 Active Directory 的叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>)。

若要自動建立 CNO，建立容錯移轉叢集的使用者必須擁有組織單位 (OU) 或是將構成叢集的伺服器所在容器的 [建立電腦物件] 權限。 若要讓沒有此權限的使用者或群組能夠建立叢集，在 AD DS 中擁有適當權限的使用者 (通常是網域系統管理員) 可以在 AD DS 中預先設定 CNO。 這也讓網域系統管理員更能掌控叢集所使用的命名慣例，並控管建立叢集物件的 OU。

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>步驟 1：在 AD DS 中預先設定 CNO

在開始之前，請確定您已知道：

- 您想要指派叢集的名稱
- 您要授與許可權以建立叢集的使用者帳戶或組名

我們建議最好的做法是為叢集物件建立一個 OU。 如果您要使用的 OU 已經存在，至少需要 **Account Operators** 群組的成員資格才能完成此步驟。 如果您需要為叢集物件建立 OU，則至少需要 **Domain Admins** 群組的成員資格或同等權限才能完成此步驟。

>[!NOTE]
>如果您是在預設電腦容器而不是 OU 中建立 CNO，不需要完成本主題的步驟 3。 在這個案例中，叢集系統管理員最多可以建立 10 個 VCO，不需任何額外的設定。

### <a name="prestage-the-cno-in-ad-ds"></a>在 AD DS 中預先設置 CNO

1. 在從遠端伺服器管理工具安裝 AD DS 工具的電腦或在網域控制站，開啟 [Active Directory 使用者和電腦]。 若要在伺服器上執行此動作，請啟動伺服器管理員，然後在 [**工具**] 功能表上，選取 [ **Active Directory 使用者和電腦**]。
2. 若要建立叢集電腦物件的 OU，請以滑鼠右鍵按一下功能變數名稱或現有的 OU，指向 [**新增**]，然後選取 [**組織單位**]。 在 [**名稱**] 方塊中，輸入 OU 的名稱，然後選取 **[確定]** 。
3. 在主控台樹中，以滑鼠右鍵按一下您要建立 CNO 的 OU，指向 [**新增**]，然後選取 [**電腦**]。
4. 在 [**電腦名稱稱**] 方塊中，輸入將用於容錯移轉叢集的名稱，然後選取 **[確定]** 。

   >[!NOTE]
   >這是建立叢集的使用者將在建立叢集精靈中 [用於管理叢集的存取點] 頁面指定或做為 *–Name* Windows PowerShell Cmdlet 的 **New-Cluster** 參數值的叢集名稱。

5. 最佳做法是以滑鼠右鍵按一下剛才建立的電腦帳戶，選取 [**屬性**]，然後選取 [**物件**] 索引標籤。在 [**物件**] 索引標籤上，選取 [**保護物件不被意外刪除**] 核取方塊，然後選取 **[確定]** 。
6. 在您剛才建立的電腦帳戶上按一下滑鼠右鍵，然後選取 [**停用帳戶**]。 選取 **[是]** 進行確認，然後選取 **[確定]** 。

   >[!NOTE]
   >您必須停用帳戶，這樣在叢集建立期間，叢集建立程序才能確認目前沒有現有的電腦或網域中的叢集使用該帳戶。

![範例叢集 OU 中停用的 CNO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**[圖 1]範例叢集 OU 中已停用的 CNO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>步驟 2：授與使用者建立叢集的權限

您必須設定權限，讓將用來建立容錯移轉叢集的使用者帳戶具有 CNO 的完全控制權限。

若要完成此步驟，至少需要 **Account Operators** 群組的成員資格。

以下說明如何授與使用者建立叢集的許可權：

1. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
2. 找出並以滑鼠右鍵按一下 CNO，然後選取 [**屬性**]。
3. 在 [**安全性**] 索引標籤上，選取 [**新增**]。
4. 在 [**選取使用者、電腦或群組**] 對話方塊中，指定您想要授與許可權的使用者帳戶或群組，然後選取 **[確定]** 。
5. 選取您剛才新增的使用者帳戶或群組，然後選取 [完全控制] 旁邊的 [允許] 核取方塊。
  
   ![將完全控制授與將要建立叢集的使用者或群組](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
   **[圖 2]將完全控制授與將建立叢集的使用者或群組**
6. 選取 **[確定]** 。

完成此步驟之後，獲得您授與權限的使用者就可以建立容錯移轉叢集。 不過，如果 CNO 位於 OU，必須等到您完成步驟 3，使用者才能建立需要用戶端存取點的叢集角色。

>[!NOTE]
>如果 CNO 位於預設電腦容器，叢集系統管理員最多可以建立 10 個 VCO，不需任何額外的設定。 若要新增 10 個以上的 VCO，您必須將 [建立電腦物件] 權限明確地授與電腦容器的 CNO。

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>步驟 3：將 CNO 權限授與 OU 或預先設定叢集角色的 VCO

當您建立含有用戶端存取點的叢集角色時，叢集會在與 CNO 相同的 OU 中建立 VCO。 若要自動執行此動作，CNO 必須在 OU 中擁有建立電腦物件的權限。

如果您在 AD DS 中預先設定 CNO，可以執行下列任一項以建立 VCO：

- 選項 1：[將 CNO 權限授與 OU](#grant-the-cno-permissions-to-the-ou)。 如果使用此選項，叢集可以在 AD DS 自動建立 VCO。 因此，容錯移轉叢集的系統管理員不必要求您在 AD DS 預先設定 VCO，就可以建立叢集角色。

>[!NOTE]
>若要完成此選項的步驟，至少需要 **Domain Admins** 群組的成員資格或同等權限。

- 選項2：[為叢集角色](#prestage-a-vco-for-a-clustered-role)預先設置 VCO。 如果因為組織的需求而必須預先設定叢集角色的帳戶，請使用此選項。 例如，您可能需要控制命名慣例，或控制要建立哪些叢集角色。

>[!NOTE]
>若要完成此選項的步驟，至少需要 **Account Operators** 群組的成員資格。

### <a name="grant-the-cno-permissions-to-the-ou"></a>將 CNO 許可權授與 OU

1. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
2. 以滑鼠右鍵按一下您在[步驟1：在 AD DS](#step-1-prestage-the-cno-in-ad-ds)中預先設置 CNO 中建立 CNO 的 OU，然後選取 [**屬性**]。
3. 在 [**安全性**] 索引標籤上，選取 [ **Advanced**]。
4. 在 [**高級安全性設定**] 對話方塊中，選取 [**新增**]。
5. 在 [**主體**] 旁，選取 [**選取主體**]。
6. 在 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊中，選取 [**物件類型**]，選取 [**電腦**] 核取方塊，然後選取 **[確定]** 。
7. 在 [**輸入物件名稱來選取**] 下，輸入 CNO 的名稱，選取 [**檢查名稱**]，然後選取 **[確定]** 。 若要回應指出您即將新增已停用物件的警告訊息，請選取 **[確定]** 。
8. 在 [權限項目] 對話方塊中，確定 [類型] 清單設定為 [允許]，且 [套用到] 清單設定為 [此物件及所有子系物件]。
9. 在 [權限] 底下，選取 [建立電腦物件] 核取方塊。

   ![將建立電腦物件權限授與 CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

   **[圖 3]將 [建立電腦物件] 許可權授與 CNO**
10. 選取 [**確定]** ，直到返回 [Active Directory 使用者和電腦] 嵌入式管理單元為止。

容錯移轉叢集的系統管理員現在可以建立含有用戶端存取點的叢集角色，並將資源上線。

### <a name="prestage-a-vco-for-a-clustered-role"></a>為叢集角色預先設置 VCO

1. 開始之前，請先確定您知道叢集的名稱以及叢集角色將擁有的名稱。
2. 在 [Active Directory 使用者和電腦] 的 [檢視] 功能表上，確定已選取 [進階功能]。
3. 在 Active Directory 使用者和電腦 中，以滑鼠右鍵按一下叢集的 CNO 所在的 OU，指向 **新增**，然後選取 **電腦**。
4. 在 [**電腦名稱稱**] 方塊中，輸入您將用於叢集角色的名稱，然後選取 **[確定]** 。
5. 最佳做法是以滑鼠右鍵按一下剛才建立的電腦帳戶，選取 [**屬性**]，然後選取 [**物件**] 索引標籤。在 [**物件**] 索引標籤上，選取 [**保護物件不被意外刪除**] 核取方塊，然後選取 **[確定]** 。
6. 在您剛才建立的電腦帳戶上按一下滑鼠右鍵，然後選取 [**屬性**]。
7. 在 [**安全性**] 索引標籤上，選取 [**新增**]。
8. 在 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊中，選取 [**物件類型**]，選取 [**電腦**] 核取方塊，然後選取 **[確定]** 。
9. 在 [**輸入物件名稱來選取**] 下，輸入 CNO 的名稱，選取 [**檢查名稱**]，然後選取 **[確定]** 。 如果您收到一則警告訊息，指出您即將新增已停用的物件，請選取 **[確定]** 。
10. 確定已選取 CNO，然後選取 [完全控制] 旁邊的 [允許] 核取方塊。
11. 選取 **[確定]** 。

容錯移轉叢集的系統管理員現在可以建立含有符合預先設定 VCO 名稱的用戶端存取點的叢集角色，並將資源上線。

## <a name="more-information"></a>詳細資訊

- [容錯移轉叢集](failover-clustering.md)
- [在 Active Directory 中設定叢集帳戶](configure-ad-accounts.md)