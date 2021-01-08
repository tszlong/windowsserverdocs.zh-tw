---
title: 步驟 7-從網際網路測試 DirectAccess 連線能力
description: 瞭解如何開始測試網際網路的連線能力。
manager: brianlic
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: f921a536d229edc6295df2f18d3ab2bda62133aa
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040328"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>步驟 7-從網際網路測試 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

DirectAccess 一次性密碼 (OTP) 部署已經過 Homenet 子網的測試，現在可以從網際網路進行測試。

### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>在 CLIENT1 上從網際網路測試 OTP 功能

1. 在 CLIENT1 上，請確定您是以 **User1** 身分登入。 將 CLIENT1 連線到公司網路子網。

2. 在 [ **開始** ] 畫面上輸入 **powershell.exe**，以滑鼠右鍵按一下 [ **powershell**]，再按一下 [ **Advanced**]，然後按一下 [以 **系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

3. 在 [Windows PowerShell] 視窗中，輸入 **gpupdate/force** ，然後按 enter 鍵。

4. 從 Homenet 子網中拔下 CLIENT1、將它連線到網際網路，然後重新開機電腦。

5. 在 CLIENT1 上，開啟 Internet Explorer，然後在網址列中輸入， **https://app1.corp.contoso.com/** 然後按 enter 鍵。 按 F5。

   網站不應開啟。

6. 在 [ **開始** ] 畫面上，輸入 **rsa**，然後按一下 [ **rsa SecurID Token**]。

7. 等候 RSA SecurID 權杖變更一次性密碼，然後按一下 [ **複製**]。

8. 按一下通知區域中的 [網路連線] 圖示來存取 DA 媒體管理員。

9. 按一下 [ **工作地點連接**]，然後按一下 [ **繼續**]。

10. 按下 Ctrl + Alt + Delete，然後按一下單次 **密碼 (OTP)** 磚。

11. 貼上先前複製的八位數權杖，然後按一下 **[確定]**。 等候驗證完成。 DirectAccess Workplace 線上狀態現在將會 **連接**。

12. 在 Internet Explorer 的網址列中，輸入， **https://app1.corp.contoso.com/** 然後按 enter 鍵。 按 F5。 您將會看到 APP1 上的預設 IIS 網站。

13. 在 Internet Explorer 網址列中，輸入， **https://app2.corp.contoso.com/** 然後按 enter。 按 F5。 您將會在 APP2 上看到預設的 IIS 網站。

14. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP1\FILES</strong>，然後按 enter。

15. **在 [檔案共用資料夾**] 視窗中，按兩下 **Example.txt** 檔案。 您將會看到 Example.txt 檔案的內容。

16. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。

17. **在 [檔案共用資料夾**] 視窗中，按兩下新的 **文字 Document.txt** 檔。 您將會看到新文字 Document.txt 檔案的內容。



