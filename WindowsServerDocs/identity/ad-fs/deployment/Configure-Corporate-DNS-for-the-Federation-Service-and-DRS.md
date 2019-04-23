---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: 設定同盟服務與 DRS 的公司 DNS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876389"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>設定同盟服務與 DRS 的公司 DNS

>適用於：Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步驟 6：將主機新增\(A\)和別名\(CNAME\)同盟服務和 DRS 的公司 DNS 資源記錄  
您必須加入公司網域名稱系統中的下列資源記錄\(DNS\)您的 federation service 和您在上一個步驟中設定的裝置註冊服務。  
  
|進入|類型|地址|  
|---------|--------|-----------|  
|federation\_service\_name|主機\(A\)|在 AD FS 伺服器或 AD FS 伺服器陣列之前已設定的負載平衡器的 IP 位址的 IP 位址|  
|enterpriseregistration|別名\(CNAME\)|federation\_server\_name.contoso.com|  
  
您可以使用下列程序，新增的主機\(A\)和別名\(CNAME\)公司 DNS，同盟伺服器和裝置註冊服務的資源記錄。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最低需求。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>若要新增的主機\(A\)和別名\(CNAME\) dns 資源記錄您的同盟伺服器  
  
1.  您網域控制站上，在 [伺服器管理員] 中，在**工具**功能表上，按一下**DNS**以開啟 DNS 嵌入式管理單元\-中。  
  
2.  在主控台樹狀目錄中，依序展開**網域\_控制器\_名稱**節點，展開**正向對應區域**，以滑鼠右鍵\-按一下**網域\_名稱**，然後按一下**新的主控件\(A 或 AAAA\)**。  
  
3.  在 **名稱**方塊中，輸入要使用您的 AD FS 伺服器陣列的名稱。  
  
4.  在 [**IP 位址**] 方塊中，輸入您的同盟伺服器的 IP 位址。 按一下 [新增主機] 。  
  
5.  右\-按一下 **網域\_名稱**節點，然後再按一下**新別名\(CNAME\)**。  
  
6.  在 [新增資源記錄] 對話方塊中，在 [別名名稱] 方塊中輸入 **enterpriseregistration**。  
  
7.  在 完整的網域名稱\(FQDN\)的目標主機 方塊中，輸入**同盟\_服務\_伺服陣列\_name.domain\_名稱.com**，，然後按一下 **確定**。  
  
    > [!IMPORTANT]  
    > 在真實世界部署中，如果您的公司有多個使用者主體名稱\(UPN\)尾碼，您必須為每一個 UPN 尾碼，在 DNS 中建立多個 CNAME 記錄。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

