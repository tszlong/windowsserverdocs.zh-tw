---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: 將同盟伺服器新增至同盟伺服器陣列
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 040caf6395b7c70313de900d522241f97699a999
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192498"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>將同盟伺服器新增至同盟伺服器陣列


安裝 Federation Service 角色服務後，當您在電腦上設定必要的憑證，您就能夠將電腦設定為同盟伺服器。 您可以使用下列程序，將電腦加入至新的同盟伺服器陣列。  
  
您可以將電腦加入 AD FS 同盟伺服器設定精靈的伺服器陣列。 當您使用此精靈將電腦加入至現有的伺服器陣列時，電腦都設定搭配讀取\-只有 AD FS 設定資料庫和它的複本必須接收來自主要同盟伺服器的更新。  
  
> [!NOTE]  
> 同盟網頁單一\-號\-上\(SSO\)設計中，您必須在帳戶夥伴組織中的至少一部同盟伺服器和資源夥伴組織中的至少一部同盟伺服器. 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>若要將同盟伺服器新增至同盟伺服器陣列  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   Federation Service 角色服務安裝完成之後，開啟 AD FS 管理嵌入式管理單元\-中，按一下  **AD FS 同盟伺服器設定精靈**上的連結**概觀**頁面或在**動作**窗格。  
  
    -   隨時安裝精靈完成，請開啟 Windows 檔案總管後，瀏覽至**c:\\Windows\\ADFS**資料夾，並按兩下\-按一下**FsConfigWizard.exe**.  
  
2.  在 [歡迎]  頁面上，確認已選取 [將同盟伺服器新增至現有的 Federation Service]  ，然後按一下 [下一步]  。  
  
3.  如果您已選取的 AD FS 資料庫的話**AD FS 組態資料庫偵測到現有的**頁面隨即出現。 如果出現該頁面，按一下 **[刪除資料庫]** ，然後按一下 **[下一步]** 。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用生產同盟伺服器陣列中，請選取此選項。  
  
4.  在 **[指定主要同盟伺服器及服務帳戶]** 頁面的 **[主要同盟伺服器名稱]** 底下，輸入伺服器陣列中的主要同盟伺服器電腦名稱，然後按一下 **[瀏覽]** 。 在 **[瀏覽]** 對話方塊中，找出其他所有同盟伺服器在現有的同盟伺服器陣列中當作服務帳戶使用的網域帳戶，然後按一下 **[確定]** 。 輸入密碼並加以確認，，然後按**下一步** :  
  
    > [!NOTE]  
    > 如需指定同盟伺服器陣列的服務帳戶的詳細資訊，請參閱[手動設定同盟伺服器陣列的 服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 同盟伺服器陣列中的每部同盟伺服器都必須指定相同的服務帳戶，陣列才能運作。 例如，如果已建立的服務帳戶為 contoso\\ADFS2SVC，您為同盟伺服器角色設定，並將會參與相同的伺服陣列的每一部電腦都必須指定 contoso\\ADFS2SVC 在此步驟同盟伺服器設定精靈的伺服器陣列才能運作。  
  
5.  在 [準備套用設定]  頁面上，檢閱詳細資料。 如果設定正確，請按一下**下一步** 若要開始設定 AD FS 使用這些設定。  
  
6.  在 [設定結果]  頁面上，檢閱結果。 當所有的組態步驟都完成後時，按一下**關閉**結束精靈。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

