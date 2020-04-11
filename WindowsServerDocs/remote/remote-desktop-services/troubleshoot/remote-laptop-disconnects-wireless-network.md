---
title: 遠端膝上型電腦從無線網路中斷連線
description: 針對遠端膝上型電腦的無線網路連線中斷問題進行疑難排解。
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 72bf482512ff3bb0a678ae59cd6ac20b947a54d9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857151"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>遠端膝上型電腦從無線網路中斷連線

遠端桌面用戶端使用 802.1x 無線網路連線到膝上型電腦時，可能會發生此問題。 膝上型電腦偶爾會從無線網路中斷連線，且不會自動重新連線。

這是個已知問題，當無線網路連線的網路驗證設定為**使用者驗證**，就會發生這個問題。

若要處理此問題，請將網路驗證設定設為**使用者或電腦驗證**或者**電腦驗證**。

 > [!NOTE]  
> 若要在單一電腦上變更網路驗證設定，您需要使用 [網路和共用中心] 控制台，透過新的設定建立新的無線連線。

如需如何使用 GPO 設定無線網路設定的完整說明，請參閱[設定無線網路 (IEEE 802.11) 原則](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies)。
