---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: 建立獨立同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359665"
---
# <a name="create-a-stand-alone-federation-server"></a>建立獨立同盟伺服器

安裝同盟服務角色服務並在電腦上設定所需的憑證之後，您就可以設定電腦成為同盟伺服器。 您可以使用下列程式，將電腦設定為 no__t 0alone 同盟伺服器。 建立待命 @ no__t-0alone 同盟伺服器的動作也會建立新的同盟服務。 您可以使用 AD FS 同盟伺服器設定 Wizard 來建立同盟伺服器。  
  
> [!NOTE]  
> 針對同盟 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 設計，您必須在帳戶夥伴組織中至少有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(HTTP：\/ \/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>建立獨立式 @ no__t-0alone 同盟伺服器  
  
1.  有兩種方式可以啟動 [AD FS 同盟伺服器設定向導]。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   同盟服務角色服務安裝完成後，請開啟 AD FS 管理 snap @ no__t-0in，然後按一下 [**總覽**] 頁面或 [**動作**] 窗格中的 [ **AD FS 同盟伺服器設定]** 連結。  
  
    -   在安裝程式完成之後，請開啟 [Windows Explorer]，流覽至**C： \\Windows @ no__t-2ADFS**資料夾，然後按兩下 @ no__t-3click **fsconfigwizard.exe .exe**。  
  
2.  在 [歡迎] 頁面上，確認已選取 [建立新的 Federation Service]，然後按一下 [下一步]。  
  
3.  在 [**選取待命 @ no__t-1Alone 或伺服器陣列部署**] 頁面上，按一下 [待命 **@ no__t-3Alone 同盟伺服器**]，然後按 **[下一步]** 。  
  
    > [!IMPORTANT]  
    > 當您在 [AD FS 同盟伺服器設定] Wizard 中選取 [待命 @ no__t-0alone 同盟伺服器] 選項時，與此同盟服務相關聯的服務帳戶將會自動指派給網路服務帳戶。 只有當您在測試實驗室環境中評估 AD FS 時，才建議使用 NETWORK SERVICE 做為服務帳戶。 如果您想要使用 [待命 @ no__t-0alone 同盟伺服器] 選項，在生產環境中部署同盟伺服器，請務必將此服務帳戶變更為更適當的服務帳戶，以便專用於服務的要求這個新的同盟服務。 將服務帳戶變更為網路服務以外的帳戶，將可減輕可能的攻擊媒介，否則會讓您的同盟伺服器容易遭受惡意攻擊。  
  
4.  在 [指定 Federation Service 名稱] 頁面上，確認顯示的 [SSL 憑證] 是正確的。 如果沒有，請從 [ **SSL 憑證**] 清單中選取適當的憑證。  
  
    此憑證是從預設網站的安全通訊端層 \(SSL @ no__t-1 設定產生。 若「預設的網站」只設定一個 SSL 憑證，則會出示該憑證並自動選取該憑證以供使用。 若已為「預設的網站」設定多個 SSL 憑證，則此處會列出所有那些憑證，而且您必須從中選取一個憑證。 若沒有為「預設的網站」設定 SSL 設定，則會從本機電腦上個人憑證存放區中可用的憑證產生清單。  
  
    > [!NOTE]  
    > 若已為 IIS 設定 SSL 憑證，精靈將不會允許您覆寫該憑證。 這樣可確保先前為 IIS 設定的 SSL 憑證都會被保留。 若要解決此限制，您可以移除憑證，或使用 IIS 管理主控台手動重新設定它。  
  
5.  如果您選取的 AD FS 資料庫已經存在，則會出現 [偵測**到現有的 AD FS 設定資料庫**] 頁面。 如果出現該頁面，按一下 **[刪除資料庫]** ，然後按一下 **[下一步]** 。  
  
    > [!CAUTION]  
    > 只有在您確定此 AD FS 資料庫中的資料不重要，或在生產同盟伺服器陣列中未使用時，才選取此選項。  
  
6.  在 [準備套用設定] 頁面上，檢閱詳細資料。 如果設定看起來是正確的，請按 **[下一步]** 開始使用這些設定來設定 AD FS。  
  
7.  在 [設定結果] 頁面上，檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

