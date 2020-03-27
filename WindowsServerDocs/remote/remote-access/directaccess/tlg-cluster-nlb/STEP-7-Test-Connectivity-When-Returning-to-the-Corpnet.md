---
title: 步驟7在返回公司網路時測試連線能力
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 491533ae5d141de4ab4f15126d8977cf15c8f7f4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314679"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>步驟7在返回公司網路時測試連線能力

>適用於：Windows Server (半年通道)、Windows Server 2016

您的許多使用者會在遠端位置與公司網路之間移動，因此當他們回到公司網路時，他們能夠存取資源，而不需要進行任何設定變更，這點很重要。 遠端存取可讓您這麼做，因為當 DirectAccess 用戶端回到公司網路時，它就能夠連接到網路位置伺服器。 成功建立與網路位置伺服器的 HTTPS 連線之後，DirectAccess 用戶端會停用 DirectAccess 用戶端設定，並使用與公司網路的直接連線。  
  
### <a name="test-connectivity-on-client1"></a>在 CLIENT1 上測試連線能力  
  
1. 關閉 CLIENT1，然後從 Homenet 子網或虛擬交換器中斷 CLIENT1，並將它連線到公司網路子網或虛擬交換器。 開啟 CLIENT1，並以 CORP\User1. 的身分登入  
  
2. 開啟提升許可權的 Windows PowerShell 視窗，輸入**ipconfig/all**，然後按 enter。 輸出會指出 CLIENT1 具有本機 IP 位址，而且沒有作用中的6to4、Teredo 或 IP-HTTPS 通道。  
  
3. 測試 APP2 上網路共用的連線能力。 在 [**開始**] 畫面上，輸入<strong>\\\APP2\Files</strong>，然後按 enter。 您將能夠開啟該資料夾中的檔案。  
  


