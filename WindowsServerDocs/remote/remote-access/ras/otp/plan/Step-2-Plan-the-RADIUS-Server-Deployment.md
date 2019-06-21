---
title: 步驟 2 計劃 RADIUS 伺服器部署
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f74a83c3962c7accd76fbf07307216742ada863d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280824"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>步驟 2 計劃 RADIUS 伺服器部署

>適用於：Windows Server （半年通道），Windows Server 2016

部署單一遠端存取伺服器之後, 計劃的單次密碼 (OTP) 驗證伺服器。  
  
|工作|描述|  
|----|--------|  
|2.1 計劃 RADIUS 伺服器|OTP 驗證伺服器、 Windows Server 2016 和 Windows Server 2012 中的遠端存取支援任何支援的密碼驗證通訊協定 (PAP) 的已啟用 RADIUS 的 OTP 伺服器。|  
  
## <a name="BKMK_1.1"></a>2.1 計劃 RADIUS 伺服器  
規劃 RADIUS 伺服器進行 OTP 驗證時，請注意下列：  
  
-   對於大部分的 OTP 部署類型，您必須設定遠端存取伺服器做為 RADIUS 代理程式。 如需詳細資訊，請參閱 OTP 廠商的文件。  
  
-   對於所有的 OTP 部署，您必須同步您的 Active Directory 使用者與您的 RADIUS 伺服器。  
  
-   RADIUS 伺服器不必是網域成員。  
  
-   當您部署 RADIUS 伺服器時，您可以設定共用的密碼和用於 RADIUS 流量的連接埠號碼。 請記下的這些詳細資料;當您設定遠端存取伺服器時，它們是必要的。  
  
您可以檢視設定使用 RSA SecurID 伺服器中的 OTP 驗證的範例測試實驗室指南[測試實驗室指南：示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid)。  
  
  
  


