---
title: 步驟 12 測試 DirectAccess 連線
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4e45f0c3c988c86a2428c3beb8bafc29b7b16bc0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446931"
---
# <a name="step-12-test-directaccess-connectivity"></a>步驟 12 測試 DirectAccess 連線

>適用於：Windows Server （半年通道），Windows Server 2016

當它們位於網際網路或家用網路的網路上時，您可以測試從用戶端電腦的連線之前，您必須確定它們有正確的群組原則設定。  
  
- 若要確認用戶端有正確的群組原則  
  
- 測試從 EDGE1 透過網際網路的 DirectAccess 連線  
  
- CLIENT2 移動至 Win7_Clients_Site2 安全性群組  
  
- 測試 2 EDGE1 透過網際網路的 DirectAccess 連線  
  
## <a name="prerequisites"></a>必要條件  
這兩個用戶端電腦連線到公司網路，並再重新啟動這兩個用戶端電腦。  
  
## <a name="policy"></a>請確認用戶端有正確的群組原則  
  
1.  在 CLIENT1 上，按一下**開始**，型別**powershell.exe**，以滑鼠右鍵按一下**powershell**，按一下 **進階**，然後按一下**系統管理員身分執行**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
2.  在 Windows PowerShell 視窗中，輸入**ipconfig**按 ENTER 鍵。  
  
    請確定您的公司網路介面卡 IPv4 位址開頭 10.0.0。  
  
3.  在 Windows PowerShell 視窗輸入**Get-dnsclientnrptpolicy**按 ENTER 鍵。 隨即顯示 DirectAccess 的名稱解析原則表格 (NRPT) 項目。  
  
    -   。 corp.contoso.com-這些設定指示，以其中一個 DirectAccess 的 DNS 伺服器，IPv6 位址 2001:db8:1::2 或 2001:db8:2::20 解決 corp.contoso.com 的所有連線。  
  
    -   nls.corp.contoso.com 這些設定會指出沒有名稱 nls.corp.contoso.com 豁免。  
  
4.  讓 Windows PowerShell 視窗開啟，以供下一個程序。  
  
5.  在 CLIENT2 上，按一下**開始**，按一下**所有程式**，按一下 **附屬應用程式**，按一下  **Windows PowerShell**，以滑鼠右鍵按一下**Windows PowerShell**，然後按一下**系統管理員身分執行**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
6.  在 Windows PowerShell 視窗中，輸入**ipconfig**按 ENTER 鍵。  
  
    請確定您的公司網路介面卡 IPv4 位址開頭 10.0.0。  
  
7.  在 Windows PowerShell 視窗中，輸入**netsh 命名空間會顯示原則**按 ENTER 鍵。  
  
    在輸出中，應該有兩個區段：  
  
    -   。 corp.contoso.com-這些設定可讓您指出 DirectAccess 的 DNS 伺服器，與 IPv6 位址 2001:db8:1::2 應該解決 corp.contoso.com 的所有連線。  
  
    -   nls.corp.contoso.com 這些設定會指出沒有名稱 nls.corp.contoso.com 豁免。  
  
8.  讓 Windows PowerShell 視窗開啟，以供下一個程序。  
  
## <a name="EDGE1"></a>測試從 EDGE1 透過網際網路的 DirectAccess 連線  
  
1. 請拔除 2-EDGE1 來自網際網路的網路。  
  
2. 將 CLIENT1 和 CLIENT2 從公司網路交換器拔除，並將其連接到網際網路交換器。 等候 30 秒。  
  
3. 在 CLIENT1 上，在 Windows PowerShell 視窗中，輸入**ipconfig /all**按 ENTER 鍵。  
  
4. 檢查 ipconfig 命令的輸出。  
  
   用戶端電腦現在已連線到網際網路，並具有公用的 IPv4 位址。 當 DirectAccess 用戶端具有公用的 IPv4 位址時，它會使用 Teredo 或 IP-HTTPS IPv6 轉換技術，來建立通道的 IPv6 訊息以透過 IPv4 網際網路的 DirectAccess 用戶端和遠端存取伺服器之間。 請注意，Teredo 慣用的轉換技術。  
  
5. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。 這會排清可能仍存在於用戶端 DNS 快取中時用戶端電腦連線到公司網路的名稱解析項目。  
  
6. 停用 Teredo 介面，以確保用戶端電腦使用 IP-HTTPS 連線到公司網路，使用下列命令：  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. 確定您已連線透過 EDGE1。 型別**netsh 介面 httpstunnel 顯示介面**按 ENTER 鍵。  
  
   輸出應包含 URL: https://edge1.contoso.com:443/IPHTTPS。  
  
   > [!TIP]  
   > 在 CLIENT1 上，您也可以執行下列 Windows PowerShell 命令：**取得 NetIPHTTPSConfiguration**。 輸出會顯示可用的伺服器 URL 連線與目前作用中的設定檔。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自指派到 APP1，在此案例中是 2001:db8:1::3 的回覆的 IPv6 位址。  
  
9. 在 Windows PowerShell 視窗中，輸入**ping 2-app1**按 ENTER 鍵。 您應該會看到來自指派給 2-APP1，在此案例中是 2001:db8:2::3 的 IPv6 位址。  
  
10. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給在此情況下是 fd 的 APP2 之 NAT64 位址**c9:9f4e:eb1b**: 7777::a00:4。 請注意，粗體顯示的值會因地址的產生方式而有所不同。  
  
    Ping APP2 的功能很重要，因為成功，表示您仍可連線使用 nat64/dns64，因為 APP2 IPv4 唯一的資源。  
  
11. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://app1/** 按 ENTER 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
12. 在 Internet Explorer 網址列中，輸入 **https://2-app1/** 按 ENTER 鍵。 2-APP1 上，您會看到預設的網站。  
  
13. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
14. 在 **開始**畫面上，輸入<strong>\\\2-App1\Files</strong>，然後按 ENTER 鍵。 按兩下 範例文字檔案。  
  
    這示範了您仍可連線到檔案伺服器，透過 EDGE1 連線時 corp2.corp.contoso.com 網域中。  
  
15. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。  
  
    這示範了您仍可連線到 IPv4 唯一伺服器使用 SMB 來取得資源網域中的資源。  
  
16. 在 **開始**畫面上，輸入**wf.msc**，然後按 ENTER 鍵。  
  
17. 在 **具有進階安全性的 Windows 防火牆**主控台中，請注意，只有**公用設定檔**作用中。 DirectAccess，讓它正常運作時，必須啟用 Windows 防火牆。 如果已停用 Windows 防火牆，DirectAccess 連線無法運作。  
  
18. 在主控台的左窗格中，依序展開**監視**節點，然後按一下**連線安全性規則**節點。 您應該會看到的作用中連線安全性規則：**DirectAccess 原則 ClientToCorp**， **DirectAccess 原則 ClientToDNS64NAT64PrefixExemption**， **DirectAccess 原則 ClientToInfra**，和**DirectAccess原則 ClientToNlaExempt**。 捲動至右方，才能顯示中間的窗格**第 1 個驗證方法**並**第 2 個驗證方法**資料行。 請注意，第一個規則 (ClientToCorp) 會使用 Kerberos V5，在建立內部網路通道和第三個規則 (ClientToInfra) 來建立基礎結構通道會使用 NTLMv2。  
  
19. 在主控台的左窗格中，依序展開**安全性關聯**節點，然後按一下**主要模式**節點。 請注意使用 NTLMv2 的基礎結構通道安全性關聯，並使用 Kerberos V5 內部網路通道的安全性關聯。 以滑鼠右鍵按一下顯示的項目**使用者 (Kerberos V5)** 作為**第 2 個驗證方法**然後按一下**屬性**。 在上**一般**索引標籤上，請注意**第二個驗證地區設定識別碼**是**CORP\User1**，指出 User1 可以成功進行驗證，使用 CORP 網域Kerberos。  
  
20. 重複此程序在 CLIENT2 上的步驟 3 中。  
  
## <a name="secgroup"></a>CLIENT2 移動至 Win7_Clients_Site2 安全性群組  
  
1.  在 DC1 上，按一下**開始**，型別**dsa.msc**，然後按 ENTER 鍵。  
  
2.  在 [Active Directory 使用者和電腦] 主控台中，開啟**corp.contoso.com/Users** ，然後按兩下**Win7_Clients_Site1**。  
  
3.  在  **Win7_Clients_Site1 屬性** 對話方塊中，按一下**成員**索引標籤上，按一下  **CLIENT2**，按一下 **移除**，按一下**是**，然後按一下**確定**。  
  
4.  按兩下**Win7_Clients_Site2**，然後在**Win7_Clients_Site2 屬性** 對話方塊中，按一下 **成員** 索引標籤。  
  
5.  按一下 **新增**，然後在**選取使用者、 連絡人、 電腦或服務帳戶** 對話方塊中，按一下 **物件類型**，選取**電腦**，然後按一下**確定**。  
  
6.  在 **輸入要選取的物件名稱**，型別**CLIENT2**，然後按一下  **確定**。  
  
7.  重新啟動 CLIENT2，並使用 corp/User1 帳戶登入。  
  
8.  在 CLIENT2 上，開啟提升權限的 Windows PowerShell 視窗中，型別**netsh 命名空間會顯示原則**按 ENTER 鍵。  
  
    在輸出中，應該有兩個區段：  
  
    -   。 corp.contoso.com-這些設定可讓您指出 DirectAccess 的 DNS 伺服器，與 IPv6 位址 2001:db8:2::20 應該解決 corp.contoso.com 的所有連線。  
  
    -   nls.corp.contoso.com 這些設定會指出沒有名稱 nls.corp.contoso.com 豁免。  
  
## <a name="DAConnect"></a>測試 2 EDGE1 透過網際網路的 DirectAccess 連線  
  
1. 連線到網際網路的網路的 2 EDGE1。  
  
2. 從網際網路的網路，請拔除 EDGE1。  
  
3. 在 CLIENT1 上，開啟提升權限的 Windows PowerShell 視窗。  
  
4. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。 這會排清可能仍存在於用戶端 DNS 快取中時用戶端電腦連線到公司網路的名稱解析項目。  
  
5. 確定您已連線到 2 EDGE1。 型別**netsh 介面 httpstunnel 顯示介面**按 ENTER 鍵。  
  
   輸出應包含 URL: https://2-edge1.contoso.com:443/IPHTTPS。  
  
   > [!TIP]  
   > 在 CLIENT1 上，您也可以執行下列命令：**取得 NetIPHTTPSConfiguration**。 輸出會顯示可用的伺服器 URL 連線與目前作用中的設定檔。  
  
   > [!NOTE]  
   > CLIENT1 會自動變更的伺服器透過它連線到公司資源。 如果命令的輸出會顯示連接至 EDGE1，等候約五分鐘，並再試一次。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自指派到 APP1，在此案例中是 2001:db8:1::3 的回覆的 IPv6 位址。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping 2-app1**按 ENTER 鍵。 您應該會看到來自指派給 2-APP1，在此案例中是 2001:db8:2::3 的 IPv6 位址。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給在此情況下是 fd 的 APP2 之 NAT64 位址**c9:9f4e:eb1b**: 7777::a00:4。 請注意，粗體顯示的值會因地址的產生方式而有所不同。  
  
   Ping APP2 的功能很重要，因為成功，表示您仍可連線使用 nat64/dns64，因為 APP2 IPv4 唯一的資源。  
  
9. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://app1/** 按 ENTER 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
10. 在 Internet Explorer 網址列中，輸入 **https://2-app1/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
11. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 APP3 上，您會看到預設的網站。  
  
12. 在 **開始**畫面上，輸入<strong>\\\App1\Files</strong>，然後按 ENTER 鍵。 按兩下 範例文字檔案。  
  
    這示範了您能夠連線到透過 2 EDGE1 連線時 corp.contoso.com 網域中的檔案伺服器。  
  
13. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。  
  
    這示範了您仍可連線到 IPv4 唯一伺服器使用 SMB 來取得資源網域中的資源。  
  
14. 重複此程序在 CLIENT2 上的從步驟 3。  
  


