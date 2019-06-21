---
title: 步驟 4 計劃遠端存取伺服器上 OTP
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a22d58e8e775ae341691cab1f5b40f8121ea533f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282377"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>步驟 4 計劃遠端存取伺服器上 OTP

>適用於：Windows Server （半年通道），Windows Server 2016

規劃單次密碼 (OTP) RADIUS 伺服器和憑證設定之後, 規劃遠端存取 OTP 部署的最後一個步驟就是規劃 「 遠端存取 」 伺服器上的用戶端 OTP 設定。  
  
|工作|描述|  
|----|--------|  
|[4.1 規劃 OTP 用戶端的豁免](#bkmk_4_1_Exemptions)|您不需要使用 OTP 驗證的使用者豁免的規劃。|  
|[4.2 規劃 Windows 7 用戶端](#bkmk_4_2_Win7)|計劃將 DirectAccess 連線助理 (DCA) 2.0 部署至 Windows 7 用戶端電腦。|  
|[4.3 規劃智慧卡](#BKMK_smartcard)|規劃使用智慧卡進行額外的授權。|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 規劃 OTP 用戶端的豁免  
啟用 OTP 驗證時，根據預設，所有使用者都需要使用使用者名稱和密碼，以及 OTP 認證的組合進行驗證。 不過，您可以允許選取的使用者，而 OTP 不使用使用者名稱和密碼，來進行驗證。 若要這樣做，請建立安全性群組並加入您想要的任何使用者豁免使用 OTP 驗證。  
  
> [!NOTE]  
> 僅來自單一樹系的用戶端電腦，可能會因為以下事實免套用的用戶端的豁免，可以選取該只有一個安全性群組。  
  
## <a name="bkmk_4_2_Win7"></a>4.2 規劃 Windows 7 用戶端  
根據預設，Windows 7 用戶端電腦無法使用 OTP 驗證的。  Windows 7 用戶端電腦需要 DCA 2.0，以在 Windows Server 2012 遠端存取部署使用 OTP 進行驗證。 如需 DCA 2.0 的詳細資訊，請參閱[DirectAccess 連線助理 （jlca) 2.0](https://go.microsoft.com/fwlink/?LinkId=253699) Microsoft Download Center 上。  
  
## <a name="BKMK_smartcard"></a>4.3 規劃智慧卡  
啟用 OTP 驗證時，此選項可啟用額外的授權使用智慧卡使用。 建立以使用者的智慧卡未運作時允許暫時存取權的安全性群組。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [設定 DirectAccess 使用 OTP 驗證](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


