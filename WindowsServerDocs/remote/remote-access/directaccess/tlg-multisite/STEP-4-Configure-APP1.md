---
title: 步驟4設定 APP1
description: 瞭解如何設定預設閘道和 DNS 伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 43b77e32bae337ee1751ed9e6c12e2fa30f0c52c
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040458"
---
# <a name="step-4-configure-app1"></a>步驟4設定 APP1

>適用於：Windows Server (半年度管道)、Windows Server 2016

設定靜態 IPv6 位址和閘道設定，以啟用 APP1 存取2個公司的子網。

- 設定預設閘道和 DNS 伺服器。 多網站設定會使用 ROUTER1 電腦作為預設閘道。 設定 APP1 上的預設閘道。

## <a name="to-configure-the-default-gateway-and-dns-server"></a>設定預設閘道和 DNS 伺服器

1.  在伺服器管理員主控台中，按一下 [ **本機伺服器**]，然後在 [ **屬性** ] 區域中，按一下 [ **有線乙太** 網路連線] 旁的連結。

2.  在 [**網路** 連線] 視窗中，以滑鼠 **按右鍵 [****有線乙太** 網路連線]，然後按一下 [內容]。

3.  在 [**有線乙太** 網路線上內容] 對話方塊中 **，按一下 [****網際網路通訊協定第4版 (TCP/IPv4)**]，然後按一下 [內容]。

4.  在 [ **預設閘道**] 中，輸入 **10.0.0.254**，然後在 [ **備用 DNS 伺服器**] 中輸入 **10.2.0.1**，然後按一下 **[確定]**。

5.  在 [**有線乙太** 網路線上內容] 對話方塊中 **，按一下 [****網際網路通訊協定第6版 ([TCP/IPv6)**]，然後按一下 [內容]。

6.  在 [ **預設閘道**] 中，輸入 **2001： db8：1：： fe**。 在 [ **備用 DNS 伺服器**] 中，輸入 **2001： db8：2：： 1**，然後按一下 **[確定]**。

7.  在 [ **有線乙太網路連接屬性** ] 對話方塊中，按一下 [ **關閉**]，然後關閉 [ **網路** 連線] 視窗。



