---
title: 在企業中部署遠端存取
description: 本主題提供適用於企業的 Windows Server 2016 中的 DirectAccess 案例簡介。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cbcc844c356978f5bb5f34b66aa36dec9b163c1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283008"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>在企業中部署遠端存取

>適用於：Windows Server （半年通道），Windows Server 2016

本主題為企業提供 DirectAccess 案例的簡介。  
  
  
> [!IMPORTANT]  
> 若要部署使用本指南的 DirectAccess，您必須使用執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的 DirectAccess 伺服器。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在開始部署之前，請參閱不支援的設定清單、已知問題和先決條件  
  
-   [DirectAccess 不支援的設定](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [DirectAccess 已知問題](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [部署 DirectAccess 的必要條件） 必要條件](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="BKMK_OVER"></a>案例描述  
「遠端存取」包含一些企業功能，其中包括在利用 Windows 網路負載平衡 (NLB) 或外部負載平衡器平均分攤工作量的叢集中，部署多個遠端存取伺服器、為分散各地的遠端存取伺服器設定多站台部署，以及利用一次性密碼 (OTP) 部署雙因素用戶端驗證的 DirectAccess。  
  
## <a name="in-this-scenario"></a>在這個案例中  
每一個企業案例都會有一份相關文件加以說明並提供計劃和部署步驟。 如需詳細資訊，請參閱：  
  
-   [在叢集中部署遠端存取](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [以多站台部署方式部署多個遠端存取伺服器](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [使用 OTP 驗證部署遠端存取](otp/Deploy-RA-OTP.md)  
  
-   [部署在多樹系環境中的遠端存取](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="BKMK_APP"></a>實際的應用程式  
遠端存取企業案例提供下優點：  
  
-   **提高可用性**。 部署在叢集中的多部遠端存取伺服器可以提高延展性並容納的輸送量和使用者數目。 讓叢集負載平衡可以提高可用性。 如果叢集中有一部伺服器當機，遠端使用者可以透過叢集中的其他伺服器，繼續連接公司內部網路。 因為用戶端使用虛擬 IP (VIP) 位址連接叢集，所以不會察覺容錯移轉的進行。  
  
-   **管理容易**。 可以設定叢集或多站台部署，並作為單一實體，使用一部叢集伺服器上執行遠端存取管理主控台進行管理。 此外，多站台部署方式可以讓系統管理員根據 Active Directory 網站的數量，部署「遠端存取」，提供一種簡化的架構。 共用的設定可以輕鬆設定到各個叢集伺服器或所有多站台進入點伺服器。 「遠端存取設定」可以從叢集或部署中的任何伺服器進行管理，或者利用遠端伺服器管理工具 (RSAT) 從遠端管理。 此外，整個叢集或部署的多站台可以從單個「遠端存取管理」主控台進行監視。  
  
-   **成本效益**。 多站台部署遠端存取可讓部署對應至用戶端位置的多個站台中的遠端存取伺服器的企業。 這樣一來，無論遠端用戶端所在的位置為何，都可以為它們提供一種可預測的連接方式，另外將用戶端使用的網際網路流量，改由最近的遠端存取伺服器負責傳送，藉此降低成本和內部網路頻寬。  
  
-   **安全性**。 部署增強式用戶端驗證，而不是標準的 Active Directory 密碼的一次性密碼 (OTP)，可提高安全性。  
  
## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出本企業案例使用的角色和功能：  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取伺服器角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<br /><br />1.DirectAccess 與路由及遠端存取服務 (RRAS) VPN-管理 DirectAccess 和 VPN 一起在遠端存取管理主控台中。<br />2.在舊版的路由及遠端存取主控台中管理 RRAS 路由-RRAS 路由功能。<br /><br />遠端存取伺服器角色需要以下伺服器功能：<br /><br />為 Internet Information Services (IIS)-這項功能，才能設定網路位置伺服器和預設 web 探查。<br />-群組原則管理主控台功能-功能所建立和管理群組原則物件 (Gpo) 在 Active Directory 中的 DirectAccess，必須安裝為伺服器角色的必要功能。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它預設會安裝在遠端存取伺服器時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面。<br />-它可以選擇性地安裝在不執行 遠端存取伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />1.遠端存取 GUI 和命令列工具<br />2.適用於 Windows PowerShell 的遠端存取模組<br /><br />依存項目包括：<br /><br />1.群組原則管理主控台<br />2.RAS 連線管理員系統管理組件 (CMAK)<br />3.Windows PowerShell 3.0<br />4.圖形化管理工具與基礎結構|  
|Windows NLB|這個功能可以平均分攤多個遠端存取伺服器的工作量。|  
  

  


