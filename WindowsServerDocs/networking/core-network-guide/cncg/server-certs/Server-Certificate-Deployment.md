---
title: 伺服器的憑證部署
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 1ae4384b-f4e4-41e8-bc5f-9ac41953bca4
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a10a9bafa6a8c9fddecac799ec8e837bf339d0e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment"></a>伺服器的憑證部署

>適用於：Windows Server（以每年次管道）、Windows Server 2016

請依照下列步驟來安裝企業版根憑證授權單位及部署 PEAP 與 EAP 使用伺服器的憑證。  
  
> [!IMPORTANT]  
> Active Directory 憑證服務安裝之前，您必須為電腦、 靜態 IP 位址，以設定電腦和加入網域的電腦。 AD CS 安裝之後，您無法變更的電腦名稱或網域的電腦，但是您可以視需要變更的 IP 位址。 如需如何完成這些工作，查看 Windows Server&reg; 2016年[核心網路指南](../../Core-Network-Guide.md)。  

  
-   [安裝網頁伺服器 WEB1](../../../core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)  
  
-   [在 [DNS 建立 WEB1 別名 (CNAME) 記錄](../../../core-network-guide/cncg/server-certs/Create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)  
  
-   [設定 WEB1 散發憑證撤銷列出 (Crl)](../../../core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-Lists.md)  
  
-   [準備 CAPolicy inf 檔案](../../../core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)  
  
-   [安裝憑證授權單位](../../../core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)  
  
-   [設定 CA1 CDP 和 AIA 擴充功能](../../../core-network-guide/cncg/server-certs/Configure-the-CDP-and-AIA-Extensions-on-CA1.md)  
  
-   [將憑證及 CRL 複製 virtual directory](../../../core-network-guide/cncg/server-certs/Copy-the-CA-Certificate-and-CRL-to-the-Virtual-Directory.md)  
  
-   [設定伺服器的憑證範本](../../../core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)  
  
-   [設定伺服器認證自動授權](../../../core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)  
  
-   [重新整理群組原則](../../../core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)  
  
-   [請確認伺服器註冊伺服器的憑證](../../../core-network-guide/cncg/server-certs/Verify-Server-Enrollment-of-a-Server-Certificate.md)  
  
> [!NOTE]  
> 本文中的程序中不包含案例指示**使用者 Account 控制項**對話方塊要求您的權限才能繼續。 如果此對話方塊同時執行程序本指南，如果對話方塊一個因應以您的動作，按一下 [**繼續**。  
  


