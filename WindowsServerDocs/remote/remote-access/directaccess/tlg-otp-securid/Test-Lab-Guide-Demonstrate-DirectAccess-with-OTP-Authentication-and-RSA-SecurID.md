---
title: 測試實驗室指南： 示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess
description: 本主題是測試實驗室指南-使用 OTP 驗證和 RSA SecurID for Windows Server 2016 的示範 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6e4b03ceb331899ac622bffe44061a7d92f5a09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866229"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>測試實驗室指南：示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess

>適用於：Windows Server （半年通道），Windows Server 2016

遠端存取是 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 作業系統中，可讓遠端使用者可以安全地存取內部網路資源使用路由中的 DirectAccess 或虛擬私人網路 (Vpn) 伺服器角色與遠端存取服務 (RRAS)。 本指南包含逐步解說以擴充[測試實驗室指南：示範 DirectAccess 單一伺服器安裝使用混合的 IPv4 與 IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004)示範遠端存取的單次密碼 (OTP) 設定。  
  
> [!WARNING]  
> 這個測試實驗室指南的設計，包括基礎結構伺服器，例如網域控制站和執行 Windows Server 2012 R2 或 Windows Server 2012 的憑證授權單位 (CA)。 使用這個測試實驗室指南設定執行其他作業系統的基礎結構伺服器尚未經過測試，並設定其他作業系統的指示不包含在本指南中。  
  
## <a name="about-this-guide"></a>關於本指南  
在 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的遠端存取新增 OTP 用戶端驗證的支援。 這個測試實驗室的目的只是 RSA SecurID 用來示範 OTP 功能與遠端存取。 其他 RADIUS 基礎 OTP 解決方案，支援，但是這些範圍以外的這個測試實驗室。 本指南使用六部伺服器與兩部用戶端電腦，說明設定和示範遠端存取的方法。 已完成的遠端存取使用 OTP 測試實驗室會模擬內部網路、 網際網路及家用網路，並示範不同網際網路連線案例中的遠端存取功能。  
  
> [!IMPORTANT]  
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。  
  


