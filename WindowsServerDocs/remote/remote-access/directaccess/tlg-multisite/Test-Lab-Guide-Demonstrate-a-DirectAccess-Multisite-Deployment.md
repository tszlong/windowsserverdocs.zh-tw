---
title: 測試實驗室指南-示範 DirectAccess 多網站部署
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1980923d0ed29caa60efa012342ef52c93970dfe
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314505"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>測試實驗室指南：示範 DirectAccess 多站台部署

>適用於：Windows Server (半年通道)、Windows Server 2016

遠端存取是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 作業系統中的一個伺服器角色，可讓遠端使用者使用 DirectAccess 或 RRAS VPN 安全地存取內部網路資源。 本指南包含擴充[測試實驗室指南的逐步指示：使用混合 IPv4 和 IPv6 示範 DirectAccess 單一伺服器安裝](https://go.microsoft.com/fwlink/p/?LinkId=237004)，以示範在多站案例中的遠端存取。  
  
在多網站案例中部署遠端存取可讓您在不同地理位置上設定遠端存取服務器。 過去，遠端使用者必須一律透過特定 DirectAccess 伺服器連線到公司網路。 在 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 和 Windows 10 或 Windows 8 中，您可以設定部署中每個地理位置的進入點。 每個進入點都可以是單一遠端存取服務器或遠端存取服務器的叢集。 遠端使用者可以選擇連接到組織的任何遠端存取進入點。 例如，如果遠端使用者通常連線到位於亞洲的「遠端存取」進入點，但又到達「歐洲」的商務旅程，則用戶端電腦會自動連接到最接近的「遠端存取」進入點。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南包含設定和示範使用九部伺服器和三部用戶端電腦進行遠端存取的指示。 完成的遠端存取多網站測試實驗室會模擬內部網路、網際網路和家用網路，並示範不同網際網路連線案例中的遠端存取功能。  
  
> [!IMPORTANT]  
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。  
  


