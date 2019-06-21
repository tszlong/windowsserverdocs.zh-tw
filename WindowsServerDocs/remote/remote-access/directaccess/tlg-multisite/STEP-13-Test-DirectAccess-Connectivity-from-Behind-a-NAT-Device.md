---
title: 從 NAT 裝置後方的步驟 13 測試 DirectAccess 連線
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34a65434a0035c60c888170e779496653ad0ab6d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283204"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>從 NAT 裝置後方的步驟 13 測試 DirectAccess 連線

>適用於：Windows Server （半年通道），Windows Server 2016

將 DirectAccess 用戶端從 NAT 裝置或 Web Proxy 伺服器後方連線到網際網路時，DirectAccess 用戶端會使用 Teredo 或 IP-HTTPS 來連線到「遠端存取」伺服器。 如果 NAT 裝置啟用輸出 UDP 連接埠 3544 為遠端存取伺服器的公用 IP 位址，則會使用 Teredo。 如果無法使用 Teredo 存取，DirectAccess 用戶端就會回復成經由輸出 TCP 連接埠 443 的 IP-HTTPS，這可允許透過防火牆或 Web Proxy 伺服器經由傳統 SSL 連接埠存取。 如果 Web Proxy 要求驗證，IP-HTTPS 連線將會失敗。 如果 Web Proxy 執行輸出 SSL 檢查，IP-HTTPS 連線也會失敗，因為 HTTPS 工作階段會在 Web Proxy 被終止，而不是在「遠端存取」伺服器被終止。  
  
下列程序會在這兩部用戶端電腦上執行：  
  
1. 測試 Teredo 連線。 當 DirectAccess 用戶端設定為使用 Teredo 時，會執行第一組測試。 這是當 NAT 裝置允許對 UDP 連接埠 3544 進行輸出存取時的自動設定。 第一次在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。  
  
2. 測試 IP-HTTPS 連線。 當 DirectAccess 用戶端設定為使用 IP-HTTPS，則會執行第二組測試。 為了示範 IP-HTTPS 連線，會在用戶端電腦上停用 Teredo。 第一次在 CLIENT1 上執行測試，然後在 CLIENT2 上執行測試。  
  
## <a name="prerequisites"></a>必要條件  
如果它們尚未執行，並確定它們連線到網際網路子網路，請啟動 EDGE1 和 2 EDGE1。  
  
然後再執行這些測試，將 CLIENT1 和 CLIENT2 從網際網路交換器拔除，並將它們連接到家用網路交換器。 如果系統詢問您想要定義目前的網路中，選取的網路類型**家用網路**。  
  
## <a name="TeredoCLIENT1"></a>測試 Teredo 連線  
  
1. 在 CLIENT1 上，開啟提升權限的 Windows PowerShell 視窗。  
  
2. 啟用 Teredo 配接器類型**netsh 介面 teredo 設定狀態 enterpriseclient**，然後按 ENTER 鍵。  
  
3. 在 Windows PowerShell 視窗中，輸入**ipconfig /all**按 ENTER 鍵。  
  
4. 檢查 ipconfig 命令的輸出。  
  
   這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 當 DirectAccess 用戶端位於 NAT 裝置後方並且被指派私人 IPv4 位址時，慣用的 IPv6 轉換技術是 Teredo。 如果您查看 ipconfig 命令的輸出時，您應該會看到通道介面卡 Teredo 通道虛擬介面的區段，然後描述 Microsoft Teredo 通道介面卡，以配合正在 Teredo 2001:0 開頭的 IP 位址地址。 您應該會看到列出 Teredo 通道介面卡，做為預設閘道 ':: '。  
  
5. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。  
  
   這會將自用戶端電腦連線到網際網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給在此情況下是 fd 的 APP2 之 NAT64 位址**c9:9f4e:eb1b**: 7777::a00:4。 請注意，粗體顯示的值會因地址的產生方式而有所不同。  
  
8. 在 Windows PowerShell 視窗中，輸入**ping 2-app1**按 ENTER 鍵。 您應該會看到來自 2-APP1 2001:db8:2::3 的 IPv6 位址。  
  
9. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://2-app1/** 按 ENTER 鍵。 2-APP1 上，您會看到預設 IIS 網站。  
  
10. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
11. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。  
  
12. 重複此程序在 CLIENT2 上。  
  
## <a name="IPHTTPS_CLIENT1"></a>測試 IP-HTTPS 連線  
  
1. 在 CLIENT1 上，開啟提高權限的 Windows PowerShell 視窗，然後輸入**netsh 介面 teredo 設定停用狀態**按 ENTER 鍵。 這會在用戶端電腦上停用 Teredo，並讓用戶端電腦設定它自己使用 IP-HTTPS。 當命令完成時，會出現 [確定]  回應。  
  
2. 在 Windows PowerShell 視窗中，輸入**ipconfig /all**按 ENTER 鍵。  
  
3. 檢查 ipconfig 命令的輸出。 這部電腦現在已從 NAT 裝置後方連線到網際網路，並且已被指派一個私人 IPv4 位址。 Teredo 已被停用，而 DirectAccess 用戶端則回復成 IP-HTTPS。 當您查看 ipconfig 命令的輸出時，您會看到開頭為 2001:db8:1:1000 或 2001:db8:2:2000 配合此所根據的前置詞的 IP-HTTPS 位址的 IP 位址的通道介面卡 iphttpsinterface 的區段設定 DirectAccess 時設定。 您不會看到列出 IPHTTPSInterface 通道介面卡的預設閘道。  
  
4. 在 Windows PowerShell 視窗中，輸入**ipconfig /flushdns**按 ENTER 鍵。 這會將自用戶端電腦連線到公司網路後，可能仍存在於用戶端 DNS 快取中的名稱解析項目排清。  
  
5. 在 Windows PowerShell 視窗中，輸入**ping app1**按 ENTER 鍵。 您應該會看到來自 APP1 IPv6 位址 2001:db8:1::3 的回覆。  
  
6. 在 Windows PowerShell 視窗中，輸入**ping app2**按 ENTER 鍵。 您應該會看到來自 EDGE1 指派給在此情況下是 fd 的 APP2 之 NAT64 位址**c9:9f4e:eb1b**: 7777::a00:4。 請注意，粗體顯示的值會因地址的產生方式而有所不同。  
  
7. 在 Windows PowerShell 視窗中，輸入**ping 2-app1**按 ENTER 鍵。 您應該會看到來自 2-APP1 2001:db8:2::3 的 IPv6 位址。  
  
8. 開啟 Internet Explorer，在 Internet Explorer 網址列中，輸入 **https://2-app1/** 按 ENTER 鍵。 2-APP1 上，您會看到預設 IIS 網站。  
  
9. 在 Internet Explorer 網址列中，輸入 **https://app2/** 按 ENTER 鍵。 您將會看到 APP2 上的預設網站。  
  
10. 在 **開始**畫面上，輸入<strong>\\\App2\Files</strong>，然後按 ENTER 鍵。 按兩下 [新文字文件] 檔案。 這示範了您能夠使用 SMB 連線到僅支援 IPv4 的伺服器，來取得僅支援 IPv4 之主機上的資源。  
  
11. 重複此程序在 CLIENT2 上。  
  


