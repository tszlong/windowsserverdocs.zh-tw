---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: 設定同盟服務與 DRS 的公司 DNS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408444"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>設定同盟服務與 DRS 的公司 DNS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步驟 6：將主機 \(A @ no__t-1 和 Alias \(CNAME @ no__t-3 資源記錄新增至同盟服務和 DRS 的公司 DNS  
您必須針對您在先前步驟中設定的 federation service 和裝置註冊服務，將下列資源記錄新增至公司網域名稱系統 \(DNS @ no__t-1。  
  
|進入|Type|地址|  
|---------|--------|-----------|  
|同盟 @ no__t-0service @ no__t-1name|主機 \(A @ no__t-1|AD FS 伺服器的 IP 位址，或在 AD FS 伺服器陣列前方設定的負載平衡器 IP 位址|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|同盟 @ no__t-0server\_name.contoso.com|  
  
您可以使用下列程式，將主機 \(A @ no__t-1 和 alias \(CNAME @ no__t-3 資源記錄新增至同盟伺服器和裝置註冊服務的公司 DNS。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>若要將主機 \(A @ no__t-1 和 alias \(CNAME @ no__t-3 資源記錄新增至同盟伺服器的 DNS  
  
1.  在您的網域控制站上，于伺服器管理員的 [**工具**] 功能表上，按一下 [ **dns** ] 以開啟 dns 貼齊 @ no__t-2in。  
  
2.  在主控台樹中，依序展開 [ **domain @ no__t-1controller @ no__t-2name** ] 節點、[**正向對應區域**]、[右 @ no__t-4click**網域 @ no__t-6name**]，然後按一下 [**新增主機] \(A 或 AAAA @ no__t-9**。  
  
3.  在 [**名稱**] 方塊中，輸入要用於 AD FS 伺服器陣列的名稱。  
  
4.  在 [**IP 位址**] 方塊中，輸入您的同盟伺服器的 IP 位址。 按一下 [新增主機]。  
  
5.  Right @ no__t-0click [ **domain @ no__t-2name** ] 節點，然後按一下 [**新增別名] \(CNAME @ no__t-5**。  
  
6.  在 [新增資源記錄] 對話方塊中，在 [別名名稱] 方塊中輸入 **enterpriseregistration**。  
  
7.  在 [目標主機] 方塊的 [完整功能變數名稱 \(FQDN @ no__t-1] 中，輸入**同盟 @ no__t-3service @ no__t-4farm @ no__t-5name. domain @ no__t-6name .com**，然後按一下 **[確定]** 。  
  
    > [!IMPORTANT]  
    > 在真實世界的部署中，如果您的公司有多個使用者主體名稱 \(UPN @ no__t-1 尾碼，您就必須為 DNS 中的每個 UPN 尾碼建立多個 CNAME 記錄。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

