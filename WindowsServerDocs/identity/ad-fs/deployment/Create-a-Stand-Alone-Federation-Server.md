---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: "建立獨立聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>建立獨立聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您安裝同盟服務的角色，並在電腦上設定所需的憑證之後，您就可以設定電腦成為聯盟伺服器。 成為聯盟 stand\ 只伺服器設定電腦，您可以使用下列程序。 建立 stand\ 只聯盟伺服器的動作也會建立新的同盟服務。 您使用 AD FS 聯盟伺服器設定精靈建立聯盟伺服器。  
  
> [!NOTE]  
> 聯盟網路 Single\-Sign\-On \(SSO\) 設計，您必須至少一個聯盟伺服器 account 合作夥伴組織和資源合作夥伴組織中的至少一個聯盟伺服器。 如需詳細資訊，請查看[放置聯盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-create-a-stand-alone-federation-server"></a>若要建立 stand\ 只聯盟伺服器  
  
1.  有兩種方法可以開始 AD FS 聯盟伺服器設定精靈。 若要開始精靈中，執行下列其中一個動作：  
  
    -   同盟服務角色服務安裝完成後，開放 AD FS 管理 snap\ 中，按一下**AD FS 聯盟伺服器設定精靈**上的連結**概觀**頁面或**控制項**窗格。  
  
    -   依照本身需求加以安裝精靈完成，開放 Windows 檔案總管] 之後，瀏覽至**C:\\Windows\\ADFS**資料夾，然後 double\ 按**FsConfigWizard.exe**。  
  
2.  在**歡迎**頁面上，確認**建立新的同盟服務**已選取，然後按一下 [**下一步**。  
  
3.  上**選取 Stand\-只或發電廠部署**頁面上，按一下 [ **Stand\ 只聯盟伺服器**，然後按一下 [**下一步**。  
  
    > [!IMPORTANT]  
    > 當您選取 AD FS 聯盟伺服器設定精靈中的 [Stand\ 只聯盟伺服器] 選項時，與此同盟服務相關的服務帳號將會自動指派給網路服務 account。 僅限建議的情形評估 AD FS 在實驗室測試環境中使用服務 account 網路的服務。 如果您想要部署聯盟伺服器 production 環境中的使用 [Stand\ 只聯盟伺服器] 選項，請務必，您可以要求波此新同盟服務專用更適當服務過去變更此服務 account。 變更服務帳號以外的其他網路服務過去將會減少可能攻擊能否則讓您聯盟伺服器惡意攻擊。  
  
4.  在**同盟服務名稱指定**頁面上，確認**SSL 憑證**，會顯示正確。 如果不行，請選取適當的憑證的**SSL 憑證**清單中。  
  
    這個憑證也從安全通訊端層 \(SSL\) 設定為預設值的網站。 如果只有一個 SSL 憑證設定預設值的網站，該憑證呈現及自動選取 [使用。 多個 SSL 憑證的網站，預設設定，如果以下列出這些所有憑證，您必須從他們中選取。 不 SSL 設定為預設值的網站時，也可在本機電腦上的個人化的憑證存放區憑證的清單。  
  
    > [!NOTE]  
    > 精靈將不允許您若 SSL 憑證已設定為 IIS 覆寫憑證。 這樣可確保任何預期會保留先前 IIS 組態 SSL 憑證。 若要替代這項限制時，您可以移除憑證或重新設定以手動方式與 IIS 管理主控台。  
  
5.  如果您已經選取 AD FS 資料庫存在，**現有 AD FS 設定資料庫偵測到**頁面隨即顯示。 發生這種情形，如果按一下**Delete 資料庫**，然後按一下 [**下**。  
  
    > [!CAUTION]  
    > 只有當您確定此 AD FS 資料庫中的資料並不重要或不使用正式作業聯盟伺服器陣列中，選取此選項。  
  
6.  在**適用於設定準備**頁面上，檢視詳細資料。 若出現正確設定，請按一下**下一步**來設定 AD FS 使用這些設定。  
  
7.  在**設定結果**頁面上，檢視結果。 所有的設定步驟完成時，按**關閉**以結束精靈。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

