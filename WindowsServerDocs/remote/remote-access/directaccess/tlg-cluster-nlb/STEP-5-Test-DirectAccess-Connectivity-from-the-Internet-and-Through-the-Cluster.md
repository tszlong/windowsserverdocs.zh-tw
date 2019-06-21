---
title: 步驟 5 的測試 DirectAccess 連線從網際網路，並透過叢集
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23aed915edb827fd0cd61e6778167108647269ea
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283348"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>步驟 5 的測試 DirectAccess 連線從網際網路，並透過叢集

>適用於：Windows Server （半年通道），Windows Server 2016

CLIENT1 已準備好進行測試的 DirectAccess。  
  
- 測試從網際網路的 DirectAccess 連線。 連線到模擬網際網路的 CLIENT1。 當連線到模擬網際網路時，用戶端會指派公用 IPv4 位址。 當 DirectAccess 用戶端指派的公用 IPv4 位址時，它會嘗試連接到遠端存取伺服器使用 IPv6 轉換技術。  
  
- 測試叢集內的 DirectAccess 用戶端連線。 測試叢集功能。 在開始測試之前，我們建議您關閉 EDGE1 和 EDGE2 至少五分鐘。 有多種原因所造成，包括 ARP 快取逾時以及與 NLB 相關的變更。 當驗證的測試實驗室中的 NLB 設定時，您必須在組態中的變更將不會立即反映在連線，直到經過一段時間之後，請耐心。 這很重要要牢記在心，當您執行下列工作。  
  
    > [!TIP]  
    > 我們建議您執行此程序之前清除 Internet Explorer 快取，並每次測試透過不同的遠端存取伺服器，藉此確定您已用來測試連線，且無法從快取中擷取網頁的連線。  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>測試從網際網路的 DirectAccess 連線  
  
1. 從公司網路交換器拔除 CLIENT1，並將它連線到網際網路交換器。 等候 30 秒。  
  
2. 在提升權限的 Windows PowerShell 視窗中輸入**ipconfig /flushdns**按 ENTER 鍵。 這會排清可能仍存在於用戶端 DNS 快取中時用戶端電腦連線到公司網路的名稱解析項目。  
  
3. 在 Windows PowerShell 視窗中，輸入**Get-dnsclientnrptpolicy**按 ENTER 鍵。  
  
   輸出會顯示「名稱解析原則表格」(NRPT) 的目前設定。 這些設定會指出所有連線。 遠端存取 」 DNS 伺服器，IPv6 位址 2001:db8:1::2 因應解決 corp.contoso.com。 另外，請注意指出名稱 nls.corp.contoso.com 獲得豁免的 NRPT 項目；豁免清單上的名稱不會得到「遠端存取」DNS 伺服器的回應。 您可以 ping 遠端存取 」 DNS 伺服器 IP 位址，以確認連線到遠端存取伺服器;例如，您可以 ping 2001:db8:1::2。  
  
4. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自的 IPv6 位址為 APP1，在此案例中是 2001:db8:1::3 的回覆。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。  
  
   Ping APP2 的功能很重要，因為成功，表示您仍可連線使用 nat64/dns64，因為 APP2 IPv4 唯一的資源。  
  
6. 讓 Windows PowerShell 視窗開啟，以供下一個程序。  
  
7. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://app1/** 按 ENTER 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
8. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
9. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。  
  
    這示範了您仍可連線到 IPv4 唯一伺服器使用 SMB 來取得資源網域中的資源。  
  
10. 在 **開始**畫面上，輸入**wf.msc**，然後按 ENTER 鍵。  
  
11. 在 **具有進階安全性的 Windows 防火牆**主控台中，請注意，只有**私人**或**公用設定檔**作用中。 DirectAccess，讓它正常運作時，必須啟用 Windows 防火牆。 如果已停用 Windows 防火牆，DirectAccess 連線無法運作。  
  
12. 在主控台的左窗格中，依序展開**監視**節點，然後按一下**連線安全性規則**節點。 您應該會看到的作用中連線安全性規則：**DirectAccess 原則 ClientToCorp**， **DirectAccess 原則 ClientToDNS64NAT64PrefixExemption**， **DirectAccess 原則 ClientToInfra**，和**DirectAccess原則 ClientToNlaExempt**。 捲動至右方，才能顯示中間的窗格**第 1 個驗證方法**並**第 2 個驗證方法**資料行。 請注意，第一個規則 (ClientToCorp) 會使用 Kerberos V5，在建立內部網路通道和第三個規則 (ClientToInfra) 來建立基礎結構通道會使用 NTLMv2。  
  
13. 在主控台的左窗格中，依序展開**安全性關聯**節點，然後按一下**主要模式**節點。 請注意使用 NTLMv2 的基礎結構通道安全性關聯，並使用 Kerberos V5 內部網路通道的安全性關聯。 以滑鼠右鍵按一下顯示的項目**使用者 (Kerberos V5)** 作為**第 2 個驗證方法**然後按一下**屬性**。 在上**一般**索引標籤上，請注意**第二個驗證地區設定識別碼**是**CORP\User1**，指出 User1 可以成功進行驗證，使用 CORP 網域Kerberos。  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>測試叢集透過 DirectAccess 用戶端連線  
  
1. EDGE2 執行正常關機程序。  
  
   若要檢視伺服器的狀態，執行這些測試時，您可以使用網路負載平衡管理員。  
  
2. 在 CLIENT1 上，在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。 這會排清可能仍存在於用戶端 DNS 快取中的名稱解析項目。  
  
3. 在 Windows PowerShell 視窗中，偵測 APP1 和 APP2。 您應該從這兩個這些資源收到回覆。  
  
4. 在 **開始**畫面上，輸入<strong>\\\app2\files</strong>。 您應該會看到 APP2 電腦上的共用的資料夾。 開啟 APP2 上的檔案共用的能力會指出第二個通道，需要進行 Kerberos 驗證的使用者，正常運作。  
  
5. 開啟 Internet Explorer 中，然後再開啟網站 https://app1/ 和 https://app2/ 。 開啟兩個網站的能力可讓您確認第一個和第二個通道已啟動而且可以正常運作。 關閉 Internet Explorer。  
  
6. 啟動 EDGE2 電腦。  
  
7. 在 EDGE1 上執行正常關機程序。  
  
8. 等候 5 分鐘，然後返回 CLIENT1。 執行步驟 2-5。 這可確認 CLIENT1 已能夠以透明方式容錯移轉至 EDGE2 之後 EDGE1 變成無法使用。
