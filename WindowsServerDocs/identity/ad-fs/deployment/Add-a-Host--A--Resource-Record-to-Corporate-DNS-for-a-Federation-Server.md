---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: 將主機 (A) 資源記錄新增至同盟伺服器的公司 DNS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fdf75408e4627fb587f95e9b1c8d2fb93c8e9410
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960040"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>將主機 (A) 資源記錄新增至同盟伺服器的公司 DNS



若要讓公司網路上的用戶端使用 Windows 整合式驗證成功存取同盟伺服器， \( \) 必須先在公司網域名稱系統 DNS 中建立主機資源記錄， \( 以 \) 解析帳戶同盟伺服器的主機名稱 \( ，例如，fs.fabrikam.com \) 到同盟伺服器或同盟伺服器叢集的 IP 位址。 您可以使用下列程式，將主機 \( a \) 資源記錄新增至同盟伺服器的公司 DNS。  
  
若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>將主機 \( a \) 資源記錄新增至同盟伺服器的公司 DNS  
  
1.  在公司網路的 DNS 伺服器上，開啟 [DNS] 嵌入式管理單元 \- 。  
  
2.  在主控台樹中，以滑鼠右鍵 \- 按一下適用的正向對應區域，然後按一下 [**新增主機 \( A] 或 [AAAA \) **]。  
  
3.  在 [**名稱**] 中，僅輸入同盟伺服器或同盟伺服器叢集的電腦名稱稱;例如，針對完整功能變數名稱 \( FQDN \) fs.fabrikam.com，輸入**fs**。  
  
4.  在 [ **ip 位址**] 中，輸入同盟伺服器或同盟伺服器叢集的 ip 位址，例如192.168.1.4。  
  
5.  按一下 [新增主機]****。  
  
## <a name="additional-references"></a>其他參考  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[同盟伺服器的名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))  
  
