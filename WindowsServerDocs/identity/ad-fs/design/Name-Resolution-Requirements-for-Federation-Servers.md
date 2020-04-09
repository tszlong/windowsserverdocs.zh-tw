---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: 同盟伺服器的名稱解析需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b455596910e1b04569e087843da8b88f9f020137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853031"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>同盟伺服器的名稱解析需求

當公司網路上的用戶端電腦嘗試存取受 Active Directory 同盟服務保護的應用程式或 Web 服務 \(AD FS\)時，他們必須先向同盟伺服器進行驗證。 驗證的方法之一是讓公司網路客戶透過 Windows 整合式驗證存取本機同盟伺服器。  
  
## <a name="configure-corporate-dns"></a>設定公司 DNS  
為了讓本機同盟伺服器上的 Windows 整合式驗證能夠成功進行名稱解析，必須為新主機設定帳戶夥伴的公司網路中的網域名稱系統 \(DNS\)，\(\) 資源記錄，將同盟伺服器的完整功能變數名稱 \(FQDN\) 主機名稱解析為同盟伺服器叢集的 IP 位址。  
  
在下圖中，您可以看到特定案例如何完成這項工作。 在此案例中，Microsoft 網路負載平衡 \(NLB\) 會為現有的同盟伺服器陣列提供單一叢集 FQDN 名稱和單一叢集 IP 位址。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
如需有關如何使用 NLB 設定叢集 IP 位址或叢集 FQDN 的詳細資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
如需有關如何為同盟伺服器設定公司 DNS 的詳細資訊，請參閱[將&#40;主機&#41; a 資源記錄新增至同盟伺服器的公司 dns](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
如需有關如何在周邊網路中設定同盟伺服器 proxy 的詳細資訊，請參閱[同盟伺服器 proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
