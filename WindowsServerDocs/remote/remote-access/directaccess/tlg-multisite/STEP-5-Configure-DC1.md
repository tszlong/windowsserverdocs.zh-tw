---
title: 步驟 5 設定 DC1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7f907c3bf463e3a90d413e5b167a70051057f06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876769"
---
# <a name="step-5-configure-dc1"></a>步驟 5 設定 DC1

>適用於：Windows Server （半年通道），Windows Server 2016

DC1 會做為網域控制站、 DNS 伺服器和 corp.contoso.com 網域的 DHCP 伺服器。  
  
若要設定遠端存取權，請使用多站台的拓撲，就必須新增額外的 Active Directory 網域服務 (AD DS) 站台的第二個網域控制站 2-DC1，以及設定子網路之間路由。  
  
1. 若要設定網域控制站上的預設閘道。 在 DC1 上設定的預設閘道。  
  
2. 在 DC1 上的 Windows 7 DirectAccess 用戶端建立安全性群組。 設定 DirectAccess 時，它會自動建立群組原則物件 (Gpo) 和 GPO 設定，會套用至 DirectAccess 用戶端和伺服器。 DirectAccess 用戶端 GPO 會套用至特定的 Active Directory 安全性群組。  
  
3. 若要加入新的 AD DS 站台。 建立第二個的 AD DS 站台。  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>若要設定網域控制站上的預設閘道  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步**有線乙太網路連線**，按一下連結。  
  
2.  在 [網路連線] 視窗中，以滑鼠右鍵按一下**有線乙太網路連線**，然後按一下**屬性**。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
4.  在**預設閘道**，型別**10.0.0.254**，然後在**備用 DNS 伺服器**，型別**10.2.0.1**，然後按一下  **確定**.  
  
5.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]**，然後按一下 **[屬性]**。  
  
6.  在**預設閘道**，型別**2001:db8:1::fe**，然後在**備用 DNS 伺服器**，型別**2001:db8:2::1**，然後按一下 **[確定]**。  
  
7.  在 [**有線乙太網路連線內容**] 對話方塊中，按一下**關閉**。  
  
8.  關閉 [網路連線]  視窗。  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>在 DC1 上的 Windows 7 DirectAccess 用戶端建立安全性群組  
使用下列程序中建立適用於 Windows 7 DirectAccess 的安全性群組。  
  
 Windows 7 用戶端電腦必須是個別的安全性群組的成員，因為他們就能夠連線到內部資源，透過單一進入點。 當啟用多站台的支援，或新增項目點，如果 Windows 7 支援要求時，然後個別的 GPO 將會自動建立每個進入點的 Windows 7 的 DirectAccess 用戶端。  
  
### <a name="create-security-groups"></a>建立安全性群組  
  
1.  在 **開始**畫面上，輸入**dsa.msc**，然後按 ENTER 鍵。  
  
2.  在左窗格中，依序展開**corp.contoso.com**，按一下**使用者**，然後以滑鼠右鍵按一下**使用者**，指向**新增**，然後按一下**群組**。  
  
3.  在 **新增物件-群組**對話方塊的 **群組名稱**，輸入**Win7_Clients_Site1**。  
  
4.  在 [群組領域] 之下按一下 [全域]，在 [群組類型] 之下按一下 [安全性]，然後按一下 [確定]。  
  
5.  按兩下**Win7_Clients_Site1**安全性群組，然後在**Win7_Clients_Site1 屬性** 對話方塊中，按一下 **成員** 索引標籤。  
  
6.  在 [成員]  索引標籤上，按一下 [新增] 。  
  
7.  在 [**選取使用者、 連絡人、 電腦或服務帳戶**] 對話方塊中，按一下**物件類型**。 在上**物件類型**對話方塊中，選取**電腦**，然後按一下 **確定**。  
  
8.  在**輸入要選取的物件名稱**，型別**client2**，然後按一下 **[確定]**，然後在**Win7_Clients_Site1 屬性**對話方塊中，按一下**確定**。  
  
9. 在  **Active Directory 使用者和電腦**主控台中的，在左窗格中，以滑鼠右鍵按一下**使用者**，指向**新增**，然後按一下**群組**.  
  
10. 在 **新增物件-群組**對話方塊的 **群組名稱**，輸入**Win7_Clients_Site2**。  
  
11. 在 [群組領域] 之下按一下 [全域]，在 [群組類型] 之下按一下 [安全性]，然後按一下 [確定]。  
  
12. 關閉 [Active Directory 使用者和電腦]  主控台。  
  
## <a name="to-add-a-new-ad-ds-site"></a>若要新增新的 AD DS 站台  
  
1.  在 **開始**畫面上，輸入**dssite.msc**，然後按 ENTER 鍵。  
  
2.  在 Active Directory 站台和服務主控台中，在主控台樹狀目錄中，以滑鼠右鍵按一下**站台**，然後按一下**新的站台**。  
  
3.  在 **新物件-站台**對話方塊中，於**名稱**方塊中，輸入**第二個站台**。  
  
4.  在清單方塊中，按一下**DEFAULTIPSITELINK**，然後按一下**確定**兩次。  
  
5.  在主控台樹狀目錄中，依序展開**站台**，以滑鼠右鍵按一下**子網路**，然後按一下**新的子網路**。  
  
6.  在上**新增物件-子網路**對話方塊的 **前置詞**，型別**10.0.0.0/24**中**選取此首碼的站台物件**清單中，按一下**預設第一個站台名稱**，然後按一下**確定**。  
  
7.  在主控台樹狀目錄中，以滑鼠右鍵按一下**子網路**，然後按一下**新的子網路**。  
  
8.  在上**新增物件-子網路**對話方塊的 **前置詞**，型別**2001:db8:1:: / 64**中**選取此首碼的站台物件** 清單中，按一下 **預設第一個站台名稱**，然後按一下**確定**。  
  
9. 在主控台樹狀目錄中，以滑鼠右鍵按一下**子網路**，然後按一下**新的子網路**。  
  
10. 在上**新增物件-子網路**對話方塊的 **前置詞**，型別**10.2.0.0/24**中**選取此首碼的站台物件**清單中，按一下**第二個站台**，然後按一下**確定**。  
  
11. 在主控台樹狀目錄中，以滑鼠右鍵按一下**子網路**，然後按一下**新的子網路**。  
  
12. 在上**新增物件-子網路**對話方塊的 **前置詞**，型別**2001:db8:2:: / 64**中**選取此首碼的站台物件** 清單中，按一下 **第二個站台**，然後按一下**確定**。  
  
13. 關閉 Active Directory 站台及服務。  
  


