---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: "匯出伺服器驗證憑證的私人鍵部分"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>匯出伺服器驗證憑證的私人鍵部分

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 發電廠每個聯盟伺服器必須私密金鑰伺服器驗證憑證的存取。 如果您正在實作伺服器陣列聯盟伺服器或網頁伺服器，您必須單一驗證憑證。 必須核發此憑證的企業憑證授權單位 \(CA\)，而且它必須匯出私密金鑰。 伺服器驗證憑證的私密金鑰必須匯出，它可提供陣列中所有的伺服器。  
  
此相同的概念是如此聯盟伺服器 proxy 農場陣列中的所有聯盟伺服器 proxy，必須都共用相同的伺服器驗證憑證的私密部分據用量感知器中。  
  
> [!NOTE]  
> AD FS 管理 snap\ 中稱為伺服器驗證憑證的聯盟伺服器服務通訊的憑證。  
  
根據這台電腦將會播放的角色，使用此程序聯盟伺服器電腦或聯盟 proxy 伺服器的電腦上安裝伺服器驗證憑證的私密金鑰位置。 當您完成程序時，您可以再匯入這個預設網站發電廠中每個伺服器上的憑證。 如需詳細資訊，請查看[匯入伺服器驗證憑證的預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>若要匯出金鑰的私密部分伺服器驗證憑證  
  
1.  在**[開始]**畫面中，輸入**\(IIS\) 管理員]**，然後按 ENTER 鍵。  
  
2.  主控台中，按一下 [**電腦名稱**。  
  
3.  在中央窗格中，按一下 double\**伺服器的憑證**。  
  
4.  在中央窗格中，按一下您要匯出，然後按一下 [的憑證 right\**匯出**。  
  
5.  在**匯出憑證**對話方塊中，按**...** 按鈕。  
  
6.  在**檔案名稱**，輸入**C:\\***NameofCertificate*，然後按一下**開放**。  
  
7.  輸入憑證的密碼、確認，請然後按一下**[確定]**。  
  
8.  以確認您所指定的檔案，在指定的位置建立驗證您匯出的成功。  
  
    > [!IMPORTANT]  
    > 讓這個憑證可匯入到新的伺服器的憑證本機存放區，您必須將檔案傳送到實體媒體，並期間傳輸到新的伺服器保護其安全。 請務必非常使私密金鑰的安全性。 受到此機碼，如果您的整個 AD FS 部署的安全性 \ 受到（包括資源資源合作夥伴 organizations\ 與在組織中）。  
  
9. 安裝同盟服務之前，請匯出的伺服器驗證憑證匯入新的伺服器上的憑證存放區。 了解如何匯入憑證的資訊，會看到匯入伺服器的憑證 \ ([http:///\/go.microsoft.com\/fwlink\/ 嗎？LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\)。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[聯盟的 Proxy 伺服器的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)  
  

