---
title: 從 NAT 裝置後方的步驟 6 的測試 DirectAccess 用戶端連線
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d61bf2ec9abb6d54617f624f26680263d761e5b6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446086"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>從 NAT 裝置後方的步驟 6 的測試 DirectAccess 用戶端連線

>適用於：Windows Server （半年通道），Windows Server 2016

將 DirectAccess 用戶端從 NAT 裝置或 Web Proxy 伺服器後方連線到網際網路時，DirectAccess 用戶端會使用 Teredo 或 IP-HTTPS 來連線到「遠端存取」伺服器。 

如果 NAT 裝置啟用輸出 UDP 連接埠 3544 為遠端存取伺服器的公用 IP 位址，則會使用 Teredo。 如果無法使用 Teredo 存取，DirectAccess 用戶端就會回復成經由輸出 TCP 連接埠 443 的 IP-HTTPS，這可允許透過防火牆或 Web Proxy 伺服器經由傳統 SSL 連接埠存取。 

如果 Web Proxy 要求驗證，IP-HTTPS 連線將會失敗。 如果 Web Proxy 執行輸出 SSL 檢查，IP-HTTPS 連線也會失敗，因為 HTTPS 工作階段會在 Web Proxy 被終止，而不是在「遠端存取」伺服器被終止。 在本節中，您將執行與前一節中使用 6to4 連線進行連線時所執行的相同測試。  
  
下列程序會在這兩部用戶端電腦上執行：  
  
1. 測試 Teredo 連線。 當 DirectAccess 用戶端設定為使用 Teredo 時，會執行第一組測試。 這是當 NAT 裝置允許對 UDP 連接埠 3544 進行輸出存取時的自動設定。  
  
2. 測試 IP-HTTPS 連線。 當 DirectAccess 用戶端設定為使用 IP-HTTPS，則會執行第二組測試。 為了示範 IP-HTTPS 連線，會在用戶端電腦上停用 Teredo。  
  
> [!TIP]  
> 建議您清除了 Internet Explorer 快取，再執行這些程序，以確保您要測試連線並不從快取抓取網站頁面。  
  
## <a name="prerequisites"></a>先決條件

執行這些測試之前，請先將 CLIENT1 從網際網路交換器拔除，然後連接到家用網路交換器。 如果詢問您要將目前的網路定義為哪一種類型的網路，請選取 [家用網路]  。  
  
如果 EDGE1 和 EDGE2 尚未執行，請啟動它們。  
  
## <a name="test-teredo-connectivity"></a>測試 Teredo 連線  
  
1. 在 CLIENT1 上，開啟提升權限的 Windows PowerShell 視窗中，型別**ipconfig /all**按 ENTER 鍵。  
  
2. 檢查 ipconfig 命令的輸出。  
  
   CLIENT1 現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 當 DirectAccess 用戶端位於 NAT 裝置後方並且被指派私人 IPv4 位址時，慣用的 IPv6 轉換技術是 Teredo。 如果您查看 ipconfig 命令的輸出時，您應該會看到通道介面卡 Teredo 通道虛擬介面，然後描述 Microsoft Teredo 通道介面卡，開頭為 2001年的 IP 位址的區段： 與正在 Teredo 一致地址。 如果您看不到 Teredo 區段，請使用下列命令啟用 Teredo: **netsh interface Teredo 設定狀態 enterpriseclient** ，然後重新執行 ipconfig 命令。 您不會看到列出 Teredo 通道介面卡的預設閘道。  
  
3. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。  
  
   這會將自用戶端電腦連線到網際網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
4. 在 Windows PowerShell 視窗中，輸入**Get-dnsclientnrptpolicy**按 ENTER 鍵。  
  
   輸出會顯示「名稱解析原則表格」(NRPT) 的目前設定。 這些設定會指出所有連到 .corp.contoso.com 的連線都應該由 IPv6 位址為 2001:db8:1::2 的「遠端存取」DNS 伺服器解析。 另外，請注意指出名稱 nls.corp.contoso.com 獲得豁免的 NRPT 項目；豁免清單上的名稱不會得到「遠端存取」DNS 伺服器的回應。 您可以 ping「遠端存取」DNS 伺服器 IP 位址，以確認是否可以連線到「遠端存取」伺服器；例如在這個範例中，您可以 ping 2001:db8:1::2。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。  
  
7. 讓 Windows PowerShell 視窗開啟，以供下一個程序。  
  
8. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://app1/** 按 ENTER 鍵。 您將會看到 APP1 上的預設 IIS 網站。  
  
9. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
10. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。  
  
## <a name="test-ip-https-connectivity"></a>測試 IP-HTTPS 連線  
  
1. 開啟提升權限的 Windows PowerShell 視窗，輸入**netsh 介面 teredo 設定停用狀態**按 ENTER 鍵。 這會在用戶端電腦上停用 Teredo，並讓用戶端電腦設定它自己使用 IP-HTTPS。 當命令完成時，會出現 [確定]  回應。  
  
2. 在 Windows PowerShell 視窗中，輸入**ipconfig /all**按 ENTER 鍵。  
  
3. 檢查 ipconfig 命令的輸出。 這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 Teredo 已被停用，而 DirectAccess 用戶端則回復成 IP-HTTPS。 當您查看 ipconfig 命令的輸出時，您會看到開頭為 2001:db8:1:100 以此方式所設定時所設定的前置詞為基礎的 IP-HTTPS 位址的 IP 位址的通道介面卡 iphttpsinterface 的區段DirectAccess。 您不會看到列出 IP-HTTPS 通道介面卡的預設閘道。  
  
4. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。 這會將自用戶端電腦連線到公司網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給 APP2 之 NAT64 位址 (在此案例中為 fdc9:9f4e:eb1b:7777::a00:4) 的回覆。  
  
7. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://app1/** 按 ENTER 鍵。 您將會看到 APP1 上的預設 IIS 站台。  
  
8. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
9. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。
