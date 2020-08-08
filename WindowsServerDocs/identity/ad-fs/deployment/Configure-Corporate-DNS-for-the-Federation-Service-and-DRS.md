---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: 設定同盟服務與 DRS 的公司 DNS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: de0313a65234c637018b0c7b2d7a7c9212464077
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938426"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>設定同盟服務與 DRS 的公司 DNS

## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>步驟6：將主機 \( a \) 和別名 \( CNAME \) 資源記錄新增至同盟服務和 DRS 的公司 DNS
您必須 \( \) 針對您在先前步驟中設定的 federation Service 和裝置註冊服務，將下列資源記錄新增至公司網域名稱系統 DNS。

|進入|類型|位址|
|---------|--------|-----------|
|federation \_ service \_ 名稱|主機 \( A\)|AD FS 伺服器的 IP 位址，或在 AD FS 伺服器陣列前方設定的負載平衡器 IP 位址|
|enterpriseregistration|別名 \( CNAME\)|同盟 \_ 伺服器 \_ name.contoso.com|

您可以使用下列程式，將主機 \( a \) 和別名 \( CNAME \) 資源記錄新增至同盟伺服器和裝置註冊服務的公司 DNS。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>將主機 \( a \) 和別名 \( CNAME \) 資源記錄新增至同盟伺服器的 DNS

1.  在您的網域控制站上，于伺服器管理員的 [**工具**] 功能表上，按一下 [ **dns** ] 以開啟 dns 嵌入式管理單元 \- 。

2.  在主控台樹中，依序展開 [**域 \_ 控制器 \_ 名稱**] 節點和 [**正向對應區域**]，以滑鼠右鍵 \- 按一下 [功能變數名稱]，然後按一下 [**新增主機 \( A] 或 [AAAA \) **]。 ** \_ **

3.  在 [**名稱**] 方塊中，輸入要用於 AD FS 伺服器陣列的名稱。

4.  在 [**IP 位址**] 方塊中，輸入您的同盟伺服器的 IP 位址。 按一下 [新增主機]****。

5.  以滑鼠右鍵 \- 按一下 [**功能變數名稱 \_ ** ] 節點，然後按一下 [**新增別名 \( CNAME \) **]。

6.  在 [新增資源記錄]**** 對話方塊中，在 [別名名稱]**** 方塊中輸入 **enterpriseregistration**。

7.  在 [目標主機的完整功能變數名稱 FQDN] 方塊中 \( \) ，輸入**federation \_ service 伺服器 \_ 陣列 \_ 名. Domain \_ name.com**，然後按一下 **[確定]**。

    > [!IMPORTANT]
    > 在真實世界的部署中，如果您的公司有多個使用者主要名稱 \( UPN \) 尾碼，您必須為 DNS 中的每個 UPN 尾碼建立多個 CNAME 記錄。

## <a name="see-also"></a>另請參閱

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)


