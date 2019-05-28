---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: 同盟伺服器的名稱解析需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2776cc29b8c9ede884a6b304cd541f700f516ca
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191256"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>同盟伺服器的名稱解析需求

當公司網路上的用戶端電腦嘗試存取應用程式或 Web 服務所保護的 Active Directory Federation Services \(AD FS\)，他們必須先驗證同盟伺服器。 驗證的一個方式是將透過 Windows 整合式驗證存取本機同盟伺服器的公司網路用戶端。  
  
## <a name="configure-corporate-dns"></a>設定公司 DNS  
因此，透過 Windows 整合式驗證的本機同盟伺服器上的成功名稱解析可能會發生，網域名稱系統\(DNS\)公司網路中的帳戶夥伴必須設定新的主機\(A\)會將完整的網域名稱解析的資源記錄\(FQDN\)同盟伺服器叢集的 IP 位址的同盟伺服器的主機名稱。  
  
在下圖中，您可以看到特定案例如何完成這項工作。 在此案例中，Microsoft 網路負載平衡\(NLB\)提供現有的同盟伺服器陣列中的單一叢集 FQDN 名稱和單一叢集 IP 位址。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
如需有關如何設定叢集 IP 位址或叢集 FQDN 使用 NLB 的資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
如需有關如何設定公司 DNS 的同盟伺服器的資訊，請參閱 <<c0> [ 主機&#40;的&#41;至同盟伺服器的公司 DNS 資源記錄](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。</c0>  
  
如需有關如何設定同盟伺服器 proxy 在周邊網路中的資訊，請參閱 <<c0> [ 同盟伺服器 Proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
