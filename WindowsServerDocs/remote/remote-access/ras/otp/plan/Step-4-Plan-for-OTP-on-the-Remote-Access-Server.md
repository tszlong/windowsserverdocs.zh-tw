---
title: 在遠端存取服務器上規劃 OTP 的步驟4
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署「遠端存取」指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 682c6f7bd829ccdf208271196e5b0b27d660515a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948204"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>在遠端存取服務器上規劃 OTP 的步驟4

>適用於：Windows Server (半年度管道)、Windows Server 2016

在規劃單次密碼 (OTP) RADIUS 伺服器和憑證設定之後，規劃遠端存取 OTP 部署的最後一個步驟是在遠端存取服務器上規劃用戶端 OTP 設定。

|Task|描述|
|----|--------|
|[4.1 規劃 OTP 用戶端豁免](#bkmk_4_1_Exemptions)|針對您不需要使用 OTP 進行驗證的使用者，規劃豁免。|
|[適用于 Windows 7 用戶端的4.2 方案](#bkmk_4_2_Win7)|規劃將 DirectAccess 連線助理 (DCA) 2.0 部署至 Windows 7 用戶端電腦。|
|[4.3 規劃智慧卡](#BKMK_smartcard)|規劃使用智慧卡進行額外授權。|

## <a name="41-plan-for-otp-client-exemptions"></a><a name="bkmk_4_1_Exemptions"></a>4.1 規劃 OTP 用戶端豁免
啟用 OTP 驗證時，根據預設，所有使用者都必須使用使用者名稱和密碼，以及 OTP 認證的組合進行驗證。 不過，您可以允許選取的使用者僅使用使用者名稱和密碼進行驗證，而不需要 OTP。 若要這樣做，請建立安全性群組，並新增任何您想要豁免于 OTP 驗證的使用者。

> [!NOTE]
> 只有單一樹系中的用戶端電腦可能會被豁免，因為這表示用戶端豁免只可選取一個安全性群組。

## <a name="42-plan-for-windows-7-clients"></a><a name="bkmk_4_2_Win7"></a>適用于 Windows 7 用戶端的4.2 方案
根據預設，Windows 7 用戶端電腦無法使用 OTP 進行驗證。  Windows 7 用戶端電腦需要 DCA 2.0，才能在 Windows Server 2012 遠端存取部署中使用 OTP 進行驗證。 如需有關 DCA 2.0 的詳細資訊，請參閱 Microsoft 下載中心上的 [DirectAccess Connectivity Assistant 2.0](https://go.microsoft.com/fwlink/?LinkId=253699) 。

## <a name="43-plan-for-smart-cards"></a><a name="BKMK_smartcard"></a>4.3 規劃智慧卡
啟用 OTP 驗證時，可使用啟用智慧卡以取得額外授權的選項。 建立安全性群組，以允許使用者的智慧卡無法正常運作時的暫時存取。

## <a name="see-also"></a><a name="BKMK_Links"></a>請參閱

-   [使用 OTP 驗證設定 DirectAccess](../deploy-ra-otp.md)

