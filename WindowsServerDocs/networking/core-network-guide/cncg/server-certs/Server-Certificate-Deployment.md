---
title: 伺服器憑證部署
description: 瞭解您必須執行的步驟，以安裝企業根憑證授權單位，以及部署用於 PEAP 和 EAP 的伺服器憑證。
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: aaa2f2e780a088e0fc321fb53d3d710df449616b
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038588"
---
# <a name="server-certificate-deployment"></a>伺服器憑證部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

請遵循下列步驟來安裝企業根憑證授權單位 (CA) ，以及部署伺服器憑證以用於 PEAP 和 EAP。

> [!IMPORTANT]
> 安裝 Active Directory 憑證服務之前，您必須先命名電腦、使用靜態 IP 位址設定電腦，並將電腦加入網域。 安裝 AD CS 之後，您就無法變更電腦名稱稱或電腦的網域成員資格，不過，您可以視需要變更 IP 位址。 如需如何完成這些工作的詳細資訊，請參閱《 Windows Server &reg; 2016 [核心網路指南》](../../Core-Network-Guide.md)。


-   [安裝網頁伺服器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)

-   [在 DNS 中為 WEB1 建立別名 (CNAME) 記錄](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)

-   [將 WEB1 設定為將憑證撤銷清單散發 (Crl) ](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)

-   [準備 Capolicy.inf inf 檔案](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)

-   [安裝憑證授權單位](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)

-   [在 CA1 上設定 CDP 和 AIA 延伸模組](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)

-   [將 CA 憑證與 CRL 複製到虛擬目錄](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)

-   [設定伺服器憑證範本](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)

-   [設定電腦憑證自動註冊](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)

-   [重新整理群組原則](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)

-   [確認伺服器憑證的伺服器註冊](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)

> [!NOTE]
> 本指南中的程式不包含 [ **使用者帳戶控制** ] 對話方塊開啟時，要求您的許可權以繼續執行的指示。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]。



