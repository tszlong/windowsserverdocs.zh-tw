---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: "建立的第一個聯盟伺服器聯盟伺服器陣列"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>建立的第一個聯盟伺服器聯盟伺服器陣列

 >適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您安裝同盟服務的角色，並在電腦上設定所需的憑證之後，您就可以設定電腦成為聯盟伺服器。 您可以使用下列程序來設定電腦變得新聯盟伺服器陣列使用 AD FS 聯盟伺服器設定精靈中的第一個聯盟伺服器。  
  
建立的第一個聯盟伺服器發電廠中的動作也會建立新的同盟服務，並讓這台電腦的主要聯盟伺服器。 這表示這部電腦，將會使用 AD FS 設定資料庫 read\/寫入複本設定。 所有其他聯盟伺服器此必須複製將它們儲存在本機 AD FS 設定資料庫他們僅限 read\ 複本主要聯盟伺服器上所做的任何變更。 如需有關這個複寫程序，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
> [!NOTE]  
> 聯盟網路 Single\-Sign\-On \(SSO\) 設計，您必須至少一個聯盟伺服器 account 合作夥伴組織和資源合作夥伴組織中的至少一個聯盟伺服器。 如需詳細資訊，請查看[放置聯盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
資格網域系統管理員」，或獲得寫入存取 Active Directory 中程式資料容器委派的核對最小，才能完成此程序。  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>若要建立的第一個聯盟伺服器聯盟伺服器陣列中  
  
1.  有兩種方法可以開始 AD FS 聯盟伺服器設定精靈。 若要開始精靈中，執行下列其中一個動作：  
  
    -   同盟服務角色服務安裝完成後，開放 AD FS 管理 snap\ 中，按一下**AD FS 聯盟伺服器設定精靈**上的連結**概觀**頁面或**控制項**窗格。  
  
    -   隨時之後安裝精靈完成，開放 Windows 檔案總管] 瀏覽至**C:\\Windows\\ADFS**資料夾，然後 double\ 按**FsConfigWizard.exe**。  
  
2.  在**歡迎**頁面上，確認**建立新的同盟服務**已選取，然後按一下 [**下一步**。  
  
3.  上**選取 Stand\-只或發電廠部署**頁面上，按一下 [**新聯盟伺服器陣列**，然後按一下 [**下一步**。  
  
4.  在**同盟服務名稱指定**頁面上，確認**SSL 憑證**，會顯示正確。 如果這不是正確的憑證，選取適當的憑證的**SSL 憑證**清單中。  
  
    這個憑證也從安全通訊端層 \(SSL\) 設定為預設值的網站。 如果只有一個 SSL 憑證設定預設值的網站，該憑證呈現及自動選取 [使用。 多個 SSL 憑證的網站，預設設定，如果以下列出這些所有憑證，您必須從他們中選取。 不 SSL 設定為預設值的網站時，也可在本機電腦上的個人化的憑證存放區憑證的清單。  
  
    > [!NOTE]  
    > 精靈將不允許您若 SSL 憑證已設定為 IIS 覆寫憑證。 這樣可確保任何預期會保留先前 IIS 組態 SSL 憑證。 若要替代這項限制時，您可以移除憑證或重新手動 IIS Management Console 的設定。  
  
5.  如果您已經選取 AD FS 資料庫存在，**現有 AD FS 設定資料庫偵測到**頁面隨即顯示。 是否出現該頁面，請按一下**Delete 資料庫**，然後按一下 [**下**。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用正式作業聯盟伺服器陣列中，選取此選項。  
  
6.  在**指定服務帳號**頁面上，按**瀏覽]**。 在**瀏覽**對話方塊中，尋找網域帳號，做為服務中帳號這個新的聯盟伺服器發電廠，然後按一下 [ **[確定]**。 輸入密碼、確認，請然後按一下**下一步**。  
  
    > [!NOTE]  
    > 查看[手動設定聯盟伺服器陣列服務 Account](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)的詳細資訊指定聯盟伺服器陣列服務負責。 聯盟伺服器陣列中的每個聯盟伺服器必須指定相同服務負責發電廠才能正常運作。 例如，如果建立服務 account contoso\\ADFS2SVC，聯盟伺服器角色您設定，並將參與相同發電廠每一部電腦必須 contoso\\ADFS2SVC 此步驟，精靈中指定聯盟伺服器設定陣列才能正常運作。  
  
7.  在**適用於設定準備**頁面上，檢視詳細資料。 若出現正確設定，請按一下**下一步**來設定 AD FS 使用這些設定。  
  
8.  在**設定結果**頁面上，檢視結果。 所有的設定步驟完成時，按**關閉**以結束精靈。  
  
    > [!IMPORTANT]  
    > 基於安全部署，成品解析度並加以回覆偵測是當您使用 AD FS 聯盟伺服器設定精靈設定聯盟伺服器陣列停用。 精靈會自動設定 Windows 內部資料庫儲存服務設定資料。 您可能會但是，誤復原這項變更，進而成品解析度端點使用**的端點**節點 snap\ 中 AD FS 管理或 Enable\-ADFSEndpoint cmdlet Windows PowerShell 中的。 請務必重新使從此端點進行處於停用狀態，當您在一起使用聯盟伺服器陣列及 Windows 內部資料庫設定預設設定。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

