---
title: 步驟 5-從網際網路和透過叢集測試 DirectAccess 連線能力
description: 瞭解如何從網際網路和透過叢集測試 DirectAccess 連線能力。
manager: brianlic
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e301c9786b0d76e45c8417a7a3c9340937f5f529
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039208"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>步驟 5-從網際網路和透過叢集測試 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

CLIENT1 現在已準備好進行 DirectAccess 測試。

- 從網際網路測試 DirectAccess 連線能力。 將 CLIENT1 連線到模擬的網際網路。 連線到模擬的網際網路時，會指派公用 IPv4 位址給用戶端。 將公用 IPv4 位址指派給 DirectAccess 用戶端時，它會嘗試使用 IPv6 轉換技術來建立與遠端存取服務器的連線。

- 透過叢集中測試 DirectAccess 用戶端連線能力。 測試叢集功能。 開始測試之前，建議您至少關閉五分鐘的 EDGE1 和 EDGE2。 這有許多原因，包括 ARP 快取超時和與 NLB 相關的變更。 在測試實驗室中驗證 NLB 設定時，您必須耐心等候，因為在經過一段時間之後，設定中的變更將不會立即反映在連線中。 當您執行下列工作時，請務必記住這一點。

    > [!TIP]
    > 建議您在執行此程式之前清除 Internet Explorer 快取，並在每次透過不同的遠端存取服務器測試連線時，確定您要測試連線，而不是從快取中抓取網頁。

## <a name="test-directaccess-connectivity-from-the-internet"></a>從網際網路測試 DirectAccess 連線能力

1. 從公司網路交換器拔下 CLIENT1，並將它連線到網際網路交換器。 等候30秒。

2. 在提高許可權的 Windows PowerShell 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter。 當用戶端電腦連線到公司網路時，此專案的名稱解析專案可能仍存在於用戶端 DNS 快取中。

3. 在 [Windows PowerShell] 視窗中，輸入 **get-dnsclientnrptpolicy** ，然後按 enter。

   輸出會顯示「名稱解析原則表格」(NRPT) 的目前設定。 這些設定表示遠端存取 DNS 伺服器應解析 corp.contoso.com 的所有連線，IPv6 位址為2001： db8：1：：2。 另外，請注意指出名稱 nls.corp.contoso.com 獲得豁免的 NRPT 項目；豁免清單上的名稱不會得到「遠端存取」DNS 伺服器的回應。 您可以 ping 遠端存取 DNS 伺服器 IP 位址，以確認遠端存取服務器的連線能力;例如，您可以 ping 2001： db8：1：：2。

4. 在 [Windows PowerShell] 視窗中，輸入 **ping app1** ，然後按 enter。 您應該會看到 APP1 IPv6 位址的回復，在此案例中為2001： db8：1：：3。

5. 在 [Windows PowerShell] 視窗中，輸入 **ping app2** ，然後按 enter。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。

   偵測 APP2 的能力很重要，因為成功表示您可以使用 NAT64/DNS64 建立連接，因為 APP2 是僅限 IPv4 的資源。

6. 讓 Windows PowerShell 視窗保持開啟，以供下一個程式。

7. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入， **https://app1/** 然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 網站。

8. 在 Internet Explorer 網址列中，輸入， **https://app2/** 然後按 enter 鍵。 您將會看到 APP2 上的預設網站。

9. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。

    這會示範您可以使用 SMB 連線到僅 IPv4 的伺服器，以取得資源網域中的資源。

10. 在 [ **開始** ] 畫面上，輸入 **services.msc**，然後按 enter。

11. 在 [ **具有 Advanced Security 的 Windows 防火牆** ] 主控台中，請注意，只有 **私人** 或 **公用設定檔** 為作用中。 必須啟用 Windows 防火牆，DirectAccess 才能正常運作。 如果停用 Windows 防火牆，DirectAccess 連線無法運作。

12. 在主控台的左窗格中，展開 [ **監視** ] 節點，然後按一下 [ **連接安全性規則** ] 節點。 您應該會看到使用中的連線安全性規則： **Directaccess 原則-ClientToCorp**、 **Directaccess 原則-ClientToDNS64NAT64PrefixExemption**、 **Directaccess 原則-ClientToInfra** 和 **directaccess 原則-ClientToNlaExempt**。 將中間窗格向右滾動，以顯示 **第 1** 個 [驗證方法] 和 [ **第二個驗證方法** ] 資料行。 請注意，第一個規則 (ClientToCorp) 使用 Kerberos V5 來建立內部網路通道，而第三個規則 (ClientToInfra) 使用 NTLMv2 來建立基礎結構通道。

13. 在主控台的左窗格中，展開 [ **安全性關聯** ] 節點，然後按一下 [ **主要模式** ] 節點。 請注意使用 NTLMv2 的基礎結構通道安全性關聯，以及使用 Kerberos V5 的內部網路通道安全性關聯。 以滑鼠右鍵按一下顯示 **使用者 (Kerberos V5)** 作為 **第二個驗證方法** 的專案， **然後按一下 [** 內容]。 在 [ **一般** ] 索引標籤上，請注意 **第二個驗證本機識別碼** 是 **CORP\User1**，表示 USER1 能夠使用 Kerberos 成功驗證 CORP 網域。

## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>透過叢集中測試 DirectAccess 用戶端連線能力

1. 在 EDGE2 上執行正常關機。

   您可以使用 [網路負載平衡管理員]，在執行這些測試時查看伺服器的狀態。

2. 在 CLIENT1 的 [Windows PowerShell] 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter 鍵。 這會清除可能仍存在於用戶端 DNS 快取中的名稱解析專案。

3. 在 [Windows PowerShell] 視窗中，ping APP1 和 APP2。 您應該會收到這兩個資源的回復。

4. 在 [**開始**] 畫面上，輸入 <strong> \\ \app2\files</strong>。 您應該會看到 APP2 電腦上的共用資料夾。 在 APP2 上開啟檔案共用的功能，表示第二個通道（需要使用者的 Kerberos 驗證）正常運作。

5. 開啟 Internet Explorer，然後開啟網站 https://app1/ 和 https://app2/ 。 開啟這兩個網站的功能，可確認第一個和第二個通道都已啟動且正常運作。 關閉 Internet Explorer。

6. 啟動 EDGE2 電腦。

7. 在 EDGE1 上執行正常關機。

8. 等候5分鐘，然後返回 CLIENT1。 執行步驟2-5。 這可確認 CLIENT1 在 EDGE1 變成無法使用之後，能夠以透明方式容錯移轉至 EDGE2。
