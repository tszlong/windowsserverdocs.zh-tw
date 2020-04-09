---
title: 步驟4規劃遠端存取服務器上的 OTP
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 789fd72e2f3fc1693bf4803f33dcc1e7f1b3acc3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855771"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>步驟4規劃遠端存取服務器上的 OTP

>適用於：Windows Server (半年通道)、Windows Server 2016

規劃一次性密碼（OTP） RADIUS 伺服器和憑證設定之後，規劃遠端存取 OTP 部署的最後一個步驟是在遠端存取服務器上規劃用戶端 OTP 設定。  
  
|工作|描述|  
|----|--------|  
|[4.1 規劃 OTP 用戶端豁免](#bkmk_4_1_Exemptions)|為不需要使用 OTP 進行驗證的使用者規劃豁免。|  
|[適用于 Windows 7 用戶端的4.2 規劃](#bkmk_4_2_Win7)|規劃將 DirectAccess 連線助理（DCA）2.0 部署到 Windows 7 用戶端電腦。|  
|[4.3 規劃智慧卡](#BKMK_smartcard)|規劃使用智慧卡進行額外的授權。|  
  
## <a name="41-plan-for-otp-client-exemptions"></a><a name="bkmk_4_1_Exemptions"></a>4.1 規劃 OTP 用戶端豁免  
啟用 OTP 驗證時，根據預設，所有使用者都必須使用使用者名稱和密碼的組合，以及 OTP 認證來進行驗證。 不過，您可以允許選取的使用者僅使用使用者名稱和密碼來進行驗證，而不需要 OTP。 若要這麼做，請建立安全性群組，並新增任何您想要豁免 OTP 驗證的使用者。  
  
> [!NOTE]  
> 只有來自單一樹系的用戶端電腦可能會被豁免，因為只有一個安全性群組可以選取來進行用戶端豁免。  
  
## <a name="42-plan-for-windows-7-clients"></a><a name="bkmk_4_2_Win7"></a>適用于 Windows 7 用戶端的4.2 規劃  
根據預設，Windows 7 用戶端電腦無法使用 OTP 進行驗證。  Windows 7 用戶端電腦需要有 DCA 2.0，才能在 Windows Server 2012 遠端存取部署中使用 OTP 進行驗證。 如需有關 DCA 2.0 的詳細資訊，請參閱 Microsoft 下載中心上的[DirectAccess 連線助理 2.0](https://go.microsoft.com/fwlink/?LinkId=253699) 。  
  
## <a name="43-plan-for-smart-cards"></a><a name="BKMK_smartcard"></a>4.3 規劃智慧卡  
啟用 OTP 驗證時，可以使用啟用智慧卡以取得額外授權的選項。 如果使用者的智慧卡無法正常運作，請建立安全性群組來允許暫時存取。  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱  
  
-   [使用 OTP 驗證設定 DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


