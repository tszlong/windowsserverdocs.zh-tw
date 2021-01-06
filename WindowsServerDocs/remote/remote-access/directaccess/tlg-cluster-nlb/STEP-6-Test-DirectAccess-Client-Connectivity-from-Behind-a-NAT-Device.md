---
title: 步驟6測試從 NAT 裝置後方的 DirectAccess 用戶端連線能力
description: 本主題是測試實驗室指南的一部分-示範 Windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 083905f03a9a8f021d223f2ab0e7f0a5141f0e42
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946544"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>步驟6測試從 NAT 裝置後方的 DirectAccess 用戶端連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

將 DirectAccess 用戶端從 NAT 裝置或 Web Proxy 伺服器後方連線到網際網路時，DirectAccess 用戶端會使用 Teredo 或 IP-HTTPS 來連線到「遠端存取」伺服器。

如果 NAT 裝置啟用輸出 UDP 埠3544至遠端存取服務器的公用 IP 位址，則會使用 Teredo。 如果無法使用 Teredo 存取，DirectAccess 用戶端就會回復成經由輸出 TCP 連接埠 443 的 IP-HTTPS，這可允許透過防火牆或 Web Proxy 伺服器經由傳統 SSL 連接埠存取。

如果 Web Proxy 要求驗證，IP-HTTPS 連線將會失敗。 如果 Web Proxy 執行輸出 SSL 檢查，IP-HTTPS 連線也會失敗，因為 HTTPS 工作階段會在 Web Proxy 被終止，而不是在「遠端存取」伺服器被終止。 在本節中，您將執行與前一節中使用 6to4 連線進行連線時所執行的相同測試。

下列程序會在這兩部用戶端電腦上執行：

1. 測試 Teredo 連線能力。 當 DirectAccess 用戶端設定為使用 Teredo 時，會執行第一組測試。 這是當 NAT 裝置允許對 UDP 連接埠 3544 進行輸出存取時的自動設定。

2. 測試 IP-HTTPS 連線能力。 當 DirectAccess 用戶端設定為使用 IP-HTTPS 時，會執行第二組測試。 為了示範 IP-HTTPS 連線，會在用戶端電腦上停用 Teredo。

> [!TIP]
> 建議您在執行這些程式之前清除 Internet Explorer 快取，以確保您正在測試連線，而不是從快取中取出網站頁面。

## <a name="prerequisites"></a>先決條件

執行這些測試之前，請先將 CLIENT1 從網際網路交換器拔除，然後連接到家用網路交換器。 如果詢問您要將目前的網路定義為哪一種類型的網路，請選取 [家用網路]。

如果 EDGE1 和 EDGE2 尚未執行，請啟動它們。

## <a name="test-teredo-connectivity"></a>測試 Teredo 連線

1. 在 CLIENT1 上，開啟提高許可權的 Windows PowerShell 視窗中，輸入 **ipconfig/all** ，然後按 enter 鍵。

2. 檢查 ipconfig 命令的輸出。

   CLIENT1 現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 當 DirectAccess 用戶端位於 NAT 裝置後方並且被指派私人 IPv4 位址時，慣用的 IPv6 轉換技術是 Teredo。 如果您查看 ipconfig 命令的輸出，應該會看到「通道介面卡 Teredo 通道虛擬介面」的區段，然後會有一個「Microsoft Teredo 通道介面卡」描述，含有一個開頭為 2001 的 IP 位址：與 Teredo 位址相符。 如果您沒有看到 Teredo 區段，請使用下列命令來啟用 Teredo： **netsh Interface teredo set state enterpriseclient** ，然後重新執行 ipconfig 命令。 您不會看到列出 Teredo 通道介面卡的預設閘道。

3. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter。

   這會將自用戶端電腦連線到網際網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。

4. 在 [Windows PowerShell] 視窗中，輸入 **get-dnsclientnrptpolicy** ，然後按 enter。

   輸出會顯示「名稱解析原則表格」(NRPT) 的目前設定。 這些設定會指出所有連到 .corp.contoso.com 的連線都應該由 IPv6 位址為 2001:db8:1::2 的「遠端存取」DNS 伺服器解析。 另外，請注意指出名稱 nls.corp.contoso.com 獲得豁免的 NRPT 項目；豁免清單上的名稱不會得到「遠端存取」DNS 伺服器的回應。 您可以 ping「遠端存取」DNS 伺服器 IP 位址，以確認是否可以連線到「遠端存取」伺服器；例如在這個範例中，您可以 ping 2001:db8:1::2。

5. 在 [Windows PowerShell] 視窗中，輸入 **ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。

6. 在 [Windows PowerShell] 視窗中，輸入 **ping app2** ，然後按 enter。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。

7. 讓 Windows PowerShell 視窗保持開啟，以供下一個程式。

8. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入， **https://app1/** 然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 網站。

9. 在 Internet Explorer 網址列中，輸入， **https://app2/** 然後按 enter 鍵。 您將會看到 APP2 上的預設網站。

10. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。

## <a name="test-ip-https-connectivity"></a>測試 IP-HTTPS 連線

1. 開啟提高許可權的 Windows PowerShell 視窗，輸入 **netsh interface teredo set state disabled** ，然後按 enter。 這會在用戶端電腦上停用 Teredo，並讓用戶端電腦設定它自己使用 IP-HTTPS。 當命令完成時，會出現 [確定] 回應。

2. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/all** ，然後按 enter 鍵。

3. 檢查 ipconfig 命令的輸出。 這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 Teredo 已被停用，而 DirectAccess 用戶端則回復成 IP-HTTPS。 當您檢視 ipconfig 命令的輸出時，會看到「通道介面卡 iphttpsinterface」的區段，含有一個開頭為 2001:db8:1:100 的 IP 位址，根據設定 DirectAccess 時所設定的首碼，此位址與 IP-HTTPS 位址相符。 您不會看到列出 IP-HTTPS 通道介面卡的預設閘道。

4. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter。 這會將自用戶端電腦連線到公司網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。

5. 在 [Windows PowerShell] 視窗中，輸入 **ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。

6. 在 [Windows PowerShell] 視窗中，輸入 **ping app2** ，然後按 enter。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。

7. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入， **https://app1/** 然後按 enter 鍵。 您將會看到 APP1 上的預設 IIS 站台。

8. 在 Internet Explorer 網址列中，輸入， **https://app2/** 然後按 enter 鍵。 您將會看到 APP2 上的預設網站。

9. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。
