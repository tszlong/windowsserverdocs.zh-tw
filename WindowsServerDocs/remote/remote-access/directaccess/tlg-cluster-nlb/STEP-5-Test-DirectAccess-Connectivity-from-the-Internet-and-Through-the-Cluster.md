---
title: 步驟5從網際網路和叢集測試 DirectAccess 連線能力
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f8880f60104cc67ca5c25c9c4fbf4ee6c9c1d1f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819201"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>步驟5從網際網路和叢集測試 DirectAccess 連線能力

>適用於：Windows Server (半年通道)、Windows Server 2016

CLIENT1 現在已準備好進行 DirectAccess 測試。  
  
- 測試來自網際網路的 DirectAccess 連線能力。 將 CLIENT1 連接到模擬的網際網路。 連線到模擬的網際網路時，會將公用的 IPv4 位址指派給用戶端。 將公用 IPv4 位址指派給 DirectAccess 用戶端時，它會嘗試使用 IPv6 轉換技術來建立遠端存取服務器的連線。  
  
- 測試透過叢集的 DirectAccess 用戶端連線能力。 測試叢集功能。 在開始測試之前，建議您至少關閉5分鐘的 EDGE1 和 EDGE2。 有許多原因會造成此問題，包括 ARP 快取超時和 NLB 的相關變更。 在測試實驗室中驗證 NLB 設定時，您必須耐心等候，因為設定中的變更將不會立即反映在連線中，直到經過一段時間為止。 當您執行下列工作時，請務必記住這一點。  
  
    > [!TIP]  
    > 建議您在執行此程式之前，先清除 Internet Explorer 快取，並在每次透過不同的遠端存取服務器測試連線，以確定您正在測試連接，而不是從快取中抓取網頁。  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>測試來自網際網路的 DirectAccess 連線能力  
  
1. 從公司網路交換器拔下 CLIENT1，並將它連接到網際網路交換器。 等候30秒。  
  
2. 在提高許可權的 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。 這會清除用戶端電腦連線到公司網路時，用戶端 DNS 快取中可能仍存在的名稱解析專案。  
  
3. 在 Windows PowerShell 視窗中，輸入**get-dnsclientnrptpolicy** ，然後按 enter。  
  
   輸出會顯示「名稱解析原則表格」(NRPT) 的目前設定。 這些設定表示 corp.contoso.com 的所有連接都應該由遠端存取 DNS 伺服器解析，IPv6 位址為2001： db8：1：：2。 另外，請注意指出名稱 nls.corp.contoso.com 獲得豁免的 NRPT 項目；豁免清單上的名稱不會得到「遠端存取」DNS 伺服器的回應。 您可以 ping 「遠端存取」 DNS 伺服器 IP 位址，以確認遠端存取服務器的連線能力;例如，您可以 ping 2001： db8：1：：2。  
  
4. 在 Windows PowerShell 視窗中，輸入**ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址的回復，在此案例中為2001： db8：1：：3。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app2** ，然後按 enter。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。  
  
   Ping APP2 的能力很重要，因為成功表示您能夠使用 NAT64/DNS64 建立連接，因為 APP2 是僅 IPv4 的資源。  
  
6. 讓 Windows PowerShell 視窗保持開啟，以進行下一個程式。  
  
7. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入 **https://app1/** ，然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
8. 在 Internet Explorer 網址列中，輸入 **https://app2/** ，然後按 enter。 您將會看到 APP2 上的預設網站。  
  
9. 在 [**開始**] 畫面上，輸入<strong>\\\App2\Files</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。  
  
    這會示範您可以使用 SMB 連線到僅 IPv4 的伺服器，以取得資源網域中的資源。  
  
10. 在 [**開始**] 畫面上，輸入**wf**，然後按 enter。  
  
11. 請注意，在 [**具有 Advanced Security 的 Windows 防火牆**] 主控台中，只有**私人**或**公用設定檔**為作用中。 必須啟用 Windows 防火牆，DirectAccess 才能正常運作。 如果 Windows 防火牆已停用，DirectAccess 連線就無法運作。  
  
12. 在主控台的左窗格中，展開 [**監視**] 節點，然後按一下 [連線**安全性規則**] 節點。 您應該會看到使用中的連線安全性規則： **Directaccess 原則-ClientToCorp**、 **Directaccess 原則-ClientToDNS64NAT64PrefixExemption**、 **Directaccess 原則-ClientToInfra**和**directaccess 原則-ClientToNlaExempt**。 將中間窗格向右移動，以顯示 [**第一個驗證方法**] 和 [**第二個驗證方法**] 資料行。 請注意，第一個規則（ClientToCorp）會使用 Kerberos V5 來建立內部網路通道，而第三個規則（ClientToInfra）則使用 NTLMv2 來建立基礎結構通道。  
  
13. 在主控台的左窗格中，展開 [**安全性關聯**] 節點，然後按一下 [**主要模式]** 節點。 請注意，使用 NTLMv2 的基礎結構通道安全性關聯和使用 Kerberos V5 的內部網路通道安全性關聯。 以滑鼠右鍵按一下顯示 [**使用者（Kerberos V5）** ] 做為**第二個驗證方法**的專案，然後按一下 [**屬性**]。 在 [**一般**] 索引標籤上，請注意**第二個驗證本機識別碼**是**CORP\User1**，表示 User1 能夠使用 Kerberos 成功地向 CORP 網域進行驗證。  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>測試透過叢集的 DirectAccess 用戶端連線能力  
  
1. 在 EDGE2 上執行正常關機。  
  
   執行這些測試時，您可以使用「網路負載平衡管理員」來查看伺服器的狀態。  
  
2. 在 CLIENT1 的 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。 這會清除可能仍存在於用戶端 DNS 快取中的名稱解析專案。  
  
3. 在 Windows PowerShell 視窗中，偵測 APP1 和 APP2。 您應該會收到這兩個資源的回復。  
  
4. 在 [**開始**] 畫面上，輸入<strong>\\\app2\files</strong>。 您應該會看到 APP2 電腦上的共用資料夾。 在 APP2 上開啟檔案共用的功能指出第二個通道（需要使用者的 Kerberos 驗證）運作正常。  
  
5. 開啟 Internet Explorer，然後開啟 [網站] https://app1/ 並 https://app2/。 開啟這兩個網站的功能可確認第一個和第二個通道都已啟動且正常運作。 關閉 Internet Explorer。  
  
6. 啟動 EDGE2 電腦。  
  
7. 在 EDGE1 上執行正常關機。  
  
8. 等待5分鐘，然後再返回 CLIENT1。 執行步驟2-5。 這會確認 CLIENT1 在 EDGE1 變成無法使用之後，能夠以透明的方式故障切換至 EDGE2。
