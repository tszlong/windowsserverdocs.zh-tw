---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: "設定 DRS 與同盟服務的公司 DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>設定 DRS 與同盟服務的公司 DNS

>適用於：Windows Server 2016、Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步驟 6： 新增主機 \(A\) 和別名 \(CNAME\) 資源記錄公司 DNS DRS 和同盟服務  
您必須同盟服務的公司網域名稱系統 \(DNS\) 和裝置登記服務您設定在上一個步驟中新增下列資源記錄。  
  
|項目|輸入|地址|  
|---------|--------|-----------|  
|federation\_service\_name|主機 \(A\)|AD FS 伺服器的 IP 位址設定前面 AD FS 伺服器陣列負載平衡器 IP 位址|  
|enterpriseregistration|別名 \(CNAME\)|federation\_server\_name.contoso.com|  
  
您可以將主機 \(CNAME\) 資源 \(A\) 和別名記錄新增至企業 DNS 伺服器聯盟和裝置登記服務使用下列程序。  
  
資格在**系統管理員**，或相當於，才能完成此程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>新增 DNS 伺服器聯盟主機 \(CNAME\) 資源 \(A\) 和別名記錄  
  
1.  在您網域控制站在伺服器管理員中，在**工具**功能表上，按一下 [ **DNS**打開 DNS snap\ 中。  
  
2.  在主控台中，展開**domain\_controller\_name**節點中，展開**正向對應區域**，right\ 按**domain\_name**，，然後按一下**新主機 \(A or AAAA\)**。  
  
3.  在**名稱**方塊中，輸入要使用 AD FS 發電廠您的名稱。  
  
4.  在**的 IP 位址**方塊中，輸入您聯盟伺服器的 IP 位址。 按一下**新增主機**。  
  
5.  Right\ 按一下**domain\_name**節點，然後再按**新別名 \(CNAME\)**。  
  
6.  在**新資源記錄**對話方塊中，輸入**enterpriseregistration**中**別名**方塊。  
  
7.  中的完整網域名稱 \(FQDN\) 的目標主機方塊中，輸入**federation\_service\_farm\_name.domain\_name.com**，然後按**[確定]**。  
  
    > [!IMPORTANT]  
    > 在現實世界的部署，如果您的公司有多個使用者主體名稱 \(UPN\) 尾碼，您必須建立多個 CNAME 記錄每個 dns 這些 UPN 尾碼。  
  
## <a name="see-also"></a>也了 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署聯盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

