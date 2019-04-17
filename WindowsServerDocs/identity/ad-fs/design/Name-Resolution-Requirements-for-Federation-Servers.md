---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: "聯盟伺服器的名稱解析需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>聯盟伺服器的名稱解析需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當 client 公司網路上的電腦嘗試存取應用程式或 Web 服務所保護的 Active Directory 同盟服務 \(AD FS\) 時，他們就必須先驗證聯盟伺服器。 已經透過 Windows 整合式驗證存取伺服器本機聯盟公司網路戶端是一種方式驗證。  
  
## <a name="configure-corporate-dns"></a>設定公司 DNS  
可能是透過 Windows 整合式驗證聯盟本機伺服器成功名稱解析度，必須設定網域名稱系統 \(DNS\) account 合作夥伴公司網路中的新主機 \(A\) 資源將會聯盟 server 的完整的網域名稱 \(FQDN\) 主機名稱解析聯盟叢集伺服器的 IP 位址。  
  
下圖，您可以看到這項工作特定案例的方式來完成。 在本案例中，Microsoft 網路負載平衡 \(NLB\) 提供的單一叢集 FQDN 名稱和單一叢集 IP 位址的現有聯盟伺服器發電廠。  
  
![名稱需求](media/adfs2_deploy_single_fs.gif)  
  
了解如何設定叢集 IP 位址或叢集 FQDN 使用 NLB 資訊，請查看[指定叢集參數](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
了解如何設定公司 DNS 聯盟伺服器的資訊，請查看[新增主機和 #40;A 與 #41;企業的 DNS 伺服器聯盟資源記錄](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)。  
  
如需有關如何聯盟的 proxy 伺服器設定周邊網路中的資訊，[聯盟的 Proxy 伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  

## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
