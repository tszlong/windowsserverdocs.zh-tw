---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: 匯出伺服器驗證憑證的私密金鑰部分
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6baa734e3fc346d94f4387e2ed54d3e707e5af75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855421"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>匯出伺服器驗證憑證的私密金鑰部分

Active Directory 同盟服務 \(AD FS\) 伺服器陣列中的每部同盟伺服器都必須能夠存取伺服器驗證憑證的私密金鑰。 如果您要執行同盟伺服器或 Web 服務器的伺服器陣列，您必須擁有單一驗證憑證。 此憑證必須由企業憑證授權單位單位發行 \(CA\)，而且必須具有可匯出的私密金鑰。 伺服器驗證憑證的私密金鑰必須可匯出，以便可提供給陣列中的所有伺服器使用。  
  
同盟伺服器 proxy 伺服器陣列的這個概念也是如此，因為伺服器陣列中所有的同盟伺服器 proxy 必須共用相同伺服器驗證憑證的私密金鑰部分。  
  
> [!NOTE]  
> 中的 [AD FS 管理] 嵌入式\-管理單元是指做為服務通訊憑證之同盟伺服器的伺服器驗證憑證。  
  
根據這部電腦會播放的角色，在您安裝伺服器驗證憑證與私密金鑰的同盟伺服器電腦或同盟伺服器 proxy 電腦上使用此程式。 完成此程序時，您可以接著在陣列中每部伺服器的「預設的網站」上匯入此憑證。 如需詳細資訊，請參閱將[伺服器驗證憑證匯入至預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>匯出伺服器驗證憑證的私密金鑰部分  
  
1. 在 **開始** 畫面上，輸入**Internet Information Services \(IIS\) 管理員**，然後按 enter。  
  
2. 在主控台樹狀目錄中，按一下 [ComputerName]。  
  
3. 在中央窗格中，按兩下 [**伺服器憑證**]\-。  
  
4. 在中央窗格中，以滑鼠右鍵\-按一下您要匯出的憑證，然後按一下 [**匯出**]。  
  
5. 在 [**匯出憑證**] 對話方塊中，按一下 [ **...** ] 按鈕。  
  
6. 在 [**檔案名**] 中，輸入**C：\\** <em>NameofCertificate</em>，然後按一下 [**開啟**]。  
  
7. 輸入該憑證的密碼並再次確認，然後按一下 [確定]。  
  
8. 檢查系統是否已在您指定的位置建立您指定的檔案，以驗證是否成功匯出。  
  
   > [!IMPORTANT]  
   > 為了可以將此憑證匯入到新伺服器上的本機憑證存放區，在將檔案傳輸到新伺服器期間，您必須將檔案傳輸到實體媒體上並保護其安全。 保護私密金鑰的安全格外重要。 如果此金鑰遭到入侵，則整個 AD FS 部署的安全性 \(包括組織內和資源夥伴組織中的資源\) 遭到入侵。  
  
9. 安裝 Federation Service 之前，請先將匯出的伺服器驗證憑證匯入到新伺服器上的憑證存放區。 如需有關如何匯入憑證的詳細資訊，請參閱 < 匯入伺服器憑證 \([HTTP：\/\/go.microsoft.com\/fwlink\/？LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\)。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[同盟伺服器 Proxy 的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)  
  

