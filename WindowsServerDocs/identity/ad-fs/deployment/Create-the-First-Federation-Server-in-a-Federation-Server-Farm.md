---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: 在同盟伺服器陣列中建立第一部同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0fe5c3160f661357536ef3bd60762873063c8ed0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855471"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器

安裝同盟服務角色服務並在電腦上設定所需的憑證之後，您就可以設定電腦成為同盟伺服器。 您可以使用下列程式，將電腦設定為新同盟伺服器陣列中的第一部同盟伺服器，方法是使用 [AD FS 同盟伺服器設定] Wizard。  
  
在陣列中建立第一部同盟伺服器也會建立新的 Federation Service 並使此電腦成為主要同盟伺服器。 這表示這部電腦將會使用 AD FS 設定資料庫的讀取\/寫入複本進行設定。 此伺服器陣列中的所有其他同盟伺服器都必須將主要同盟伺服器上所做的任何變更複寫到其讀取\-只會將其儲存在本機的 AD FS 設定資料庫複本。 如需有關此複寫程序的詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 若為同盟 Web 單一\-在 \(SSO\) 設計上登\-，您必須在帳戶夥伴組織中至少有一部同盟伺服器，且資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要 Domain Admins 的成員資格或已被授與寫入 Active Directory 中「程式資料」容器之權限的受委派網域帳戶。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動精靈，請執行下列其中一項：  
  
    -   同盟服務角色服務安裝完成後，請在中開啟 [AD FS 管理] 嵌入式\-管理單元，然後按一下 [**總覽**] 頁面或 [**動作**] 窗格中的 [ **AD FS 同盟伺服器設定]** 連結。  
  
    -   在安裝程式完成後的任何時間，請開啟 Windows Explorer，流覽至**C：\\Windows\\ADFS**資料夾，然後按兩下 [ **fsconfigwizard.exe**]\-按一下 [執行]。  
  
2.  在 [歡迎使用] 頁面上，驗證已選取 [建立新的同盟服務]，然後按 [下一步]。  
  
3.  在 [**選取獨立\-或伺服器陣列部署**] 頁面上，按一下 [**新增同盟伺服器**陣列]，然後按 **[下一步]** 。  
  
4.  在 [指定同盟服務名稱] 頁面上，驗證顯示的 [SSL 憑證] 正確。 如果這不是正確憑證，請從 [SSL 憑證] 清單中選取適當的憑證。  
  
    此憑證是從預設網站的 安全通訊端層 \(SSL\) 設定產生。 如果 [預設網站] 只設定一個 SSL 憑證，則會呈現該憑證，並自動選取該憑證以供使用。 如果 [預設網站] 設定多個 SSL 憑證，則所有憑證都會列在這裡，而且您必須從中進行選取。 如果未設定 [預設網站] 的 SSL 設定，則會從本機電腦的個人憑證存放區中的可用憑證產生清單。  
  
    > [!NOTE]  
    > 如果不是針對 IIS 設定的 SSL 憑證，精靈將不允許您覆寫憑證。 這樣可以確認保留 SSL 憑證所有預期的舊 IIS 設定。 若要解決此限制，您可以移除憑證，或使用 [IIS 管理主控台] 手動重新設定它。  
  
5.  如果您選取的 AD FS 資料庫已經存在，則會出現 [偵測**到現有的 AD FS 設定資料庫**] 頁面。 如果顯示該頁面，請按一下 [刪除資料庫]，然後按 [下一步]。  
  
    > [!CAUTION]  
    > 只有在您確定此 AD FS 資料庫中的資料不重要，或資料未用於生產同盟伺服器陣列時，才選取此選項。  
  
6.  在 [指定服務帳戶] 頁面上，按一下 [瀏覽]。 在 [瀏覽] 對話方塊中，尋找這個新同盟伺服器陣列中要當成服務帳戶使用的網域帳戶，然後按一下 [確定]。 輸入此帳戶的密碼，並確認密碼，然後按 [下一步]。  
  
    > [!NOTE]  
    > 如需為同盟伺服器陣列指定服務帳戶的詳細資訊，請參閱[手動設定同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。 同盟伺服器陣列中的每部同盟伺服器都必須指定相同的服務帳戶，伺服器陣列才能夠運作。 例如，如果建立的服務帳戶是 contoso\\ADFS2SVC，則您為同盟伺服器角色設定且將參與相同伺服器陣列的每部電腦，都必須在同盟伺服器設定 Wizard 的這個步驟中指定 contoso\\ADFS2SVC，讓伺服器陣列能夠運作。  
  
7.  在 [已可套用設定] 頁面上，檢閱詳細資料。 如果顯示的設定正確無誤，請按 [下一步]，開始以這些設定進行 AD FS 的設定。  
  
8.  在 [設定結果] 頁面上檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。  
  
    > [!IMPORTANT]  
    > 基於安全部署用途，使用 [AD FS Federation Server 設定精靈] 設定同盟伺服器陣列時，會停用成品解析和回覆偵測。 此精靈會自動設定 Windows Internal Database 來儲存服務設定資料。 不過，您可以使用中 AD FS 管理 嵌入式\-管理單元中的 **端點** 節點，或在 Windows PowerShell 中啟用\-enable-adfsendpoint Cmdlet，藉以錯誤地復原此變更。 請特別注意，不要重新設定預設設定，以在同時使用同盟伺服器陣列和 Windows Internal Database 時，將此端點保持為停用狀態。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

