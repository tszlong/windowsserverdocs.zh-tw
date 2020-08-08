---
title: 步驟4使用 OTP 驗證 DirectAccess
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 69b6eb36e888df99888d3dbb4ad2992ef0a51606
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969025"
---
# <a name="step-4-verify-directaccess-with-otp"></a>步驟4使用 OTP 驗證 DirectAccess

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何使用 OTP 部署來驗證您的 DirectAccess 是否已正確設定。

### <a name="to-verify-otp-health-on-the-remote-access-server"></a>確認遠端存取服務器上的 OTP 健全狀況

1. 在 [遠端存取] 伺服器上，開啟 [**遠端存取管理**] 主控台。

2. 在 [**遠端存取服務器**] 下，按一下已設定 OTP 支援的遠端存取服務器。

3. 按一下 [**作業狀態**]。

4. 確認 OTP 的狀態會顯示綠色圖示且正在運作。

    > [!NOTE]
    > [健全狀況狀態] 更新間隔會是登錄機碼 HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout 中的值總和，以及遠端存取設定中設定之**伺服器活動的時間間隔**上限。

### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>使用 OTP 驗證來驗證內部資源的存取權

1.  將 DirectAccess 用戶端電腦連線到公司網路，並從命令提示字元執行**gpupdate/force**來取得群組原則。

2.  中斷用戶端電腦與公司網路的連線、連線到外部網路，並嘗試存取內部資源。 您不應該具有內部資源的存取權。

3.  在軟體權杖的情況下，請使用廠商指示來存取 OTP 用戶端權杖，並記下目前的權杖代碼。 使用硬體權杖時，請遵循廠商指示來進行驗證。

4.  按一下通知區域中的 [網路連線]**** 圖示來存取 DA 媒體管理員。

5.  按一下 [ **DirectAccess**連線]，然後按一下 [**繼續**]。

6.  輸入先前所述的權杖代碼，然後按一下 **[確定]**。 等待驗證完成。 DirectAccess 工作場所線上狀態現在將會**連接**。

7.  嘗試存取內部資源。 您應該能夠存取所有公司資源。



