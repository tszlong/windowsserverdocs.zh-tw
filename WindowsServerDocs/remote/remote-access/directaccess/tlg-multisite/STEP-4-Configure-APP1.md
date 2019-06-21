---
title: 步驟 4 設定 APP1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 208839b827965d5fdbef4927f25a2477e117999b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283187"
---
# <a name="step-4-configure-app1"></a>步驟 4 設定 APP1

>適用於：Windows Server （半年通道），Windows Server 2016

設定靜態 IPv6 位址和閘道設定，以啟用 APP1 存取與 2 Corpnet 子網路。  
  
- 若要設定的預設閘道和 DNS 伺服器。 多站台的設定會使用 「 路由器 1 」 電腦的預設閘道。 在 APP1 上設定的預設閘道。  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>若要設定的預設閘道和 DNS 伺服器  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步**有線乙太網路連線**，按一下連結。  
  
2.  在 **網路連線** 視窗中，以滑鼠右鍵按一下**有線乙太網路連線**，然後按一下 **屬性**。  
  
3.  在上**有線乙太網路連線內容** 對話方塊中，按一下**Internet Protocol Version 4 (TCP/IPv4)** ，然後按一下**屬性**。  
  
4.  在**預設閘道**，型別**10.0.0.254**，然後在**備用 DNS 伺服器**，型別**10.2.0.1**，然後按一下  **確定**.  
  
5.  在上**有線乙太網路連線內容**] 對話方塊中，按一下**Internet Protocol Version 6 (TCP/IPv6)** ，然後按一下 [**屬性**。  
  
6.  在 **預設閘道**，型別**2001:db8:1::fe**。 在 **備用 DNS 伺服器**，型別**2001:db8:2::1**，然後按一下**確定**。  
  
7.  在上**有線乙太網路連線內容** 對話方塊中，按一下**關閉**，然後關閉**網路連線**視窗。  
  


