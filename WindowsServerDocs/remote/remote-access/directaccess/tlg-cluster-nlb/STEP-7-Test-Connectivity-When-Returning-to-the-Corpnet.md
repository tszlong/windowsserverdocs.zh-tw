---
title: 步驟 7 的測試連線時回到公司網路
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af22b1bbc923b9a06e4aebb910690050af1b7492
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446628"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>步驟 7 的測試連線時回到公司網路

>適用於：Windows Server （半年通道），Windows Server 2016

因此很重要，而當他們回到公司網路，他們就能夠存取的資源而不需要進行任何組態變更，您的許多使用者會遠端位置和公司、 之間進行移動。 遠端存取做到這一點因為當 DirectAccess 用戶端回到公司網路時，它能夠連線到網路位置伺服器。 一旦到網路位置伺服器成功建立 HTTPS 連線，DirectAccess 用戶端就會停用 DirectAccess 用戶端設定，並使用直接連線到公司網路。  
  
### <a name="test-connectivity-on-client1"></a>在 CLIENT1 上的 測試連線  
  
1. 關閉 CLIENT1 然後拔除 CLIENT1 從家用網路子網路或虛擬交換器，並將它連線到公司網路子網路或虛擬交換器。 在 CLIENT1 上，開啟，並以 corp\user1 的身分登入。  
  
2. 開啟提升權限的 Windows PowerShell 視窗，輸入**ipconfig /all**，然後按 ENTER。 輸出會指出 CLIENT1 本機的 IP 位址，而且沒有任何使用中的 6to4、teredo 或 IP-HTTPS 通道。  
  
3. 測試連線能力 APP2 上的網路共用。 在 **開始**畫面上，輸入<strong>\\\APP2\Files</strong>，然後按 ENTER 鍵。 您可以開啟該資料夾中的檔案。  
  


