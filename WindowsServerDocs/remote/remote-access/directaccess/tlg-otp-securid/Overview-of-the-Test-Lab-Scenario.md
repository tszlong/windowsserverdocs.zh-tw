---
title: 測試實驗室案例概觀
description: 本主題屬於測試實驗室指南-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cd47f9866859f6d57fab8dcf76b1bcb475420d97
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314469"
---
# <a name="overview-of-the-test-lab-scenario"></a>測試實驗室案例概觀

>適用於：Windows Server (半年通道)、Windows Server 2016

遠端存取是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 作業系統中的一個伺服器角色，可讓遠端使用者使用 DirectAccess 或虛擬私人網路（Vpn），以安全地存取內部網路資源。路由及遠端存取服務（RRAS）。 本指南包含擴充[測試實驗室指南的逐步指示：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝](https://go.microsoft.com/fwlink/p/?LinkId=237004)，以示範遠端存取一次性密碼（OTP）設定。  
  
> [!WARNING]  
> 此測試實驗室指南的設計包括基礎結構伺服器，例如網域控制站和執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的憑證授權單位單位（CA）。 使用此測試實驗室指南來設定執行其他作業系統的基礎結構伺服器尚未經過測試，且本指南不含設定其他作業系統的指示。  
  
## <a name="about-this-guide"></a>關於本指南  
Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的遠端存取會新增使用 OTP 的用戶端驗證支援。 基於此測試實驗室的目的，只有 RSA SecurID 用來示範使用遠端存取的 OTP 功能。 另外也支援其他以 RADIUS 為基礎的 OTP 解決方案，但超出此測試實驗室的範圍。 本指南使用六部伺服器與兩部用戶端電腦，說明設定和示範遠端存取的方法。 「使用 OTP 完成遠端存取」測試實驗室會模擬內部網路、網際網路和家用網路，並示範不同網際網路連線案例中的遠端存取功能。  
  
> [!IMPORTANT]  
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。  
  


