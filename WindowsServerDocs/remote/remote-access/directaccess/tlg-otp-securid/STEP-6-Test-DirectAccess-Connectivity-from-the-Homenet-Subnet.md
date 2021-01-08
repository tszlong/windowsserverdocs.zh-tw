---
title: 步驟6測試 Homenet 子網的 DirectAccess 連線能力
description: 瞭解如何開始測試 Homenet 子網的連線能力。
manager: brianlic
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 1d0cd5c85ca97ffd7d5275d2402db17a560255fa
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040338"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>步驟6測試 Homenet 子網的 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

DirectAccess 一次性密碼 (OTP) 部署現在已完成，您可以開始測試 Homenet 子網的連線。

### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>從 CLIENT1 上的 Homenet 子網測試 OTP 功能

1. 在 CLIENT1 上，請確定您是以 **User1** 身分登入。

2. 在 [ **開始** ] 畫面上輸入 **powershell.exe**，以滑鼠右鍵按一下 [ **powershell**]，再按一下 [ **Advanced**]，然後按一下 [以 **系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

3. 在 [Windows PowerShell] 視窗中，輸入 **gpupdate/force**，然後按 enter 鍵。

4. 從公司網路子網中拔下 CLIENT1，並將其連線到 Homenet 子網。

5. 在 CLIENT1 上，開啟 Internet Explorer，然後在網址列中輸入， **https://app1.corp.contoso.com/** 然後按 enter 鍵。 按 F5。

   網站不應開啟。

6. 在 [ **開始** ] 畫面上，輸入 **rsa**，然後按一下 [ **rsa SecurID Token**]。

7. 等候 RSA SecurID 權杖變更一次性密碼，然後按一下 [ **複製**]。

8. 按一下通知區域中的 [網路連線] 圖示來存取 DA 媒體管理員。

9. 按一下 [ **Contoso DirectAccess 連接**]，然後按一下 [ **繼續**]。

10. 按下 Ctrl + Alt + Delete，然後按一下單次 **密碼 (OTP)** 磚。

11. 貼上先前複製的八位數權杖，然後按一下 **[確定]**。 等候驗證完成。 DirectAccess Workplace 線上狀態現在將會 **連接**。

12. 在 Internet Explorer 的網址列中，輸入， **https://app1.corp.contoso.com/** 然後按 enter 鍵。 按 F5。 您將會看到 APP1 上的預設 IIS 網站。

13. 在 Internet Explorer 網址列中，輸入， **https://app2.corp.contoso.com/** 然後按 enter。 按 F5。 您將會在 APP2 上看到預設的 IIS 網站。

14. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP1\FILES</strong>，然後按 enter。

15. **在 [檔案共用資料夾**] 視窗中，按兩下 **Example.txt** 檔案。 您將會看到 Example.txt 檔案的內容。

16. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。

17. **在 [檔案共用資料夾**] 視窗中，按兩下新的 **文字 Document.txt** 檔。 您將會看到新文字 Document.txt 檔案的內容。



