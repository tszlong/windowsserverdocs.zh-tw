---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: 將主機 (A) 資源記錄新增至同盟伺服器的公司 DNS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5767fa45f8b25680aa1b1d97ddab630923d10fae
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192494"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>將主機 (A) 資源記錄新增至同盟伺服器的公司 DNS



使用 Windows 整合式驗證，主應用程式的同盟伺服器成功存取用戶端上的公司網路\(A\)資源記錄必須先建立公司網域名稱系統\(DNS\)帳戶同盟伺服器的主機名稱解析\(例如，fs.fabrikam.com\)同盟伺服器或同盟伺服器叢集的 IP 位址。 您可以使用下列程序，新增的主機\(A\)至同盟伺服器的公司 DNS 資源記錄。  
  
中的成員資格**系統管理員**，或同等權限，才能完成此程序的最小值。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>若要新增的主機\(A\)至同盟伺服器的公司 DNS 資源記錄  
  
1.  在公司網路的 DNS 伺服器，開啟 [DNS] 嵌入式管理單元\-中。  
  
2.  在主控台樹狀目錄中，以滑鼠右鍵\-按一下適用的正向對應區域，然後按一下**新主機\(A 或 AAAA\)** 。  
  
3.  在 **名稱**，輸入只有電腦名稱的同盟伺服器或同盟伺服器叢集，例如完整的網域名稱\(FQDN\) fs.fabrikam.com，輸入**fs**.  
  
4.  在  **IP 位址**，輸入同盟伺服器或同盟伺服器叢集，例如 192.168.1.4 的 IP 位址。  
  
5.  按一下 [新增主機]  。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[同盟伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  

