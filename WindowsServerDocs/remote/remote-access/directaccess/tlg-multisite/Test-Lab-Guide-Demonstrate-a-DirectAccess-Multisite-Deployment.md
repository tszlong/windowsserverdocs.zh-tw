---
title: 測試實驗室指南： 示範 DirectAccess 多站台部署
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87c1f629d96d247d273d48066797795b9fe724e6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281384"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>測試實驗室指南：示範 DirectAccess 多站台部署

>適用於：Windows Server （半年通道），Windows Server 2016

Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 作業系統的伺服器角色，可讓遠端使用者可以安全地存取內部網路資源，透過 DirectAccess 或是 RRAS VPN 遠端存取。 本指南包含逐步解說以擴充[測試實驗室指南：示範 DirectAccess 單一伺服器安裝使用混合的 IPv4 與 IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004)示範遠端存取多站台的案例中。  
  
在多站台的情況下部署遠端存取，可讓您在 地理位置中設定遠端存取伺服器。 先前，遠端使用者必須永遠連接到公司網路，透過特定的 DirectAccess 伺服器。 與 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 和 Windows 10 或 Windows 8，您可以部署中設定針對每個地理位置的進入點。 每個進入點可以是單一的遠端存取伺服器或叢集的遠端存取伺服器。 遠端使用者可以選擇連接到任何組織的遠端存取進入點。 例如，如果遠端使用者通常會連接到位於亞洲遠端存取進入點，但接著出差的旅途歐洲，用戶端電腦會自動連線至最接近的遠端存取進入點。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南包含設定和示範使用九部伺服器和三個用戶端電腦的遠端存取的指示。 已完成的遠端存取多站台的測試實驗室會模擬內部網路、 網際網路及家用網路，並示範不同網際網路連線案例中的遠端存取功能。  
  
> [!IMPORTANT]  
> 這個實驗室是使用最少部電腦的概念性驗證。 本指南詳述的設定僅供測試實驗室使用，不得用於實際執行環境。  
  


