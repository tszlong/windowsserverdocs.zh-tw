---
title: 步驟2規劃 RADIUS 伺服器部署
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 44f8514bd046d2c5f526e85309af430383bf1ec1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313572"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>步驟2規劃 RADIUS 伺服器部署

>適用於：Windows Server (半年通道)、Windows Server 2016

部署單一遠端存取服務器之後，請規劃一次性密碼（OTP）驗證服務器。  
  
|工作|描述|  
|----|--------|  
|2.1 規劃 RADIUS 伺服器|在 OTP 驗證服務器中，Windows Server 2016 和 Windows Server 2012 中的遠端存取支援支援密碼驗證通訊協定（PAP）的任何啟用 RADIUS 的 OTP 伺服器。|  
  
## <a name="21-plan-the-radius-server"></a><a name="BKMK_1.1"></a>2.1 規劃 RADIUS 伺服器  
針對 OTP 驗證規劃 RADIUS 伺服器時，請注意下列事項：  
  
-   針對大部分的 OTP 部署類型，您必須將遠端存取服務器設定為 RADIUS 代理程式。 如需詳細資訊，請參閱 OTP 廠商檔。  
  
-   對於所有 OTP 部署，您必須將 Active Directory 使用者與 RADIUS 伺服器同步。  
  
-   RADIUS 伺服器不需要是網域成員。  
  
-   當您部署 RADIUS 伺服器時，您會設定共用密碼和 RADIUS 流量的埠號碼。 記下這些詳細資料;當您設定遠端存取服務器時，這是必要的。  
  
您可以在[測試實驗室指南：示範使用 otp 驗證的 DirectAccess 和 Rsa securid](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid)中，查看使用 RSA securid 伺服器設定 OTP 驗證的範例測試實驗室指南。  
  
  
  


