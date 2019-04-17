---
title: 預先設置 Active Directory 網域服務中的叢集電腦物件
description: 預先設置 Active Directory 網域服務中的叢集 computer 物件的方式。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081974"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>預先設置 Active Directory 網域服務中的叢集電腦物件

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主題顯示如何預先設置 Active Directory 網域服務 (AD DS) 中的叢集 computer 物件。 您可以使用此程序啟用使用者或群組時不需要在 AD DS 中建立電腦物件的權限建立的容錯移轉叢集。

當您建立的容錯移轉叢集使用 [建立叢集精靈或使用 Windows PowerShell 時，您必須指定叢集的名稱。 如果當您建立叢集有足夠的權限，叢集中建立程序會比對之叢集名稱的 AD DS 中自動建立電腦物件。 此物件會呼叫的*叢集名稱物件*或 CNO。 透過 CNO 虛擬電腦物件 (Vco) 自動建立時設定叢集使用用戶端存取點的角色。 例如，如果您建立名為*FileServer1*的用戶端存取點的高度可用的檔案伺服器，請 CNO 將 AD DS 中建立對應的 VCO。

>[!NOTE]
>在 Windows Server 2012 R2 中，有是建立 Active Directory 卸離的叢集，沒有 CNO 或 Vco AD DS 中建立所在的選項。 這被針對特定類型的叢集部署。 如需詳細資訊，請參閱 ＜ [Deploy Active Directory-Detached 叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>)。

若要自動建立 CNO，會建立容錯移轉叢集的使用者必須具有的組織單位 (OU) 或容器會形成叢集中的伺服器所在的**建立電腦物件**權限。 若要啟用使用者或群組而不需要此權限建立叢集，具有 AD DS （通常是網域系統管理員） 中的適當權限的使用者可以預先設置在 AD DS 中的 CNO。 這也提供網域系統管理員更多控制權用於叢集的命名慣例和控制項上的 OU 中建立叢集物件。

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>步驟 1： 預先設置在 AD DS 中 CNO

開始之前，請確定您知道下列：

- 您要指派之叢集名稱
- 使用者帳戶或您要授與建立叢集權限群組名稱

最佳作法是建議您建立叢集物件的 OU。 如果 OU 已經存在於您想要使用、 **Account Operators**群組成員資格，才才可完成此步驟。 如果您需要建立叢集物件的 OU，**以 Domain Admins**群組或同等群組成員資格，才才可完成此步驟。

>[!NOTE]
>如果您在預設的電腦容器，而不是 OU 中建立 CNO，您不具有完成本主題的步驟 3。 在此案例中，[叢集系統管理員可以建立最多 10 個 Vco 未含任何額外的設定。

### <a name="prestage-the-cno-in-ad-ds"></a>預先設置在 AD DS 中 CNO

1. 在已安裝遠端伺服器管理工具] 中，從 AD DS 工具的電腦上或在網域控制站上，開啟 [ **Active Directory 使用者及電腦**。 為達成此目的伺服器上，啟動伺服器管理員] 並再按一下 [**工具**] 功能表選取 [ **Active Directory 使用者及電腦**。
2. 若要建立叢集 OU computer 物件，以滑鼠右鍵按一下網域名稱或現有的 OU、 指向 [**新增**]，然後選取**組織單位**。 在 [**名稱**] 方塊中輸入 OU 的名稱，然後選取 **[確定]**。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下您要建立 CNO，並指向 [**新增**]，然後選取 [**電腦**的 OU。
4. 在 [**電腦名稱**] 方塊中輸入名稱，將用於容錯移轉叢集，然後選取 **[確定]**。

  >[!NOTE]
  >這是建立叢集的使用者會指定在建立叢集精靈中或做為值的**叢集管理存取點**] 頁面上的叢集名稱 *– Name* **新增叢集**Windows powershell 參數指令程式。

5. 最佳作法，以滑鼠右鍵按一下您剛建立的電腦帳戶、 選取 [**內容**]，然後選取 [**物件**] 索引標籤。在 [**物件**] 索引標籤上選取**遭到意外刪除的保護物件**] 核取方塊，然後再選取 **[確定]**。
6. 以滑鼠右鍵按一下您剛建立的電腦帳戶，然後選取 [**停用帳戶**。 選取 **[是]** 確認，並選取 **[確定]**。

  >[!NOTE]
  >您必須停用帳戶如此叢集建立期間叢集建立程序可確認帳戶目前不使用現有的電腦或網域中的叢集。

![在範例中的叢集 OU 的已停用的 CNO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**圖 1。 在範例中的叢集 OU 的已停用的 CNO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>步驟 2： 授與使用者建立叢集的權限

您必須設定權限讓會用來建立容錯移轉叢集的使用者帳戶具有完全控制權限給 CNO。

**Account Operators**群組成員資格，才需要有可完成此步驟。

以下是如何授與使用者建立叢集的權限：

1. 在 Active Directory 使用者及電腦] 上 [**檢視**] 功能表中，確定已選取 [**進階功能**。
2. 找出並 CNO 上按一下滑鼠右鍵，然後選取**屬性**。
3. 在 [**安全性**] 索引標籤上選取 [**新增]**。
4. 在 [**選取使用者、 電腦或群組**] 對話方塊中，指定的使用者帳戶或群組想要授與權限、，然後選取 **[確定]**。
5. 選取的使用者帳戶或剛新增的群組，然後選取 [**完全控制**] 旁的 [**允許**] 核取方塊。
  
  ![完全控制權授與使用者或群組，就會建立叢集](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **圖 2. 完全控制權授與使用者或群組，就會建立叢集**
6. 選取 **\[確定\]**。

完成此步驟之後，授與權限的使用者可以建立的容錯移轉叢集。 不過，如果 CNO 所在的 OU 中，使用者無法建立叢集的角色需要用戶端存取點直到完成步驟 3。

>[!NOTE]
>如果 CNO 位於預設的電腦容器、 叢集系統管理員可以建立最多 10 個 Vco 未含任何額外的設定。 若要新增超過 10 個 Vco，您必須明確授與**建立電腦物件**權限電腦容器 CNO。

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>步驟 3： CNO 權限授與 OU 或預先 Vco 設置叢集的角色

當您建立叢集的角色與用戶端存取點時，叢集中建立 VCO CNO 相同的 OU 中。 這會自動進行 CNO 必須建立之 ou 的 computer 物件的權限。

如果您 prestaged CNO AD DS 中的，您可以執行下列 cmdlet 以建立 Vco 任一動作：

- 做法 1：[授與 OU CNO 權限](#grant-the-cno-permissions-to-the-ou)。 如果您使用此選項，叢集中可以自動建立 Vco AD DS 中。 因此，容錯移轉叢集系統管理員可以不必要求您預先設置 Vco AD DS 中的建立叢集的角色。

>[!NOTE]
>**以 Domain Admins**群組或同等群組成員資格，才才可完成此選項的步驟。

- 選項 2： [Prestage VCO 叢集的角色](#prestage-a-vco-for-the-clustered-role)。 如果是因為您組織中的需求而預先設置叢集角色的帳戶所需，使用此選項。 例如，您可能會想要控制的命名慣例或哪些叢集的角色所建立的控制項。

>[!NOTE]
>**Account Operators**群組成員資格，才需要有可完成此選項的步驟。

### <a name="grant-the-cno-permissions-to-the-ou"></a>CNO 權限授與 OU

1. 在 Active Directory 使用者及電腦] 上 [**檢視**] 功能表中，確定已選取 [**進階功能**。
2. 以滑鼠右鍵按一下您建立 CNO 中的所在的 OU[步驟 1： 預先設置在 AD DS 中的 CNO](#step-1:-prestage-the-CNO-in-ad-ds)，然後選取 [**內容**。
3. 在 [**安全性**] 索引標籤上選取 [**進階**]。
4. [**進階安全性設定**] 對話方塊中，選取 [**新增]**。
5. **主體**、 旁，選取 [**選取主要名稱**。
6. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，選取 [**物件類型**]、 選取 [**電腦**] 核取方塊，，然後選取 **[確定]**。
7. [**輸入物件名稱來選取**，輸入 CNO 的名稱、 選取 [**檢查名稱**，然後選取 **[確定]**。 警告訊息，指出的回應您即將新增的已停用的物件，請選取 [**確定]**。
8. 在 [**權限項目**] 對話方塊中，確定 [**類型**] 清單設為 [**允許**] 和 [**套用至**] 清單設為**此物件及所有子系物件**。
9. 在**權限**] 下方選取 [**建立電腦物件**] 核取方塊。

  ![建立電腦物件權限授與 CNO](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **圖 3. 建立電腦物件權限授與 CNO**
10. 除非您會回到 [Active Directory 使用者及電腦嵌入式管理單元中選取 **[確定]** 。

容錯移轉叢集的系統管理員可以立即建立叢集的角色與用戶端存取點並將 online 的資源。

### <a name="prestage-a-vco-for-a-clustered-role"></a>預先設置 VCO 叢集的角色

1. 開始之前，請確定您知道叢集的名稱和叢集的角色具有的名稱。
2. 在 Active Directory 使用者及電腦] 上 [**檢視**] 功能表中，確定已選取 [**進階功能**。
3. Active Directory 使用者及電腦] 中的叢集 CNO 所在的 OU 上按一下滑鼠右鍵、 指向 [**新增**]，然後選取**電腦**。
4. 在 [**電腦名稱**] 方塊中輸入名稱，您將使用之叢集的角色，然後選取 **[確定]**。
5. 最佳作法，以滑鼠右鍵按一下您剛建立的電腦帳戶、 選取 [**內容**]，然後選取 [**物件**] 索引標籤。在 [**物件**] 索引標籤上選取**遭到意外刪除的保護物件**] 核取方塊，然後再選取 **[確定]**。
6. 以滑鼠右鍵按一下您剛建立的電腦帳戶，然後選取 [**屬性**。
7. 在 [**安全性**] 索引標籤上選取 [**新增]**。
8. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，選取 [**物件類型**]、 選取 [**電腦**] 核取方塊，，然後選取 **[確定]**。
9. [**輸入物件名稱來選取**，輸入 CNO 的名稱、 選取 [**檢查名稱**，然後選取 **[確定]**。 如果您收到警告訊息，告訴您即將新增的已停用的物件，請選取 **[確定]**。
10. 請確定已選取 [CNO，然後選取 [**完全控制**] 旁的 [**允許**] 核取方塊。
11. 選取 **\[確定\]**。

容錯移轉叢集的系統管理員現在可以建立叢集的角色與用戶端存取點符合預先設置的 VCO 名稱，並使資源上線。

## <a name="more-information"></a>更多資訊

- [容錯移轉叢集](failover-clustering.md)