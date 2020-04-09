---
title: 步驟12測試 DirectAccess 連線能力
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 85aff4ae9e117359f8827abb15dce47728a30da4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857281"
---
# <a name="step-12-test-directaccess-connectivity"></a>步驟12測試 DirectAccess 連線能力

>適用於：Windows Server (半年通道)、Windows Server 2016

當用戶端電腦位於網際網路或 Homenet 網路時，您必須確定它們具有正確的群組原則設定，才能測試其連線能力。  
  
- 確認用戶端具有正確的群組原則  
  
- 透過 EDGE1 測試來自網際網路的 DirectAccess 連線能力  
  
- 將 CLIENT2 移至 Win7_Clients_Site2 安全性群組  
  
- 透過 2-EDGE1 測試來自網際網路的 DirectAccess 連線能力  
  
## <a name="prerequisites"></a>必要條件  
將兩部用戶端電腦連線到公司網路，然後重新開機兩部用戶端電腦。  
  
## <a name="verify-clients-have-the-correct-group-policy"></a><a name="policy"></a>確認用戶端具有正確的群組原則  
  
1.  在 CLIENT1 上，按一下 [**開始**]，輸入**powershell**，以滑鼠右鍵按一下 [ **powershell**]，然後按一下 [ **Advanced**]，再按一下 [以**系統管理員身分執行**]。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
2.  在 Windows PowerShell 視窗中，輸入**ipconfig** ，然後按 enter。  
  
    請確定公司網路介面卡的 IPv4 位址開頭為10.0.0。  
  
3.  在 Windows PowerShell 視窗中，輸入**get-dnsclientnrptpolicy** ，然後按 enter。 隨即顯示 DirectAccess 的名稱解析原則表格 (NRPT) 項目。  
  
    -   corp.contoso.com-這些設定表示所有與 corp.contoso.com 的連線都應該由其中一部 DirectAccess DNS 伺服器解析，IPv6 位址為2001： db8：1：：2或2001： db8：2：：20。  
  
    -   nls.corp.contoso.com-這些設定表示名稱 nls.corp.contoso.com 有豁免。  
  
4.  讓 Windows PowerShell 視窗保持開啟，以進行下一個程式。  
  
5.  在 CLIENT2 上，依序按一下 [**開始**]、[**所有程式**]、[**附屬**應用]、[ **windows powershell**]、[ **windows powershell**]，然後按一下 [以**系統管理員身分執行**]。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
6.  在 Windows PowerShell 視窗中，輸入**ipconfig** ，然後按 enter。  
  
    請確定公司網路介面卡的 IPv4 位址開頭為10.0.0。  
  
7.  在 Windows PowerShell 視窗中，輸入**netsh namespace show policy** ，然後按 enter。  
  
    在輸出中，應該有兩個區段：  
  
    -   corp.contoso.com-這些設定表示 DirectAccess DNS 伺服器應解析所有與 corp.contoso.com 的連線，IPv6 位址為2001： db8：1：：2。  
  
    -   nls.corp.contoso.com-這些設定表示名稱 nls.corp.contoso.com 有豁免。  
  
8.  讓 Windows PowerShell 視窗保持開啟，以進行下一個程式。  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-edge1"></a><a name="EDGE1"></a>透過 EDGE1 測試來自網際網路的 DirectAccess 連線能力  
  
1. 從網際網路網路拔下 2-EDGE1。  
  
2. 從公司網路交換器中拔下 CLIENT1 和 CLIENT2，並將其連接到網際網路交換器。 等候30秒。  
  
3. 在 CLIENT1 的 Windows PowerShell 視窗中，輸入**ipconfig/all** ，然後按 enter。  
  
4. 檢查 ipconfig 命令的輸出。  
  
   用戶端電腦現在已連線到網際網路，而且具有公用 IPv4 位址。 當 DirectAccess 用戶端具有公用 IPv4 位址時，它會使用 Teredo 或 IP-HTTPS IPv6 轉換技術，透過 DirectAccess 用戶端與遠端存取服務器之間的 IPv4 網際網路來建立 IPv6 訊息的通道。 請注意，Teredo 是慣用的轉換技術。  
  
5. 在 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。 這會清除用戶端電腦連線到公司網路時，用戶端 DNS 快取中可能仍存在的名稱解析專案。  
  
6. 停用 Teredo 介面，以確保用戶端電腦使用 IP-HTTPS 來連接公司網路，並使用下列命令：  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. 請確定您已透過 EDGE1 連線。 輸入**netsh interface HTTPstunnel show** interface，然後按 enter。  
  
   輸出應該包含 URL： https://edge1.contoso.com:443/IPHTTPS。  
  
   > [!TIP]  
   > 在 CLIENT1 上，您也可以執行下列 Windows PowerShell 命令： **NetIPHTTPSConfiguration**。 輸出會顯示可用的伺服器 URL 連接和目前作用中的設定檔。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping app1** ，然後按 enter。 您應該會看到指派給 APP1 的 IPv6 位址回復，在此案例中為2001： db8：1：：3。  
  
9. 在 Windows PowerShell 視窗中，輸入**ping 2-app1** ，然後按 enter。 您應該會看到指派給 2-APP1 的 IPv6 位址回復，在此案例中為2001： db8：2：：3。  
  
10. 在 Windows PowerShell 視窗中，輸入**ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址回復，在此案例中為 fd**c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。  
  
    Ping APP2 的能力很重要，因為成功表示您能夠使用 NAT64/DNS64 建立連接，因為 APP2 是僅 IPv4 的資源。  
  
11. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入 **https://app1/** ，然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
12. 在 Internet Explorer 網址列中，輸入 **https://2-app1/** ，然後按 enter。 您會在 2-APP1 上看到預設的網站。  
  
13. 在 Internet Explorer 網址列中，輸入 **https://app2/** ，然後按 enter。 您將會看到 APP2 上的預設網站。  
  
14. 在 [**開始**] 畫面上，輸入<strong>\\\2-App1\Files</strong>，然後按 enter。 按兩下範例文字檔。  
  
    這會示範當透過 EDGE1 連線時，您能夠連接到 corp2.corp.contoso.com 網域中的檔案伺服器。  
  
15. 在 [**開始**] 畫面上，輸入<strong>\\\App2\Files</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。  
  
    這會示範您可以使用 SMB 連線到僅 IPv4 的伺服器，以取得資源網域中的資源。  
  
16. 在 [**開始**] 畫面上，輸入**wf**，然後按 enter。  
  
17. 在 [**具有 Advanced Security 的 Windows 防火牆**] 主控台中，請注意只有**公用設定檔**為作用中。 必須啟用 Windows 防火牆，DirectAccess 才能正常運作。 如果 Windows 防火牆已停用，DirectAccess 連線就無法運作。  
  
18. 在主控台的左窗格中，展開 [**監視**] 節點，然後按一下 [連線**安全性規則**] 節點。 您應該會看到使用中的連線安全性規則： **Directaccess 原則-ClientToCorp**、 **Directaccess 原則-ClientToDNS64NAT64PrefixExemption**、 **Directaccess 原則-ClientToInfra**和**directaccess 原則-ClientToNlaExempt**。 將中間窗格向右移動，以顯示 [**第一個驗證方法**] 和 [**第二個驗證方法**] 資料行。 請注意，第一個規則（ClientToCorp）會使用 Kerberos V5 來建立內部網路通道，而第三個規則（ClientToInfra）則使用 NTLMv2 來建立基礎結構通道。  
  
19. 在主控台的左窗格中，展開 [**安全性關聯**] 節點，然後按一下 [**主要模式]** 節點。 請注意，使用 NTLMv2 的基礎結構通道安全性關聯和使用 Kerberos V5 的內部網路通道安全性關聯。 以滑鼠右鍵按一下顯示 [**使用者（Kerberos V5）** ] 做為**第二個驗證方法**的專案，然後按一下 [**屬性**]。 在 [**一般**] 索引標籤上，請注意**第二個驗證本機識別碼**是**CORP\User1**，表示 User1 能夠使用 Kerberos 成功地向 CORP 網域進行驗證。  
  
20. 在 CLIENT2 上的步驟3中重複此程式。  
  
## <a name="move-client2-to-the-win7_clients_site2-security-group"></a><a name="secgroup"></a>將 CLIENT2 移至 Win7_Clients_Site2 安全性群組  
  
1.  在 DC1 上，按一下 [**開始**]，輸入**dsa.msc**，然後按 enter。  
  
2.  在 [Active Directory 使用者和電腦] 主控台中，開啟**corp.contoso.com/Users** ，然後按兩下 [ **Win7_Clients_Site1**]。  
  
3.  在 [ **Win7_Clients_Site1**內容] 對話方塊中，按一下 [**成員**] 索引標籤，依序按一下 [ **CLIENT2**]、[**移除** **]、[是]** ，然後按一下 **[確定]** 。  
  
4.  按兩下 [ **Win7_Clients_Site2**]，然後在 [ **Win7_Clients_Site2 屬性**] 對話方塊中，按一下 [**成員**] 索引標籤。  
  
5.  按一下 [**新增**]，然後在 [**選取使用者、連絡人、電腦或服務帳戶**] 對話方塊中，按一下 [**物件類型**]，選取 [**電腦**]，然後按一下 **[確定]** 。  
  
6.  在 **[輸入物件名稱來選取**] 中，輸入**CLIENT2**，然後按一下 **[確定]** 。  
  
7.  重新開機 CLIENT2，並使用 corp/User1 帳戶登入。  
  
8.  在 CLIENT2 上，開啟提升許可權的 Windows PowerShell 視窗，輸入**netsh namespace show policy** ，然後按 enter。  
  
    在輸出中，應該有兩個區段：  
  
    -   corp.contoso.com-這些設定表示 DirectAccess DNS 伺服器應解析所有與 corp.contoso.com 的連線，IPv6 位址為2001： db8：2：：20。  
  
    -   nls.corp.contoso.com-這些設定表示名稱 nls.corp.contoso.com 有豁免。  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-2-edge1"></a><a name="DAConnect"></a>透過 2-EDGE1 測試來自網際網路的 DirectAccess 連線能力  
  
1. 連接 2-EDGE1 到網際網路網路。  
  
2. 從網際網路網路拔下 EDGE1。  
  
3. 在 CLIENT1 上，開啟提升許可權的 Windows PowerShell 視窗。  
  
4. 在 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。 這會清除用戶端電腦連線到公司網路時，用戶端 DNS 快取中可能仍存在的名稱解析專案。  
  
5. 請確定您已透過 2-EDGE1 連線。 輸入**netsh interface HTTPstunnel show** interface，然後按 enter。  
  
   輸出應該包含 URL： https://2-edge1.contoso.com:443/IPHTTPS。  
  
   > [!TIP]  
   > 在 CLIENT1 上，您也可以執行下列命令： **NetIPHTTPSConfiguration**。 輸出會顯示可用的伺服器 URL 連接和目前作用中的設定檔。  
  
   > [!NOTE]  
   > CLIENT1 會自動變更與公司資源連接的伺服器。 如果命令的輸出顯示 EDGE1 的連線，請等候約五分鐘，然後再試一次。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app1** ，然後按 enter。 您應該會看到指派給 APP1 的 IPv6 位址回復，在此案例中為2001： db8：1：：3。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping 2-app1** ，然後按 enter。 您應該會看到指派給 2-APP1 的 IPv6 位址回復，在此案例中為2001： db8：2：：3。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址回復，在此案例中為 fd**c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。  
  
   Ping APP2 的能力很重要，因為成功表示您能夠使用 NAT64/DNS64 建立連接，因為 APP2 是僅 IPv4 的資源。  
  
9. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入 **https://app1/** ，然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
10. 在 Internet Explorer 網址列中，輸入 **https://2-app1/** ，然後按 enter。 您將會看到 APP2 上的預設網站。  
  
11. 在 Internet Explorer 網址列中，輸入 **https://app2/** ，然後按 enter。 您會看到 APP3 上的預設網站。  
  
12. 在 [**開始**] 畫面上，輸入<strong>\\\App1\Files</strong>，然後按 enter。 按兩下範例文字檔。  
  
    這會示範在透過 2 EDGE1 連線時，您能夠連接到 corp.contoso.com 網域中的檔案伺服器。  
  
13. 在 [**開始**] 畫面上，輸入<strong>\\\App2\Files</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。  
  
    這會示範您可以使用 SMB 連線到僅 IPv4 的伺服器，以取得資源網域中的資源。  
  
14. 在步驟3的 CLIENT2 上重複此程式。  
  


