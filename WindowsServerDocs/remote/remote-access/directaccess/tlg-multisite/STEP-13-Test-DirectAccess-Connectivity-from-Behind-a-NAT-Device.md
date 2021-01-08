---
title: 步驟13從 NAT 裝置後方測試 DirectAccess 連線能力
description: 瞭解如何在兩部用戶端電腦上測試 Teredo 連線能力和 ip-HTTPs 連線。
manager: brianlic
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 60cd8351a877fe2c66fdd2392348e3efbc26e6cc
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040518"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>步驟13從 NAT 裝置後方測試 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

將 DirectAccess 用戶端從 NAT 裝置或 Web Proxy 伺服器後方連線到網際網路時，DirectAccess 用戶端會使用 Teredo 或 IP-HTTPS 來連線到「遠端存取」伺服器。 如果 NAT 裝置啟用輸出 UDP 埠3544至遠端存取服務器的公用 IP 位址，則會使用 Teredo。 如果無法使用 Teredo 存取，DirectAccess 用戶端就會回復成經由輸出 TCP 連接埠 443 的 IP-HTTPS，這可允許透過防火牆或 Web Proxy 伺服器經由傳統 SSL 連接埠存取。 如果 Web Proxy 要求驗證，IP-HTTPS 連線將會失敗。 如果 Web Proxy 執行輸出 SSL 檢查，IP-HTTPS 連線也會失敗，因為 HTTPS 工作階段會在 Web Proxy 被終止，而不是在「遠端存取」伺服器被終止。

下列程序會在這兩部用戶端電腦上執行：

1. 測試 Teredo 連線能力。 當 DirectAccess 用戶端設定為使用 Teredo 時，會執行第一組測試。 這是當 NAT 裝置允許對 UDP 連接埠 3544 進行輸出存取時的自動設定。 先在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。

2. 測試 IP-HTTPS 連線能力。 當 DirectAccess 用戶端設定為使用 IP-HTTPS 時，會執行第二組測試。 為了示範 IP-HTTPS 連線，會在用戶端電腦上停用 Teredo。 先在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。

## <a name="prerequisites"></a>先決條件
啟動 EDGE1 和 2 EDGE1 （如果尚未執行），並確定它們已連線到網際網路子網。

執行這些測試之前，請從網際網路交換器中拔下 CLIENT1 和 CLIENT2，並將它們連線到 Homenet 交換器。 如果系統詢問您想要定義目前網路的網路類型，請選取 [ **家用網路**]。

## <a name="test-teredo-connectivity"></a><a name="TeredoCLIENT1"></a>測試 Teredo 連線

1. 在 CLIENT1 上，開啟提高許可權的 Windows PowerShell 視窗。

2. 啟用 Teredo 介面卡，輸入 **netsh interface Teredo set state enterpriseclient**，然後按 enter。

3. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/all** ，然後按 enter 鍵。

4. 檢查 ipconfig 命令的輸出。

   這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 當 DirectAccess 用戶端位於 NAT 裝置後方並且被指派私人 IPv4 位址時，慣用的 IPv6 轉換技術是 Teredo。 如果您查看 ipconfig 命令的輸出，您應該會看到「通道介面卡 Teredo 通道 Pseudo-Interface」的區段，然後是描述「Microsoft Teredo 通道介面卡」，其中 IP 位址的開頭為2001:0 且與 Teredo 位址一致。 您應該會看到針對 Teredo 通道介面卡列為 '：： ' 的預設閘道。

5. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter。

   這會將自用戶端電腦連線到網際網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。

6. 在 [Windows PowerShell] 視窗中，輸入 **ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。

7. 在 [Windows PowerShell] 視窗中，輸入 **ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址的回復，在此案例中為 fd **c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。

8. 在 [Windows PowerShell] 視窗中，輸入 **ping 2-app1** ，然後按 enter。 您應該會看到來自 IPv6 位址 2-APP1，2001： db8：2：：3的回復。

9. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入， **https://2-app1/** 然後按 enter 鍵。 您將會在 2 APP1 上看到預設的 IIS 網站。

10. 在 Internet Explorer 網址列中，輸入， **https://app2/** 然後按 enter 鍵。 您將會看到 APP2 上的預設網站。

11. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。

12. 在 CLIENT2 上重複此程式。

## <a name="test-ip-https-connectivity"></a><a name="IPHTTPS_CLIENT1"></a>測試 IP-HTTPS 連線

1. 在 CLIENT1 上，開啟提高許可權的 Windows PowerShell 視窗，然後輸入 **netsh interface teredo set state disabled** ，然後按 enter。 這會在用戶端電腦上停用 Teredo，並讓用戶端電腦設定它自己使用 IP-HTTPS。 當命令完成時，會出現 [確定] 回應。

2. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/all** ，然後按 enter 鍵。

3. 檢查 ipconfig 命令的輸出。 這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 Teredo 已被停用，而 DirectAccess 用戶端則回復成 IP-HTTPS。 當您查看 ipconfig 命令的輸出時，您會看到 [通道介面卡] ipHTTPsinterface」的區段，其中 IP 位址的開頭為2001： db8：1：1000或2001： db8：2：2000與設定 DirectAccess 時所設定的前置詞一致，這是 ip-HTTPs 位址。 您將不會看到針對 IpHTTPsinterface」通道介面卡列出的預設閘道。

4. 在 [Windows PowerShell] 視窗中，輸入 **ipconfig/flushdns** ，然後按 enter。 這會將自用戶端電腦連線到公司網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。

5. 在 [Windows PowerShell] 視窗中，輸入 **ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。

6. 在 [Windows PowerShell] 視窗中，輸入 **ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址的回復，在此案例中為 fd **c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。

7. 在 [Windows PowerShell] 視窗中，輸入 **ping 2-app1** ，然後按 enter。 您應該會看到來自 IPv6 位址 2-APP1，2001： db8：2：：3的回復。

8. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入， **https://2-app1/** 然後按 enter 鍵。 您將會在 2 APP1 上看到預設的 IIS 網站。

9. 在 Internet Explorer 網址列中，輸入， **https://app2/** 然後按 enter 鍵。 您將會看到 APP2 上的預設網站。

10. 在 [**開始**] 畫面上，輸入 <strong> \\ \APP2\FILES</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。

11. 在 CLIENT2 上重複此程式。



