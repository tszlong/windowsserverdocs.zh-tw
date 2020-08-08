---
title: 伺服器憑證部署
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 09b5ec45562a2ecd60c8ab1a753b9d917b6cdfd9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949332"
---
# <a name="server-certificate-deployment"></a>伺服器憑證部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

請遵循下列步驟來安裝企業根憑證授權單位 (CA) 並部署要與 PEAP 和 EAP 搭配使用的伺服器憑證。

> [!IMPORTANT]
> 在安裝 Active Directory 憑證服務之前，您必須先將電腦命名為，並將電腦設定為使用靜態 IP 位址，並將電腦加入網域。 安裝 AD CS 之後，您就無法變更電腦名稱稱或電腦的網域成員資格，不過，您可以視需要變更 IP 位址。 如需如何完成這些工作的詳細資訊，請參閱《 Windows Server &reg; 2016[核心網路指南》](../../Core-Network-Guide.md)。


-   [安裝網頁伺服器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)

-   [在 DNS 中為 WEB1 建立別名 (CNAME) 記錄](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)

-   [設定 WEB1 以發佈憑證撤銷清單 (Crl) ](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)

-   [準備 Capolicy.inf inf 檔案](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)

-   [安裝憑證授權單位](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)

-   [在 CA1 上設定 CDP 和 AIA 延伸模組](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)

-   [將 CA 憑證與 CRL 複製到虛擬目錄](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)

-   [設定伺服器憑證範本](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)

-   [設定電腦憑證自動註冊](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)

-   [重新整理群組原則](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)

-   [驗證伺服器憑證的伺服器註冊](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)

> [!NOTE]
> 本指南中的程式不包含開啟 [**使用者帳戶控制**] 對話方塊來要求您的許可權以繼續的情況的指示。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]****。



