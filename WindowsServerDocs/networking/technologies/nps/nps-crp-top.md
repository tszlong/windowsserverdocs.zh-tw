---
title: 連接要求處理
description: 本主題提供的網路原則伺服器連接要求處理中的 Windows Server 2016 的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>連接要求處理

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解在 Windows Server 2016 中的網路原則伺服器處理連接要求。

>[!NOTE]
>本主題中，除了下列連接要求處理的文件會提供。
> - [連接要求原則](nps-crp-crpolicies.md)
> - [領域名稱](nps-crp-realm-names.md)
> - [遠端 RADIUS 伺服器群組](nps-crp-rrsg.md)

您可以使用連接要求處理指定位置的連接要求驗證執行-本機電腦上或在遠端 RADIUS 伺服器的遠端 RADIUS 伺服器群組成員。 

如果您想要執行的網路原則 Server (NPS) 執行連接要求驗證本機伺服器，您可以使用預設連接要求原則，而不需要額外的設定。 根據預設的原則，NPS 驗證使用者與已經 account 本機網域與受信任的網域中的電腦。

如果您想要轉送連接要求遠端 NPS 或其他 RADIUS 伺服器、建立遠端 RADIUS 伺服器群組，並要求送給該遠端 RADIUS 伺服器群組連接要求原則設定。 使用此設定時，NPS 可以轉送驗證要求到任何 RADIUS 伺服器，並帳號，不受信任的網域中的使用者都可以進行驗證。

下圖顯示存取要求訊息從網路存取伺服器 RADIUS proxy，然後再入 RADIUS 伺服器的路徑遠端 RADIUS 伺服器群組中。 在 [proxy RADIUS，網路存取伺服器設定為 RADIUS client;並在每個 RADIUS 伺服器、RADIUS proxy 設定為 RADIUS client。


![NPS 連接要求處理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>NPS 您使用的網路存取伺服器可能閘道相容 RADIUS 通訊協定，例如 802.1 X wireless 存取點與驗證的參數，設定為 VPN 執行遠端存取伺服器或撥號伺服器或其他 RADIUS 相容裝置。

若要在本機時送給遠端 RADIUS 伺服器群組中其他要求處理一些驗證要求 NPS，設定多個連接要求原則。

若要設定指定的 NPS 或 RADIUS 伺服器群組處理驗證要求連接要求原則，查看連接要求原則。

若要指定 NPS 或驗證要求轉送其他 RADIUS 伺服器，查看遠端 RADIUS 伺服器群組。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS RADIUS 伺服器連接要求處理為

當您使用 NPS RADIUS 伺服器時，RADIUS 訊息將提供驗證、授權及計量網路存取連接的方式如下：

1. 撥號網路存取伺服器、VPN 伺服器，以及 wireless 存取點，例如存取伺服器，從存取收到連接要求。 

2. 存取伺服器] 設定為使用 RADIUS 驗證、授權及計量通訊協定，以建立存取要求訊息，並將其傳送到 NPS 伺服器。 

3. NPS 伺服器評估存取要求訊息。 

4. 如果需要的話，NPS 伺服器傳送存取查問訊息存取伺服器。 存取伺服器處理挑戰，並更新的存取要求傳送給 NPS 伺服器。 

5. 檢查使用者的認證，取得使用 [安全連接到網域控制站的使用者 account 撥號屬性。 

6. 連接嘗試授權的使用者 account 以及網路原則這兩個撥號屬性。 

7. 如果連接嘗試同時驗證，授權 NPS 伺服器傳送存取接受訊息存取伺服器。 如果連接嘗試無法通過驗證，或未取得授權，NPS 伺服器傳送存取-退回訊息存取伺服器。 

8. 存取伺服器存取 client 連接處理程序完成時，並傳送計量要求訊息 NPS 伺服器，位置登訊息。 

9. NPS 伺服器傳送計量回應存取伺服器。 

>[!NOTE]
>存取伺服器也會傳送計量要求訊息連接、存取 client 連接時關閉，以及當存取伺服器會開始和停止的時間。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS RADIUS proxy 連接要求處理為

NPS RADIUS client 之間 RADIUS 伺服器 RADIUS proxy 為使用時，RADIUS 訊息的網路存取的連接嘗試轉送方式如下：

1. 撥號網路存取伺服器、virtual 私人網路 (VPN) 伺服器、和 wireless 存取點的存取伺服器接收連接要求從存取。

2. 存取伺服器] 設定為使用 RADIUS 驗證、授權及計量通訊協定，以建立存取要求訊息，並將其傳送到 NPS 作為 NPS RADIUS proxy 伺服器。

3. NPS RADIUS proxy 收到存取要求訊息，根據本機設定的連接要求原則，判斷轉寄存取要求訊息中的位置。

4. NPS RADIUS proxy 轉送適當 RADIUS 伺服器存取要求訊息。

5. RADIUS 伺服器評估存取要求訊息。

6. 如果需要的話，RADIUS 伺服器傳送存取查問訊息給 NPS RADIUS proxy，就會轉送存取伺服器。 存取伺服器處理存取 client 的挑戰，並將更新的存取要求傳送給 NPS RADIUS proxy，就會轉送 RADIUS 伺服器。

7. RADIUS 伺服器驗證，並授權連接嘗試。

8. 如果連接嘗試同時驗證，授權 RADIUS 伺服器傳送存取接受訊息給 NPS RADIUS proxy，就會轉送存取伺服器。 如果連接嘗試無法通過驗證，或未取得授權，RADIUS 伺服器傳送存取-退回訊息給 NPS RADIUS proxy，就會轉送存取伺服器。

9. 存取伺服器存取 client 連接處理程序完成時，傳送給 NPS RADIUS proxy 計量要求訊息。 NPS RADIUS proxy 登計量資料，並將轉寄 RADIUS 伺服器的訊息。

10. RADIUS 伺服器傳送計量回應 NPS RADIUS proxy，就會轉送存取伺服器。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
