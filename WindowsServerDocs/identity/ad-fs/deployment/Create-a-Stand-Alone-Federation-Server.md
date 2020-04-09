---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: 建立獨立同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 603786d8553cce20f0b559ba8a91dfc29f760488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855481"
---
# <a name="create-a-stand-alone-federation-server"></a>建立獨立同盟伺服器

安裝同盟服務角色服務並在電腦上設定所需的憑證之後，您就可以設定電腦成為同盟伺服器。 您可以使用下列程式，將電腦設定成獨立的\-獨立同盟伺服器。 建立獨立\-獨立同盟伺服器的動作也會建立新的同盟服務。 您可以使用 AD FS 同盟伺服器設定 Wizard 來建立同盟伺服器。  
  
> [!NOTE]  
> 若為同盟 Web 單一\-在 \(SSO\) 設計上登\-，您必須在帳戶夥伴組織中至少有一部同盟伺服器，且資源夥伴組織中至少要有一部同盟伺服器。 如需詳細資訊，請參閱[同盟伺服器放置位置](https://technet.microsoft.com/library/dd807127.aspx)。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\(HTTP：\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>建立獨立的\-獨立同盟伺服器  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動精靈，請執行下列其中一項：  
  
    -   同盟服務角色服務安裝完成後，請在中開啟 [AD FS 管理] 嵌入式\-管理單元，然後按一下 [**總覽**] 頁面或 [**動作**] 窗格中的 [ **AD FS 同盟伺服器設定]** 連結。  
  
    -   在安裝程式完成之後，請開啟 [Windows Explorer]，流覽至**C：\\windows\\ADFS**資料夾，然後按兩下 [fsconfigwizard.exe]\-按一下 [ **.exe**]。  
  
2.  在 [歡迎使用] 頁面上，驗證已選取 [建立新的同盟服務]，然後按 [下一步]。  
  
3.  在 [**單獨選取獨立\-或伺服器陣列部署**] 頁面上，按一下 [**獨立式\-獨立同盟伺服器**]，然後按 **[下一步]** 。  
  
    > [!IMPORTANT]  
    > 當您選取 [AD FS 同盟伺服器設定向導] 中的 [獨立\-獨立同盟伺服器] 選項時，與此同盟服務相關聯的服務帳戶將會自動指派給網路服務帳戶。 只有當您在測試實驗室環境中評估 AD FS 時，才建議使用 NETWORK SERVICE 做為服務帳戶。 如果您想要使用 [獨立\-的同盟伺服器] 選項，在生產環境中部署同盟伺服器，請務必將此服務帳戶變更為更適當的服務帳戶，以便專門用來提供此新同盟服務的要求。 將服務帳戶變更為網路服務以外的帳戶，將可減輕可能的攻擊媒介，否則會讓您的同盟伺服器容易遭受惡意攻擊。  
  
4.  在 [指定同盟服務名稱] 頁面上，驗證顯示的 [SSL 憑證] 正確。 如果沒有，請從 [ **SSL 憑證**] 清單中選取適當的憑證。  
  
    此憑證是從預設網站的 安全通訊端層 \(SSL\) 設定產生。 如果 [預設網站] 只設定一個 SSL 憑證，則會呈現該憑證，並自動選取該憑證以供使用。 如果 [預設網站] 設定多個 SSL 憑證，則所有憑證都會列在這裡，而且您必須從中進行選取。 如果未設定 [預設網站] 的 SSL 設定，則會從本機電腦的個人憑證存放區中的可用憑證產生清單。  
  
    > [!NOTE]  
    > 如果不是針對 IIS 設定的 SSL 憑證，精靈將不允許您覆寫憑證。 這樣可以確認保留 SSL 憑證所有預期的舊 IIS 設定。 若要解決此限制，您可以移除憑證，或使用 IIS 管理主控台手動重新設定它。  
  
5.  如果您選取的 AD FS 資料庫已經存在，則會出現 [偵測**到現有的 AD FS 設定資料庫**] 頁面。 如果出現此情形，請按一下 [刪除資料庫]，然後按 [下一步]。  
  
    > [!CAUTION]  
    > 只有在您確定此 AD FS 資料庫中的資料不重要，或資料未用於生產同盟伺服器陣列時，才選取此選項。  
  
6.  在 [已可套用設定] 頁面上，檢閱詳細資料。 如果顯示的設定正確無誤，請按 [下一步]，開始以這些設定進行 AD FS 的設定。  
  
7.  在 [設定結果] 頁面上檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

