---
title: 使用 OTP 驗證規劃遠端存取
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署「遠端存取」指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e4b171c628259abe59a6954ef9936b025f10c28e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950274"
---
# <a name="plan-remote-access-with-otp-authentication"></a>使用 OTP 驗證規劃遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務 (RRAS) VPN 合併為單一遠端存取角色。 本總覽提供部署單一 Windows Server 2016 或 Windows Server 2012 遠端存取多網站部署所需的設定步驟簡介。


-  步驟1： [部署具有 Advanced Settings 的單一 DirectAccess 伺服器](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md)。 此步驟包含部署單一伺服器所需的基礎結構規劃。 其中包括規劃網路和伺服器設定、憑證需求、DNS 設定、網路位置伺服器部署、DirectAccess 管理伺服器、Active Directory 設定，以及 (Gpo) 的群組原則物件。

-   [步驟2：規劃 RADIUS 伺服器部署](Step-2-Plan-the-RADIUS-Server-Deployment.md)

-   [步驟3：規劃 OTP 憑證部署](Step-3-Plan-OTP-Certificate-Deployment.md)

-   [步驟4：在遠端存取服務器上規劃 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)

完成這些規劃步驟之後，請參閱 [使用 OTP 驗證設定遠端存取](../configure/configure-ra-with-otp-authentication.md)。 如需在實驗室環境中將多網站部署設定為概念證明的詳細資訊，請參閱 [測試實驗室指南：示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md)。

