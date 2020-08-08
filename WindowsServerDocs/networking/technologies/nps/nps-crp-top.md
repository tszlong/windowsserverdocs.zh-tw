---
title: 連線要求處理
description: 本主題概要說明 Windows Server 2016 中的網路原則伺服器連線要求處理。
manager: brianlic
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9b50fe9a3adce7967a555237446e77e4d4080221
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955595"
---
# <a name="connection-request-processing"></a>連線要求處理

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 Windows Server 2016 中網路原則伺服器的連線要求處理。

>[!NOTE]
>除了本主題之外，還有下列連接要求處理檔可供使用。
> - [連線要求原則](nps-crp-crpolicies.md)
> - [領域名稱](nps-crp-realm-names.md)
> - [遠端 RADIUS 伺服器群組](nps-crp-rrsg.md)

您可以使用連線要求處理來指定執行連線要求驗證的位置-在本機電腦上，或在屬於遠端 RADIUS 伺服器群組成員的遠端 RADIUS 伺服器上。

如果您想要 (NPS) 執行網路原則伺服器的本機伺服器，以執行連線要求的驗證，可以使用預設連線要求原則，而不需要其他設定。 根據預設原則，NPS 會驗證擁有本機網域與受信任網域之帳戶的使用者與電腦。

如果您想要將連線要求轉送到遠端 NPS 或其他 RADIUS 伺服器，請建立遠端 RADIUS 伺服器群組，然後設定連線要求原則，將要求轉送到該遠端 RADIUS 伺服器群組。 透過此設定，NPS 便可以將驗證要求轉送到任何 RADIUS 伺服器，而且可以驗證擁有不受信任網域中之帳戶的使用者。

以下圖例顯示 Access-Request 訊息的路徑，路徑是從網路存取伺服器到 RADIUS Proxy，再到遠端 RADIUS 伺服器群組的 RADIUS 伺服器。 在 RADIUS Proxy 上，是將網路存取伺服器設定為 RADIUS 用戶端；而在每個 RADIUS 伺服器上，則是將 RADIUS Proxy 設定為 RADIUS 用戶端。


![NPS 連接要求處理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>您搭配 NPS 使用的網路存取伺服器可以是與 RADIUS 通訊協定相容的閘道裝置，例如 802.1 X 無線存取點和驗證交換器、執行遠端存取的伺服器，設定為 VPN 或撥號伺服器，或其他與 RADIUS 相容的裝置。

如果要讓 NPS 在本機處理某些驗證要求，而將其他要求轉送到遠端 RADIUS 伺服器群組，請設定一個以上的連線要求原則。

若要設定連線要求原則來指定要處理驗證要求的 NPS 或 RADIUS 伺服器群組，請參閱連線要求原則。

若要指定要轉送驗證要求的 NPS 或其他 RADIUS 伺服器，請參閱遠端 RADIUS 伺服器群組。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS 做為 RADIUS 伺服器連接要求處理

當您使用 NPS 做為 RADIUS 伺服器時，RADIUS 訊息會以下列方式提供網路存取連線的驗證、授權和帳戶管理：

1. 存取伺服器，例如撥號網路存取伺服器、VPN 伺服器以及無線存取點，都會從存取用戶端接收連線要求。

2. 設定為使用 RADIUS 做為驗證、授權和帳戶處理通訊協定的存取伺服器，會建立存取要求訊息，並將它傳送到 NPS。

3. NPS 會評估要求存取訊息。

4. 如有需要，NPS 會將「存取挑戰」訊息傳送至存取伺服器。 存取伺服器會處理挑戰，並將更新的存取要求傳送到 NPS。

5. 檢查使用者認證以及取得使用者帳戶的撥入內容時，是透過連到網域控制站的安全連線。

6. 授權連線嘗試時，是同時透過使用者帳戶的撥入內容以及網路原則。

7. 如果連線嘗試已通過驗證和授權，NPS 就會傳送接受存取訊息給存取伺服器。 如果連線嘗試未通過驗證或未獲授權，NPS 會將拒絕存取訊息傳送至存取伺服器。

8. 存取伺服器會完成與存取用戶端的連線程式，並將帳戶處理要求訊息傳送至 NPS，其中會記錄訊息。

9. NPS 會將帳戶處理回應傳送到存取伺服器。

>[!NOTE]
>當存取用戶端連線關閉時，以及當存取伺服器啟動和停止時，存取伺服器也會在建立連線期間傳送 Accounting-Request 訊息。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS 做為 RADIUS proxy 連線要求處理

當 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS Proxy 時，網路存取連線嘗試的 RADIUS 訊息會以下列方式轉寄：

1. 存取伺服器，例如撥號網路存取伺服器、虛擬私人網路 (VPN) 伺服器以及無線存取點，都會從存取用戶端接收連線要求。

2. 設定為使用 RADIUS 做為驗證、授權和帳戶處理通訊協定的存取伺服器，會建立存取要求訊息，並將它傳送至做為 NPS RADIUS proxy 使用的 NPS。

3. NPS RADIUS Proxy 接收 Access-Request 訊息，並根據本機設定的連線要求原則，判斷將 Access-Request 訊息轉寄到何處。

4. NPS RADIUS Proxy 將 Access-Request 訊息轉寄到適當的 RADIUS 伺服器。

5. RADIUS 伺服器評估 Access-Request 訊息。

6. 必要時，RADIUS 伺服器將 Access-Challenge 訊息傳送到 NPS RADIUS Proxy，在此將它轉寄到存取伺服器。 存取伺服器處理與存取用戶端的挑戰，並將更新的 Access-Request 傳送到 NPS RADIUS Proxy，在此將它轉寄到 RADIUS 伺服器。

7. RADIUS 伺服器驗證並授權連線嘗試。

8. 如果同時驗證和授權連線嘗試，RADIUS 伺服器會將 Access-Challenge 訊息傳送到 NPS RADIUS Proxy，在此將它轉寄到存取伺服器。 如果既未驗證也未授權連線嘗試，RADIUS 伺服器會將 Access-Reject 訊息傳送到 NPS RADIUS Proxy，在此將它轉寄到存取伺服器。

9. 存取伺服器完成與存取用戶端的連線程序，並傳送 Accounting-Request 訊息給 NPS RADIUS Proxy。 NPS RADIUS Proxy 記錄帳戶處理資料，並將訊息轉寄到 RADIUS 伺服器。

10. RADIUS 伺服器將 Accounting-Response 傳送到 NPS RADIUS Proxy，在此將它轉寄到存取伺服器。

如需有關 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS) ](nps-top.md)。
