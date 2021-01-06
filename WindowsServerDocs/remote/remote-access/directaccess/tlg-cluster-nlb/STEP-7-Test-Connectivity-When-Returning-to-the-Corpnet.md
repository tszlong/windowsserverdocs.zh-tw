---
title: 返回公司網路時，步驟7測試連線能力
description: 本主題是測試實驗室指南的一部分-示範 Windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a808b97ad531c38f3f6a08f416b0f0f30a5e50fa
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946534"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>返回公司網路時，步驟7測試連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

許多使用者會在遠端位置與公司網路之間移動，因此當他們回到公司網路時，他們就能存取資源，而不需要進行任何設定變更。 遠端存取可進行這項工作，因為當 DirectAccess 用戶端返回公司網路時，它就能夠連接到網路位置伺服器。 一旦成功建立 HTTPS 連線到網路位置伺服器之後，DirectAccess 用戶端會停用 DirectAccess 用戶端設定，並使用與公司網路的直接連線。

### <a name="test-connectivity-on-client1"></a>測試 CLIENT1 上的連線能力

1. 關閉 CLIENT1，然後從 Homenet 子網或虛擬交換器中拔下 CLIENT1，然後將它連線到公司網路子網或虛擬交換器。 開啟 CLIENT1，並以 CORP\User1. 登入

2. 開啟提高許可權的 Windows PowerShell 視窗中，輸入 **ipconfig/all**，然後按 enter 鍵。 輸出會指出 CLIENT1 具有本機 IP 位址，且沒有使用中的6to4、Teredo 或 IP-HTTPS 通道。

3. 測試對 APP2 上網路共用的連線能力。 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 您將可以開啟該資料夾中的檔案。



