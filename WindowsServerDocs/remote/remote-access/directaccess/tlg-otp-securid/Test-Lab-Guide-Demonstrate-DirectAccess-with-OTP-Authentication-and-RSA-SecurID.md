---
title: 測試實驗室指南-示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess
description: 瞭解如何使用混合的 IPv4 和 IPv6 測試實驗室指南，擴充示範 DirectAccess 單一伺服器設定，以示範遠端存取一次性密碼 (OTP) 設定。
manager: brianlic
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 91f24de07909fabcb0b3d2272d57887cfa12a22e
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040308"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>測試實驗室指南：示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess

>適用於：Windows Server (半年度管道)、Windows Server 2016

遠端存取是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 作業系統中的伺服器角色，可讓遠端使用者使用 DirectAccess 或虛擬私人網路來安全地存取內部網路資源 (Vpn) 與路由及遠端存取服務 (RRAS) 。 本指南包含逐步指示，說明如何擴充 [測試實驗室指南：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝](https://go.microsoft.com/fwlink/p/?LinkId=237004) ，以示範遠端存取一次性密碼 (OTP) 設定。

> [!WARNING]
> 此測試實驗室指南的設計包括基礎結構伺服器，例如網域控制站，以及執行 Windows Server 2012 R2 或 Windows Server 2012 (CA) 的憑證授權單位單位。 使用此測試實驗室指南來設定執行其他作業系統的基礎結構伺服器尚未經過測試，本指南不包含設定其他作業系統的指示。

## <a name="about-this-guide"></a>關於本指南
Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的遠端存取會新增支援 OTP 的用戶端驗證。 基於此測試實驗室的目的，僅使用 RSA SecurID 來示範使用遠端存取的 OTP 功能。 也支援其他 RADIUS 型 OTP 解決方案，但不在此測試實驗室的範圍內。 本指南使用六部伺服器與兩部用戶端電腦，說明設定和示範遠端存取的方法。 使用 OTP 測試實驗室完成的遠端存取會模擬內部網路、網際網路和家用網路，並示範不同網際網路連線案例中的遠端存取功能。

> [!IMPORTANT]
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。



