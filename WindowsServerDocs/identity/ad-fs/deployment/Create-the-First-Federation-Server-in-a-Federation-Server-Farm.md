---
description: 深入瞭解：在同盟伺服器陣列中建立第一部同盟伺服器
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: 在同盟伺服器陣列中建立第一部同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: b58db4c771a4fe40a44f972521a8664f2c876e5d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044976"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器

在安裝同盟服務角色服務並在電腦上設定所需的憑證之後，您就可以設定電腦成為同盟伺服器。 您可以使用下列程式，設定電腦，使其成為新的同盟伺服器陣列中的第一部同盟伺服器，方法是使用 AD FS 同盟伺服器設定]。

在陣列中建立第一部同盟伺服器也會建立新的 Federation Service 並使此電腦成為主要同盟伺服器。 這表示將會使用 AD FS 設定資料庫的讀寫複本來設定這部電腦 \/ 。 此伺服器陣列中的所有其他同盟伺服器，都必須將在主要同盟伺服器上進行的任何變更複寫到其 \- 儲存在本機的 AD FS 設定資料庫的唯讀複本。 如需有關此複寫程序的詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。

> [!NOTE]
> 針對同盟網頁單一 \- 登錄 \- \( SSO \) 設計，您必須至少在帳戶夥伴組織中至少有一部同盟伺服器，以及在資源夥伴組織中至少有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11))。

若要完成此程序，至少需要 Domain Admins 的成員資格或已被授與寫入 Active Directory 中「程式資料」容器之權限的受委派網域帳戶。

### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>在同盟伺服器陣列中建立第一部同盟伺服器

1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：

    -   同盟服務的角色服務安裝完成之後，請開啟 AD FS 管理] 嵌入式管理單元， \- 然後按一下 [**總覽**] 頁面或 [**動作**] 窗格中的 **AD FS 同盟伺服器設定向導]** 連結。

    -   在安裝程式完成之後的任何時間，開啟 [Windows 檔案總管]，流覽至 **C： \\ Windows \\ ADFS** 資料夾，然後按兩下 [ \- **FsConfigWizard.exe**]。

2.  在 [歡迎使用] 頁面上，驗證已選取 [建立新的同盟服務]，然後按 [下一步]。

3.  在 [ **選取獨立 \- 或伺服器陣列部署** ] 頁面上，按一下 [ **新增同盟伺服器** 陣列]，然後按 **[下一步]**。

4.  在 [指定同盟服務名稱] 頁面上，驗證顯示的 [SSL 憑證] 正確。 如果這不是正確憑證，請從 [SSL 憑證] 清單中選取適當的憑證。

    此憑證是從 \( 預設網站的安全通訊端層 SSL \) 設定產生。 若「預設的網站」只設定一個 SSL 憑證，則會出示該憑證並自動選取該憑證以供使用。 若已為「預設的網站」設定多個 SSL 憑證，則此處會列出所有那些憑證，而且您必須從中選取一個憑證。 若沒有為「預設的網站」設定 SSL 設定，則會從本機電腦上個人憑證存放區中可用的憑證產生清單。

    > [!NOTE]
    > 若已為 IIS 設定 SSL 憑證，精靈將不會允許您覆寫該憑證。 這樣可確保先前為 IIS 設定的 SSL 憑證都會被保留。 為因應此限制，您可以移除該憑證或使用 [IIS 管理主控台] 手動重新設定憑證。

5.  如果您選取的 AD FS 資料庫已存在，則會出現 [偵測到現有的 AD FS 組態資料庫] 頁面。 如果顯示該頁面，請按一下 [刪除資料庫]，然後按 [下一步]。

    > [!CAUTION]
    > 只有在您確定此 AD FS 資料庫中的資料不重要，或資料未用於生產同盟伺服器陣列時，才選取此選項。

6.  在 [指定服務帳戶] 頁面上，按一下 [瀏覽] 。 在 [瀏覽] 對話方塊中，尋找這個新同盟伺服器陣列中要當成服務帳戶使用的網域帳戶，然後按一下 [確定]。 輸入此帳戶的密碼，並確認密碼，然後按 [下一步]。

    > [!NOTE]
    > 如需有關為同盟伺服器陣列指定服務帳戶的詳細資訊，請參閱 [手動設定同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) 。 同盟伺服器陣列中的每部同盟伺服器都必須指定相同的服務帳戶，伺服器陣列才能運作。 例如，如果建立的服務帳戶是 contoso \\ ADFS2SVC，則您為同盟伺服器角色設定且將參與相同伺服器陣列的每部電腦都必須在 \\ 同盟伺服器設定向導的這個步驟中指定 contoso ADFS2SVC，才能讓伺服器陣列運作。

7.  在 [已可套用設定] 頁面上，檢閱詳細資料。 如果顯示的設定正確無誤，請按 [下一步]，開始以這些設定進行 AD FS 的設定。

8.  在 [設定結果] 頁面上檢閱結果。 當所有設定步驟都完成後，按一下 [關閉] 以 **結束**  嚮導。

    > [!IMPORTANT]
    > 基於安全的部署目的，當您使用 [AD FS 同盟伺服器設定精靈] 來設定同盟伺服器陣列時，會停用成品解析與回覆偵測。 此精靈會自動設定「Windows 內部資料庫」來儲存服務設定資料。 不過，您可能會使用 AD FS 管理嵌入式管理單元中的 [ **端點** ] 節點 \- 或 Windows PowerShell 中的 [啟用 get-adfsendpoint] Cmdlet，以錯誤地復原這項變更 \- 。 請小心，不要重新設定預設設定，這樣當您使用同盟伺服器陣列搭配「Windows 內部資料庫」時，此端點才能維持停用狀態。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)

