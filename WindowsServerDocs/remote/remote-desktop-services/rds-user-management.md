---
title: 管理 RDS 集合中的使用者
description: 了解如何管理遠端桌面服務中的使用者。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: ff782bc4d01709f56d19ee3e9a06a95267cf7a12
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870745"
---
# <a name="manage-users-in-your-rds-collection"></a>管理 RDS 集合中的使用者

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

身為系統管理員，您可以直接管理哪些使用者有權存取特定的集合。 如此一來，您就能建立一個集合來為資訊工作者提供標準應用程式，但接著要建立不同的集合來為工程師提供大量圖形的模型化應用程式。 有兩個主要步驟可用來管理遠端桌面服務 (RDS) 部署中的使用者存取：

1.  [在 Active Directory 中建立使用者和群組](#create-your-users-and-groups-in-active-directory)
2.  [將使用者和群組指派給集合](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>在 Active Directory 中建立使用者和群組

在 RDS 部署中，Active Directory 網域服務 (AD DS) 是網域中所有使用者、群組和其他物件的來源。 您可以使用 PowerShell 直接管理 Active Directory，或者使用內建的 UI 工具來增加簡易性與彈性。 下列步驟將引導您安裝那些工具 (如果您尚未安裝它們)，然後使用那些工具來管理使用者和群組。

### <a name="install-ad-ds-tools"></a>安裝 AD DS 工具

下列步驟將詳細說明如何在已經執行 AD DS 的伺服器上安裝 AD DS 工具。 安裝之後，您接著可以建立使用者或建立群組。

1. 連線至執行 Active Directory 網域服務的伺服器。 針對 Azure 部署：
   1. 在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  ，然後按一下部署的資源群組。
   2. 選取 AD 虛擬機器。
   3. 按一下 [連線] > [開啟]  以開啟遠端桌面用戶端。 如果 [連線]  呈現灰色，則虛擬機器可能沒有公用 IP 位址。 若要為它提供一個，請執行下列步驟，然後重試此步驟。
      1. 按一下 [設定] > [網路介面]  ，然後按一下對應的網路介面。
      2. 按一下 [設定] > [IP 位址]  。
      3. 針對 [公用 IP 位址]  選取 [已啟用]  ，然後按一下 [IP 位址]  。
      4. 如果您目前有想要使用的公用 IP 位址，請從清單中選取它。 否則，按一下 [新建]  、輸入名稱，然後依序按一下 [確定]  和 [儲存]  。
   4. 在用戶端，按一下 [連線]  ，然後按一下 [使用其他帳戶]  。 輸入網域系統管理員帳戶的使用者名稱和密碼。
   5. 當系統詢問憑證相關資訊時，按一下 [是]  。
2. 安裝 AD DS 工具：
   1. 在 [伺服器管理員] 中，按一下 [管理] > [新增角色及功能]  。
   2. 按一下 [角色型或功能型安裝]  ，然後按一下目前的 AD 伺服器。 遵循步驟，直到您到達 [功能]  索引標籤為止。
   3. 展開 [遠端伺服器管理工具] > [角色管理工具] > [AD DS 及 AD LDS 工具]  ，然後選取 [AD DS 工具]  。
   4. 選取 [必要時自動重新啟動目的地伺服器]  ，然後按一下 [安裝]  。

### <a name="create-a-group"></a>建立群組

您可以使用 AD DS 群組來為一組需要使用相同遠端資源的使用者授與存取。

1. 在執行 AD DS 之伺服器上的 [伺服器管理員] 中，按一下 [工具] > [Active Directory 使用者和電腦]  。
2. 展開左側窗格中的網域，以檢視它的子資料夾。
3. 以滑鼠右鍵按一下您想要建立群組的資料夾，然後按一下 [新增] > [群組]  。
4. 輸入適當的群組名稱，然後依序選取 [全域]  和 [安全性]  。

### <a name="create-a-user-and-add-to-a-group"></a>建立使用者並新增至群組
1. 在執行 AD DS 之伺服器上的 [伺服器管理員] 中，按一下 [工具] > [Active Directory 使用者和電腦]  。
2. 展開左側窗格中的網域，以檢視它的子資料夾。
3. 以滑鼠右鍵按一下 [使用者]  ，然後按一下 [新增] > [使用者]  。
4. 至少需輸入名字和使用者登入名稱。
5. 輸入並確認使用者的密碼。 設定適當的使用者選項，例如 [使用者必須在下次登入時變更密碼]  。
6. 將新的使用者新增至群組：
   1. 在 [使用者]  資料夾中，以滑鼠右鍵按一下新的使用者。
   2. 按一下 [加入群組]  。
   3. 輸入您要將使用者新增至其中的群組名稱。

## <a name="assign-users-and-groups-to-collections"></a>將使用者和群組指派給集合
既然您已在 Active Directory 中建立了使用者和群組，您可以新增一些有關您部署中有權存取遠端桌面集合之人員的細微性。

1. 遵循稍早所述的步驟，連線至執行遠端桌面連線代理人 (RD 連線代理人) 角色的伺服器。
2. 將其他遠端桌面伺服器新增至 RD 連線代理人的受控伺服器集區：
   1. 在 [伺服器管理員] 中，按一下 [管理] > [新增伺服器]  。
   2. 按一下 [立即尋找]  。
   3. 按一下部署中正在執行遠端桌面服務角色的每一部伺服器，然後按一下 [確定]  。
3. 編輯集合以便為特定使用者或群組指派存取：
   1. 在 [伺服器管理員] 中，按一下 [遠端桌面服務] > [概觀]  ，然後按一下特定的集合。
   2. 在 [屬性]  下方，按一下 [工作] > [編輯屬性]  。
   3. 按一下 [使用者群組]  。
   4. 按一下 [新增]  ，然後輸入您想要使其有權存取集合的使用者或群組。 您也可以從這個視窗中選取要移除的使用者或群組，然後按一下 [移除]  來移除使用者或群組。 
   
   >[!NOTE] 
   > [使用者群組] 視窗絕對不能是空的。 若要縮小有權存取集合的使用者範圍，您必須先新增特定的使用者或群組，然後再移除較大範圍的群組。