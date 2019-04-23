---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: 匯出伺服器驗證憑證的私密金鑰部分
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857189"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>匯出伺服器驗證憑證的私密金鑰部分

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Active Directory Federation Services 的每一部同盟伺服器\(AD FS\)伺服陣列必須具有伺服器驗證憑證的私密金鑰存取權。 如果您要實作同盟伺服器或 Web 伺服器的伺服器陣列，您必須有單一驗證憑證。 此憑證必須由企業憑證授權單位核發\(CA\)，而且必須可匯出私密金鑰。 伺服器驗證憑證的私密金鑰必須可匯出，以便可提供給陣列中的所有伺服器使用。  
  
這個相同的概念是伺服陣列中的所有同盟伺服器 proxy 必須都共用相同的伺服器驗證憑證的私密金鑰部分，因為同盟伺服器 proxy 的陣列，則為 true。  
  
> [!NOTE]  
> AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。  
  
根據此電腦將扮演的角色，使用此程序在同盟伺服器電腦或同盟伺服器 proxy 電腦上安裝伺服器驗證憑證的私密金鑰。 完成此程序時，您可以接著在陣列中每部伺服器的「預設的網站」上匯入此憑證。 如需詳細資訊，請參閱 <<c0> [ 伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>匯出伺服器驗證憑證的私密金鑰部分  
  
1.  在 **開始**畫面上，輸入**Internet Information Services \(IIS\) Manager**，然後按 ENTER 鍵。  
  
2.  在主控台樹狀目錄中，按一下 [ComputerName]。  
  
3.  在中央窗格中，按兩下\-按一下 **伺服器憑證**。  
  
4.  在中央窗格中，以滑鼠右鍵\-按一下您想要匯出，然後按一下 憑證**匯出**。  
  
5.  在 **匯出憑證** 對話方塊中，按一下  **...** 按鈕。  
  
6.  在 **檔案名稱**，類型 **c:\\* * * NameofCertificate*，然後按一下**開啟**。  
  
7.  輸入該憑證的密碼並再次確認，然後按一下 [確定]。  
  
8.  檢查系統是否已在您指定的位置建立您指定的檔案，以驗證是否成功匯出。  
  
    > [!IMPORTANT]  
    > 為了可以將此憑證匯入到新伺服器上的本機憑證存放區，在將檔案傳輸到新伺服器期間，您必須將檔案傳輸到實體媒體上並保護其安全。 保護私密金鑰的安全格外重要。 如果此機碼遭到入侵，整個 AD FS 部署的安全性\(包括在組織內部和資源夥伴組織中的資源\)遭到入侵。  
  
9. 安裝 Federation Service 之前，請先將匯出的伺服器驗證憑證匯入到新伺服器上的憑證存放區。 如需如何匯入憑證的資訊，請參閱匯入伺服器憑證\( [http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\)。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[同盟伺服器 Proxy 的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)  
  

