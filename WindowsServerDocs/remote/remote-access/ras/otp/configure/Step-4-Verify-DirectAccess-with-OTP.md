---
title: 步驟 4： 確認 DirectAccess OTP 與
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcc152723ee339c8652c9480647bfb8f438c87d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813239"
---
# <a name="step-4-verify-directaccess-with-otp"></a>步驟 4： 確認 DirectAccess OTP 與

>適用於：Windows Server （半年通道），Windows Server 2016

本主題描述如何確認已正確設定您的 DirectAccess OTP 部署。
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>若要確認遠端存取伺服器上的 OTP 健全狀況

1. 在開啟的遠端存取伺服器上**遠端存取管理**主控台。  

2. 底下**遠端存取伺服器**按一下 OTP 支援已設定的遠端存取伺服器。  

3. 按一下 **操作狀態**。  

4. 請確認 OTP 的狀態會顯示綠色圖示，以及作用中。  
  
    > [!NOTE]  
    > 健全狀況狀態的更新間隔會從登錄機碼 HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout 值總和的最大值和**伺服器活動的時間間隔**中設定的遠端存取設定。  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>若要確認使用 OTP 驗證的內部資源的存取權  
  
1.  DirectAccess 用戶端電腦連線到公司網路，並執行**gpupdate /force**從命令提示字元，以取得群組原則。  
  
2.  中斷用戶端電腦從公司網路，連接到外部網路中，而且嘗試存取內部資源。 您不應該有存取內部資源。  
  
3.  如果軟體權杖，存取 OTP 用戶端權杖使用廠商的指示，並記下目前權杖的程式碼。 使用硬體權杖時，請依照廠商指示來進行驗證。  
  
4.  按一下通知區域中的 [網路連線]  圖示來存取 DA 媒體管理員。  
  
5.  按一下  **DirectAccess 連線**，然後按一下**繼續**。  
  
6.  輸入先前，記下的語彙基元的程式碼，然後按一下**確定**。 等候要完成驗證。 DirectAccess 「 工作地方連線狀態就會變成**Connected**。  
  
7.  嘗試存取內部資源。 您應該能夠存取所有公司資源。  
  


