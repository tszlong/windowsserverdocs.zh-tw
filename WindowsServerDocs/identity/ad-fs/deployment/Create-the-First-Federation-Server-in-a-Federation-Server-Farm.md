---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: 在同盟伺服器陣列中建立第一部同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e16289142ea2e53adba52a4ed8f6c01a929a530d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192217"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器

安裝 Federation Service 角色服務後，當您在電腦上設定必要的憑證，您就能夠將電腦設定為同盟伺服器。 若要將電腦要成為新的同盟伺服器陣列，使用 AD FS 同盟伺服器設定精靈的第一個同盟伺服器設定，您可以使用下列程序。  
  
在陣列中建立第一部同盟伺服器也會建立新的 Federation Service 並使此電腦成為主要同盟伺服器。 這表示這台電腦就會以讀取設定\/寫入 AD FS 設定資料庫的複本。 此伺服器陣列中的所有其他同盟伺服器必須複寫到其讀取主要同盟伺服器進行任何變更\-份其本機儲存的 AD FS 設定資料庫。 如需有關此複寫程序的詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 同盟網頁單一\-號\-上\(SSO\)設計中，您必須在帳戶夥伴組織中的至少一部同盟伺服器和資源夥伴組織中的至少一部同盟伺服器. 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要 Domain Admins 的成員資格或已被授與寫入 Active Directory 中「程式資料」容器之權限的受委派網域帳戶。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   Federation Service 角色服務安裝完成之後，開啟 AD FS 管理嵌入式管理單元\-中，按一下  **AD FS 同盟伺服器設定精靈**上的連結**概觀**頁面或在**動作**窗格。  
  
    -   安裝精靈已完成，請開啟 Windows 檔案總管中之後, 隨時瀏覽至**c:\\Windows\\ADFS**資料夾，，然後按兩下\-按一下**FsConfigWizard.exe**.  
  
2.  在 [歡迎]  頁面上，確認已選取 [建立新的 Federation Service]  ，然後按一下 [下一步]  。  
  
3.  在上**選取，就能\-Alone 或伺服陣列部署**頁面上，按一下**新同盟伺服器陣列**，然後按一下**下一步** 。  
  
4.  在 [指定 Federation Service 名稱]  頁面上，確認顯示的 [SSL 憑證]  是正確的。 若這不是正確的憑證，請從 [SSL 憑證]  清單中選取適當憑證。  
  
    此憑證會產生從 Secure Sockets Layer \(SSL\)在預設的網站的設定。 若「預設的網站」只設定一個 SSL 憑證，則會出示該憑證並自動選取該憑證以供使用。 若已為「預設的網站」設定多個 SSL 憑證，則此處會列出所有那些憑證，而且您必須從中選取一個憑證。 若沒有為「預設的網站」設定 SSL 設定，則會從本機電腦上個人憑證存放區中可用的憑證產生清單。  
  
    > [!NOTE]  
    > 若已為 IIS 設定 SSL 憑證，精靈將不會允許您覆寫該憑證。 這樣可確保先前為 IIS 設定的 SSL 憑證都會被保留。 為因應此限制，您可以移除該憑證或使用 [IIS 管理主控台] 手動重新設定憑證。  
  
5.  如果您已選取的 AD FS 資料庫的話**AD FS 組態資料庫偵測到現有的**頁面隨即出現。 若出現該頁面，請按一下 [刪除資料庫]  ，然後按一下 [下一步]  。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用生產同盟伺服器陣列中，請選取此選項。  
  
6.  在 [指定服務帳戶]  頁面上，按一下 [瀏覽]  。 在 [瀏覽]  對話方塊中，尋找將做為這個新的同盟伺服器陣列之服務帳戶使用的網域帳戶，然後按一下 [確定]  。 輸入此帳戶的密碼並再次確認，然後按一下 [下一步]  。  
  
    > [!NOTE]  
    > 請參閱[手動設定同盟伺服器陣列的 服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)如需指定同盟伺服器陣列的服務帳戶的詳細資訊。 同盟伺服器陣列中的每部同盟伺服器都必須指定相同的服務帳戶，陣列才能運作。 例如，如果已建立的服務帳戶為 contoso\\ADFS2SVC，您為同盟伺服器角色設定，將會參與相同的伺服陣列的每一部電腦都必須指定 contoso\\ADFS2SVC 在此步驟同盟伺服器設定精靈的伺服器陣列才能運作。  
  
7.  在 [準備套用設定]  頁面上，檢閱詳細資料。 如果設定正確，請按一下**下一步** 若要開始設定 AD FS 使用這些設定。  
  
8.  在 [設定結果]  頁面上，檢閱結果。 當所有的組態步驟都完成後時，按一下**關閉**結束精靈。  
  
    > [!IMPORTANT]  
    > 基於安全的部署目的，當您使用 [AD FS 同盟伺服器設定精靈] 來設定同盟伺服器陣列時，會停用成品解析與回覆偵測。 此精靈會自動設定「Windows 內部資料庫」來儲存服務設定資料。 您可能會不過，錯誤地復原這項變更藉由使用 [成品解析] 端點**端點**AD FS 管理嵌入式管理單元中的節點\-中，或啟用\-ADFSEndpoint cmdletWindows PowerShell。 請小心，不要重新設定預設設定，這樣當您使用同盟伺服器陣列搭配「Windows 內部資料庫」時，此端點才能維持停用狀態。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

