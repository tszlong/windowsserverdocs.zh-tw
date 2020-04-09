---
title: 步驟8設定 INET1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: ca8a49e612bef9c4a72c47e8d0e147a900686f84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861541"
---
# <a name="step-8-configure-inet1"></a>步驟 8：設定 INET1

>適用於：Windows Server (半年通道)、Windows Server 2016

若要讓用戶端電腦透過網際網路連線到遠端存取服務器，您必須在 INET1 上設定 2 EDGE1 的 DNS 專案。  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>建立2個 EDGE1 的 DNS 專案  
  
1.  在 [**開始**] 畫面上，輸入**dnsmgmt.msc**，然後按 enter。  
  
2.  在主控台樹中，開啟 [**正向對應區域**]，按一下 [ **contoso.com**]，以滑鼠右鍵按一下 [ **contoso.com**]，然後按一下 **[新增主機（A 或 AAAA）** ]。  
  
3.  在 [**名稱**] 中，輸入**2-EDGE1**。 在 [ **IP 位址**] 中，輸入**131.107.0.20**。 依序按一下 [新增主機]、[確定]，然後按一下 [完成]。  
  


