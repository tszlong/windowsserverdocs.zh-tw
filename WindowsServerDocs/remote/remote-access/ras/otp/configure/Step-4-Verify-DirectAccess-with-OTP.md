---
title: 步驟4使用 OTP 驗證 DirectAccess
description: 瞭解如何使用 OTP 部署來確認您已正確設定 DirectAccess。
manager: brianlic
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 91bc9f3ebb03d45fa221673b88f2473aa774544e
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040188"
---
# <a name="step-4-verify-directaccess-with-otp"></a>步驟4使用 OTP 驗證 DirectAccess

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何使用 OTP 部署來確認您已正確設定 DirectAccess。

### <a name="to-verify-otp-health-on-the-remote-access-server"></a>確認遠端存取服務器上的 OTP 健康情況

1. 在 [遠端存取] 伺服器上，開啟 [ **遠端存取管理** ] 主控台。

2. 在 [ **遠端存取服務器** ] 下，按一下已設定 OTP 支援的遠端存取服務器。

3. 按一下 [ **作業狀態**]。

4. 確認 OTP 的狀態顯示綠色圖示且正在運作。

    > [!NOTE]
    > 健全狀況狀態更新間隔將會是從登錄機碼 HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout 的值總和，以及遠端存取設定中設定的 **伺服器啟用時間間隔** 的最大值。

### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>使用 OTP 驗證驗證內部資源的存取權

1.  將 DirectAccess 用戶端電腦連線到公司網路，並從命令提示字元執行 **gpupdate/force** 以取得群組原則。

2.  中斷用戶端電腦與公司網路的連線，連接到外部網路，然後嘗試存取內部資源。 您不應該擁有內部資源的存取權。

3.  如果是軟體權杖，請使用廠商指示存取 OTP 用戶端權杖，並記下目前的權杖程式碼。 使用硬體權杖時，請遵循廠商指示以進行驗證。

4.  按一下通知區域中的 [網路連線] 圖示來存取 DA 媒體管理員。

5.  按一下 [ **DirectAccess 連接**]，然後按一下 [ **繼續**]。

6.  輸入先前記下的權杖碼，然後按一下 **[確定]**。 等候驗證完成。 DirectAccess Workplace 線上狀態現在將會 **連接**。

7.  嘗試存取內部資源。 您應該能夠存取所有公司資源。



