---
title: 從家用網路的子網路的步驟 6 的測試 DirectAccess 連線
description: 本主題是測試實驗室指南-使用 OTP 驗證和 RSA SecurID for Windows Server 2016 的示範 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe3014b8b37fab35532f3ecf833188fda9e8b744
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446646"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>從家用網路的子網路的步驟 6 的測試 DirectAccess 連線

>適用於：Windows Server （半年通道），Windows Server 2016

現在已完成 DirectAccess 單次密碼 (OTP) 部署，您可以開始測試從家用網路的子網路的連線。  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>若要測試從 CLIENT1 上的家用網路子網路的 OTP 功能  
  
1. 在 CLIENT1 上，確定您以登入**User1**。  
  
2. 在上**開始**畫面上，輸入**powershell.exe**，以滑鼠右鍵按一下**powershell**，按一下 **進階**，然後按一下 **執行以系統管理員身分**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
3. 在 Windows PowerShell 視窗中，輸入**gpupdate /force**，然後按 ENTER。  
  
4. 拔除 CLIENT1 從公司網路子網路，並將它連線到家用網路的子網路。  
  
5. 在 CLIENT1 上，開啟 Internet Explorer 中，並在 [網址] 列中，輸入 **https://app1.corp.contoso.com/** 按 ENTER 鍵。 按 F5。  
  
   不應該開啟網站。  
  
6. 在上**開始**畫面上，輸入**RSA**，然後按一下**RSA SecurID 權杖**。  
  
7. 等候直到 RSA SecurID 權杖變成單次密碼，然後按**複製**。  
  
8. 按一下通知區域中的 [網路連線]  圖示來存取 DA 媒體管理員。  
  
9. 按一下  **Contoso 的 DirectAccess 連線**，然後按一下**繼續**。  
  
10. 按下控制項 + Alt + Delete，然後按一下**單次密碼 (OTP)** 圖格。  
  
11. 貼上先前複製的八個位數 tokencode，然後按一下**確定**。 等候要完成驗證。 DirectAccess 「 工作地方連線狀態就會變成**Connected**。  
  
12. 在 Internet Explorer 網址列中輸入 **https://app1.corp.contoso.com/** 按 ENTER 鍵。 按 F5。 您將會看到 APP1 上的預設 IIS 網站。  
  
13. 在 Internet Explorer 網址列中，輸入 **https://app2.corp.contoso.com/** 按 ENTER 鍵。 按 F5。 您會看到預設 IIS 網站 APP2 上。  
  
14. 在 **開始**畫面上，輸入<strong>\\\app1\files</strong>，然後按 ENTER。  
  
15. 在 [**檔案**共用的資料夾] 視窗中，按兩下**Example.txt**檔案。 您會看到 Example.txt 檔案的內容。  
  
16. 在 **開始**畫面上，輸入<strong>\\\app2\files</strong>，然後按 ENTER。  
  
17. 在 [**檔案**共用的資料夾] 視窗中，按兩下**新文字.txt**檔案。 您會看到新的文字.txt 檔案的內容。  
  


