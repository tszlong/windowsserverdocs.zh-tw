---
title: 步驟7測試來自網際網路的 DirectAccess 連線能力
description: 本主題屬於測試實驗室指南-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 738e0f10762c0d292e344ba25fa34cdb0d17b766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367542"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>步驟7測試來自網際網路的 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

DirectAccess 一次性密碼（OTP）部署已從 Homenet 子網進行測試，現在可以從網際網路進行測試。  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>在 CLIENT1 上測試來自網際網路的 OTP 功能  
  
1. 在 CLIENT1 上，請確定您是以**User1**身分登入。 將 CLIENT1 連線到公司網路子網。  
  
2. 在 [**開始**] 畫面上，輸入**powershell**，以滑鼠右鍵按一下 [ **powershell**]，然後按一下 [ **Advanced**]，再按一下 [以**系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
3. 在 Windows PowerShell 視窗中，輸入**gpupdate/force** ，然後按 enter。  
  
4. 從 Homenet 子網拔下 CLIENT1、將其連線到網際網路，然後重新開機電腦。  
  
5. 在 CLIENT1 上開啟 Internet Explorer，然後在網址列中輸入 **https://app1.corp.contoso.com/** ，然後按 enter 鍵。 按 F5。  
  
   網站不應開啟。  
  
6. 在 [**開始**] 畫面上，輸入**rsa**，然後按一下 [ **rsa SecurID Token**]。  
  
7. 等待 RSA SecurID 權杖變更一次性密碼，然後按一下 [**複製**]。  
  
8. 按一下通知區域中的 [網路連線] 圖示來存取 DA 媒體管理員。  
  
9. 按一下 [**工作場所連接**]，然後按一下 [**繼續**]。  
  
10. 按 Ctrl + Alt + Delete，然後按一下 [單次**密碼（OTP）** ] 磚。  
  
11. 貼上先前複製的八個數字權杖，然後按一下 **[確定]** 。 等待驗證完成。 DirectAccess 工作場所線上狀態現在將會**連接**。  
  
12. 在 Internet Explorer 的網址列中，輸入 **https://app1.corp.contoso.com/** ，然後按 enter 鍵。 按 F5。 您將會看到 APP1 上的預設 IIS 網站。  
  
13. 在 Internet Explorer 網址列中，輸入 **https://app2.corp.contoso.com/** ，然後按 enter。 按 F5。 您會在 APP2 上看到預設的 IIS 網站。  
  
14. 在 [**開始**] 畫面上，輸入<strong>\\ \ app1\files</strong>，然後按 enter。  
  
15. **在 [檔案共用資料夾**] 視窗中，按兩下**範例 .txt**檔案。 您會看到範例 .txt 檔案的內容。  
  
16. 在 [**開始**] 畫面上，輸入<strong>\\ \ app2\files</strong>，然後按 enter。  
  
17. 在 [檔案共用資料夾] 視窗中，按兩下新的 [**檔**] [ **txt** ] 檔案。 您會看到新的文字檔 .txt 檔案的內容。  
  


