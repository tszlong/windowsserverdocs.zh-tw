---
title: 步驟13從 NAT 裝置後方測試 DirectAccess 連線能力
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41701592c0d9b143c84ad3fbad3fd77491eff5a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404717"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>步驟13從 NAT 裝置後方測試 DirectAccess 連線能力

>適用於：Windows Server (半年度管道)、Windows Server 2016

將 DirectAccess 用戶端從 NAT 裝置或 Web Proxy 伺服器後方連線到網際網路時，DirectAccess 用戶端會使用 Teredo 或 IP-HTTPS 來連線到「遠端存取」伺服器。 如果 NAT 裝置啟用輸出 UDP 埠3544至遠端存取服務器的公用 IP 位址，則會使用 Teredo。 如果無法使用 Teredo 存取，DirectAccess 用戶端就會回復成經由輸出 TCP 連接埠 443 的 IP-HTTPS，這可允許透過防火牆或 Web Proxy 伺服器經由傳統 SSL 連接埠存取。 如果 Web Proxy 要求驗證，IP-HTTPS 連線將會失敗。 如果 Web Proxy 執行輸出 SSL 檢查，IP-HTTPS 連線也會失敗，因為 HTTPS 工作階段會在 Web Proxy 被終止，而不是在「遠端存取」伺服器被終止。  
  
下列程序會在這兩部用戶端電腦上執行：  
  
1. 測試 Teredo 連線能力。 當 DirectAccess 用戶端設定為使用 Teredo 時，會執行第一組測試。 這是當 NAT 裝置允許對 UDP 連接埠 3544 進行輸出存取時的自動設定。 先在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。  
  
2. 測試 IP-HTTPS 連線能力。 當 DirectAccess 用戶端設定為使用 IP-HTTPS 時，會執行第二組測試。 為了示範 IP-HTTPS 連線，會在用戶端電腦上停用 Teredo。 先在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。  
  
## <a name="prerequisites"></a>必要條件  
啟動 EDGE1 和 2-EDGE1 （如果尚未執行），並確定它們已連線到網際網路子網。  
  
執行這些測試之前，請先從網際網路交換器拔下 CLIENT1 和 CLIENT2，然後將它們連接到 Homenet 交換器。 如果系統詢問您想要定義目前網路的網路類型，請選取 [**家用網路**]。  
  
## <a name="TeredoCLIENT1"></a>測試 Teredo 連線能力  
  
1. 在 CLIENT1 上，開啟提升許可權的 Windows PowerShell 視窗。  
  
2. 啟用 Teredo 介面卡，輸入**netsh interface Teredo set state enterpriseclient**，然後按 enter。  
  
3. 在 Windows PowerShell 視窗中，輸入**ipconfig/all** ，然後按 enter。  
  
4. 檢查 ipconfig 命令的輸出。  
  
   這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 當 DirectAccess 用戶端位於 NAT 裝置後方並且被指派私人 IPv4 位址時，慣用的 IPv6 轉換技術是 Teredo。 如果您查看 ipconfig 命令的輸出，您應該會看到「通道介面卡 Teredo 通道虛擬介面」的區段，然後是描述「Microsoft Teredo 通道介面卡」，其 IP 位址開頭為2001:0，且其為 Teredo應對. 您應該會看到為 Teredo 通道介面卡列出的預設閘道為 '：： '。  
  
5. 在 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。  
  
   這會將自用戶端電腦連線到網際網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址回復，在此案例中為 fd**c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping 2-app1** ，然後按 enter。 您應該會看到來自 IPv6 位址 2-APP1，2001： db8：2：：3的回復。  
  
9. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入 **https://2-app1/** ，然後按 enter 鍵。 您會在 2-APP1 上看到預設的 IIS 網站。  
  
10. 在 Internet Explorer 網址列中，輸入 **https://app2/** ，然後按 enter。 您將會看到 APP2 上的預設網站。  
  
11. 在 [**開始**] 畫面上，輸入<strong>\\ \ App2\Files</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。  
  
12. 在 CLIENT2 上重複此程式。  
  
## <a name="IPHTTPS_CLIENT1"></a>測試 IP-HTTPS 連線能力  
  
1. 在 CLIENT1 上，開啟提升許可權的 Windows PowerShell 視窗，並輸入**netsh interface teredo set state disabled** ，然後按 enter。 這會在用戶端電腦上停用 Teredo，並讓用戶端電腦設定它自己使用 IP-HTTPS。 當命令完成時，會出現 [確定] 回應。  
  
2. 在 Windows PowerShell 視窗中，輸入**ipconfig/all** ，然後按 enter。  
  
3. 檢查 ipconfig 命令的輸出。 這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 Teredo 已被停用，而 DirectAccess 用戶端則回復成 IP-HTTPS。 當您查看 ipconfig 命令的輸出時，您會看到通道介面卡 ipHTTPsinterface 的區段，其 IP 位址開頭為2001： db8：1：1000或2001： db8：2：2000，這是以下列首碼為基礎的 ip-HTTPs 位址設定 DirectAccess 時設定。 您不會看到針對 IPHTTPSInterface 通道介面卡列出的預設閘道。  
  
4. 在 Windows PowerShell 視窗中，輸入**ipconfig/flushdns** ，然後按 enter。 這會將自用戶端電腦連線到公司網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app1** ，然後按 enter。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app2** ，然後按 enter。 您應該會看到從 EDGE1 指派給 APP2 的 NAT64 位址回復，在此案例中為 fd**c9：9f4e： eb1b**：7777：： a00：4。 請注意，粗體值會因產生位址的方式而有所不同。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping 2-app1** ，然後按 enter。 您應該會看到來自 IPv6 位址 2-APP1，2001： db8：2：：3的回復。  
  
8. 開啟 Internet Explorer，在 Internet Explorer 網址列中輸入 **https://2-app1/** ，然後按 enter 鍵。 您會在 2-APP1 上看到預設的 IIS 網站。  
  
9. 在 Internet Explorer 網址列中，輸入 **https://app2/** ，然後按 enter。 您將會看到 APP2 上的預設網站。  
  
10. 在 [**開始**] 畫面上，輸入<strong>\\ \ App2\Files</strong>，然後按 enter。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。  
  
11. 在 CLIENT2 上重複此程式。  
  


