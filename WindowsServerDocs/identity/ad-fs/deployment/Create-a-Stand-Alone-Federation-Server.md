---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: 建立獨立同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888289"
---
# <a name="create-a-stand-alone-federation-server"></a>建立獨立同盟伺服器

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

安裝 Federation Service 角色服務後，當您在電腦上設定必要的憑證，您就能夠將電腦設定為同盟伺服器。 您可以使用下列程序來設定電腦成為獨立\-單獨的同盟伺服器。 建立獨立的動作\-單獨的同盟伺服器也會建立新的 Federation Service。 您使用 AD FS 同盟伺服器設定精靈建立同盟伺服器。  
  
> [!NOTE]  
> 同盟網頁單一\-號\-上\(SSO\)設計中，您必須在帳戶夥伴組織中的至少一部同盟伺服器和資源夥伴組織中的至少一部同盟伺服器. 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>若要建立獨立\-單獨的同盟伺服器  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   Federation Service 角色服務安裝完成之後，開啟 AD FS 管理嵌入式管理單元\-中，按一下  **AD FS 同盟伺服器設定精靈**上的連結**概觀**頁面或在**動作**窗格。  
  
    -   隨時安裝精靈完成，請開啟 Windows 檔案總管後，瀏覽至**c:\\Windows\\ADFS**資料夾，，然後按兩下\-按一下**FsConfigWizard.exe**.  
  
2.  在 [歡迎] 頁面上，確認已選取 [建立新的 Federation Service]，然後按一下 [下一步]。  
  
3.  上**選取獨立\-Alone 或伺服陣列部署**頁面上，按一下**獨立\-單獨的同盟伺服器**，然後按一下**下一步**。  
  
    > [!IMPORTANT]  
    > 當您選取之獨立\-單獨的同盟伺服器選項，在 AD FS 同盟伺服器設定精靈中，與此同盟服務相關聯的服務帳戶會自動指派給網路服務帳戶。 在您要在其中評估測試實驗室環境中的 AD FS 的情況下，才建議使用 NETWORK SERVICE 做為服務帳戶。 如果您想要使用之獨立\-單獨的同盟伺服器選項，以部署同盟伺服器在生產環境中，請務必將此服務帳戶變更至更適當的服務帳戶可以專屬於服務這個新的 Federation Service 的要求。 變更服務帳戶，而非網路服務帳戶將會降低會將您的同盟伺服器容易遭受惡意攻擊的攻擊媒介。  
  
4.  在 [指定 Federation Service 名稱] 頁面上，確認顯示的 [SSL 憑證] 是正確的。 如果沒有，請選取適當的憑證，從**SSL 憑證**清單。  
  
    此憑證會產生從 Secure Sockets Layer \(SSL\)在預設的網站的設定。 若「預設的網站」只設定一個 SSL 憑證，則會出示該憑證並自動選取該憑證以供使用。 若已為「預設的網站」設定多個 SSL 憑證，則此處會列出所有那些憑證，而且您必須從中選取一個憑證。 若沒有為「預設的網站」設定 SSL 設定，則會從本機電腦上個人憑證存放區中可用的憑證產生清單。  
  
    > [!NOTE]  
    > 若已為 IIS 設定 SSL 憑證，精靈將不會允許您覆寫該憑證。 這樣可確保先前為 IIS 設定的 SSL 憑證都會被保留。 若要解決這項限制，您可以移除憑證，或以手動方式重新加以設定使用 IIS Management Console。  
  
5.  如果您已選取的 AD FS 資料庫的話**AD FS 組態資料庫偵測到現有的**頁面隨即出現。 如果出現該頁面，按一下 **[刪除資料庫]**，然後按一下 **[下一步]**。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用生產同盟伺服器陣列中，請選取此選項。  
  
6.  在 [準備套用設定] 頁面上，檢閱詳細資料。 如果設定正確，請按一下**下一步**若要開始設定 AD FS 使用這些設定。  
  
7.  在 [設定結果] 頁面上，檢閱結果。 當所有的組態步驟都完成後時，按一下**關閉**結束精靈。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

