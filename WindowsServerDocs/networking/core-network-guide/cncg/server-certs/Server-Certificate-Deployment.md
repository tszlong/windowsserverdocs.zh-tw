---
title: 伺服器憑證部署
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 751c5c5958b3d06ae0f4b701e4d6e10a7fef19dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858489"
---
# <a name="server-certificate-deployment"></a>伺服器憑證部署

>適用於：Windows Server （半年通道），Windows Server 2016

遵循下列步驟來安裝企業根憑證授權單位 (CA)，並部署伺服器以搭配 PEAP 與 EAP 的憑證。  
  
> [!IMPORTANT]  
> 在安裝 Active Directory 憑證服務之前，您必須電腦命名為、 使用靜態 IP 位址，設定電腦和加入網域的電腦。 安裝 AD CS 之後，無法變更電腦名稱或電腦的網域成員資格，但您可以視需要變更 IP 位址。 如需有關如何完成這些工作的詳細資訊，請參閱 Windows Server&reg; 2016年[核心網路指南](../../Core-Network-Guide.md)。  

  
-   [安裝 Web 伺服器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [在 DNS 中建立的別名 (CNAME) 記錄，如 WEB1](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [設定 WEB1 以發佈憑證撤銷清單 (Crl)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [準備 CAPolicy inf 檔案](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [安裝憑證授權單位](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [CA1 上設定的 CDP 和 AIA 延伸模組](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [將 CA 憑證與 CRL 複製到虛擬目錄](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [設定伺服器憑證範本](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [設定伺服器憑證自動註冊](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [重新整理群組原則](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [確認伺服器憑證的伺服器註冊](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> 本指南中的程序不在其中包含案例的指示**使用者帳戶控制**對話方塊隨即開啟，要求您的權限，才能繼續。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]。  
  


