---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: "新增至聯盟伺服器陣列聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>新增至聯盟伺服器陣列聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您安裝同盟服務的角色，並在電腦上設定所需的憑證之後，您就可以設定電腦成為聯盟伺服器。 若要將電腦加入新的聯盟伺服器陣列，您可以使用下列程序。  
  
您可以將電腦加入發電廠使用 AD FS 聯盟伺服器設定精靈。 當您將電腦加入現有發電廠使用這個精靈時，僅限 read\ 複本 AD FS 設定資料庫設定電腦，以及它必須從主要聯盟伺服器接收更新。  
  
> [!NOTE]  
> 聯盟網路 Single\-Sign\-On \(SSO\) 設計，您必須至少一個聯盟伺服器 account 合作夥伴組織和資源合作夥伴組織中的至少一個聯盟伺服器。 如需詳細資訊，請查看[放置聯盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>聯盟伺服器新增至聯盟伺服器陣列  
  
1.  有兩種方法可以開始 AD FS 聯盟伺服器設定精靈。 若要開始精靈中，執行下列其中一個動作：  
  
    -   同盟服務角色服務安裝完成後，開放 AD FS 管理 snap\ 中，按一下**AD FS 聯盟伺服器設定精靈**上的連結**概觀**頁面或**控制項**窗格。  
  
    -   依照本身需求加以安裝精靈完成，開放 Windows 檔案總管] 之後，瀏覽至**C:\\Windows\\ADFS**資料夾，然後 double\ 按**FsConfigWizard.exe**。  
  
2.  在**歡迎**頁面上，確認**聯盟伺服器加入現有的同盟服務**已選取，然後按一下 [**下一步**。  
  
3.  如果您已經選取 AD FS 資料庫存在，**現有 AD FS 設定資料庫偵測到**頁面隨即顯示。 發生這種情形，如果按一下**Delete 資料庫**，然後按一下 [**下**。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用正式作業聯盟伺服器陣列中，選取此選項。  
  
4.  在**指定主要聯盟伺服器與服務 Account**頁面上，在**主要聯盟伺服器名稱**農場，輸入主要聯盟伺服器的電腦名稱，然後按一下 [**瀏覽]**。 在**瀏覽]**對話方塊中，找出所使用的服務帳號為所有其他聯盟伺服器現有聯盟伺服器，核對，然後按一下 [ **[確定]**。 輸入密碼並確認，然後按一下**下一步**:  
  
    > [!NOTE]  
    > 用於指定聯盟伺服器陣列服務負責的相關詳細資訊，請查看[手動設定聯盟伺服器陣列服務 Account](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 聯盟伺服器陣列中的每個聯盟伺服器必須指定相同服務負責發電廠才能正常運作。 例如，如果建立服務 account contoso\\ADFS2SVC，聯盟伺服器角色您設定，並將參與相同發電廠每一部電腦必須 contoso\\ADFS2SVC 此步驟，精靈中指定聯盟伺服器設定陣列才能正常運作。  
  
5.  在**適用於設定準備**頁面上，檢視詳細資料。 若出現正確設定，請按一下**下一步**來設定 AD FS 使用這些設定。  
  
6.  在**設定結果**頁面上，檢視結果。 所有的設定步驟完成時，按**關閉**以結束精靈。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

