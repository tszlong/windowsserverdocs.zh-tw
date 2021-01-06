---
title: 在企業中部署遠端存取
description: 本主題提供適用于企業的 Windows Server 2016 中 DirectAccess 案例的簡介。
manager: brianlic
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8da7f989bec6a0a14a87d503afb976459a073d94
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947404"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>在企業中部署遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題為企業提供 DirectAccess 案例的簡介。


> [!IMPORTANT]
> 若要使用本指南來部署 DirectAccess，您必須使用執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的 DirectAccess 伺服器。

## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在開始部署之前，請參閱不支援的設定清單、已知問題和先決條件

-   [DirectAccess 不支援的設定](../directaccess/directaccess-unsupported-configurations.md)

-   [DirectAccess 已知問題](../directaccess/directaccess-known-issues.md)

-   [部署 DirectAccess) 必要條件的必要條件](../directaccess/prerequisites-for-deploying-directaccess.md)

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述
「遠端存取」包含一些企業功能，其中包括在利用 Windows 網路負載平衡 (NLB) 或外部負載平衡器平均分攤工作量的叢集中，部署多個遠端存取伺服器、為分散各地的遠端存取伺服器設定多站台部署，以及利用一次性密碼 (OTP) 部署雙因素用戶端驗證的 DirectAccess。

## <a name="in-this-scenario"></a>在這個案例中
每一個企業案例都會有一份相關文件加以說明並提供計劃和部署步驟。 如需詳細資訊，請參閱

-   [在叢集中部署遠端存取](cluster/Deploy-Remote-Access-In-Cluster.md)

-   [以多站台部署方式部署多個遠端存取伺服器](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)

-   [使用 OTP 驗證部署遠端存取](otp/Deploy-RA-OTP.md)

-   [在多樹系環境中部署遠端存取](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
遠端存取企業案例提供下優點：

-   **提高可用性**。 在叢集中部署多個遠端存取服務器可提供擴充性，並增加輸送量和使用者數目的容量。 讓叢集負載平衡可以提高可用性。 如果叢集中有一部伺服器當機，遠端使用者可以透過叢集中的其他伺服器，繼續連接公司內部網路。 因為用戶端使用虛擬 IP (VIP) 位址連接叢集，所以不會察覺容錯移轉的進行。

-   **易於管理**。 您可以使用在其中一個叢集伺服器上執行的遠端存取管理主控台，將叢集或多網站部署設定及管理為單一實體。 此外，多站台部署方式可以讓系統管理員根據 Active Directory 網站的數量，部署「遠端存取」，提供一種簡化的架構。 共用的設定可以輕鬆設定到各個叢集伺服器或所有多站台進入點伺服器。 「遠端存取設定」可以從叢集或部署中的任何伺服器進行管理，或者利用遠端伺服器管理工具 (RSAT) 從遠端管理。 此外，整個叢集或部署的多站台可以從單個「遠端存取管理」主控台進行監視。

-   **成本效益**。 遠端存取多網站部署可讓企業在與用戶端位置對應的多個網站中部署遠端存取服務器。 這樣一來，無論遠端用戶端所在的位置為何，都可以為它們提供一種可預測的連接方式，另外將用戶端使用的網際網路流量，改由最近的遠端存取伺服器負責傳送，藉此降低成本和內部網路頻寬。

-   **安全性**。 使用一次性密碼 (OTP) 來部署強式用戶端驗證，而不是標準 Active Directory 密碼會提高安全性。

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的角色與功能
下表列出本企業案例使用的角色和功能：

|角色/功能|如何支援本案例|
|---------|-----------------|
|遠端存取伺服器角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<p>1. DirectAccess 與路由及遠端存取服務 (RRAS) VPN-DirectAccess 和 VPN 會在 [遠端存取管理] 主控台中一起管理。<br />2. 在舊版路由和遠端存取主控台中管理 RRAS 路由 RRAS 的路由功能。<p>遠端存取伺服器角色需要以下伺服器功能：<p>-Internet Information Services (IIS) -設定網路位置伺服器和預設 web 探查需要這項功能。<br />-群組原則管理主控台功能-DirectAccess 需要功能來建立和管理 Active Directory 中的群組原則物件 (Gpo) ，而且必須安裝為伺服器角色的必要功能。|
|遠端存取管理工具功能|這個功能的安裝方式如下：<p>-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<p>遠端存取管理工具功能包含以下各項：<p>1. 遠端存取 GUI 和命令列工具<br />2. Windows PowerShell 的遠端存取模組<p>依存項目包括：<p>1. 群組原則管理主控台<br />2. RAS 連線管理員系統管理組件 (CMAK) <br />3. Windows PowerShell 3。0<br />4. 圖形化管理工具和基礎結構|
|Windows NLB|這個功能可以平均分攤多個遠端存取伺服器的工作量。|



