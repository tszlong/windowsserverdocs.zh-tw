---
title: 連線要求處理
description: 本主題概要說明 Windows Server 2016 中的網路原則伺服器連線要求處理。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 268a307336d9a4fae1be5621eec7c32a06a6204e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405399"
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

如果您想要讓執行網路原則伺服器（NPS）的本機伺服器針對連線要求進行驗證，可以使用預設連線要求原則，而不需要其他設定。 根據預設原則，NPS 會驗證在本機網域和受信任網域中具有帳戶的使用者和電腦。

如果您想要將連線要求轉送到遠端 NPS 或其他 RADIUS 伺服器，請建立遠端 RADIUS 伺服器群組，然後設定連線要求原則，將要求轉送到該遠端 RADIUS 伺服器群組。 使用此設定時，NPS 可以將驗證要求轉寄到任何 RADIUS 伺服器，而且具有不受信任網域中帳戶的使用者可以進行驗證。

下圖顯示從網路存取伺服器到 RADIUS proxy 的存取要求訊息的路徑，再到遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 在 RADIUS proxy 上，網路存取伺服器會設定為 RADIUS 用戶端;而在每個 RADIUS 伺服器上，RADIUS proxy 會設定為 RADIUS 用戶端。


![NPS 連接要求處理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>您搭配 NPS 使用的網路存取伺服器可以是與 RADIUS 通訊協定相容的閘道裝置，例如 802.1 X 無線存取點和驗證交換器、執行設定為 VPN 或撥號伺服器之遠端存取的伺服器，或其他與 RADIUS 相容的裝置。

如果您想要 NPS 在本機處理某些驗證要求，同時將其他要求轉送到遠端 RADIUS 伺服器群組，請設定一個以上的連線要求原則。

若要設定連線要求原則來指定要處理驗證要求的 NPS 或 RADIUS 伺服器群組，請參閱連線要求原則。

若要指定要轉送驗證要求的 NPS 或其他 RADIUS 伺服器，請參閱遠端 RADIUS 伺服器群組。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS 做為 RADIUS 伺服器連接要求處理

當您使用 NPS 做為 RADIUS 伺服器時，RADIUS 訊息會以下列方式提供網路存取連線的驗證、授權和帳戶管理：

1. 存取伺服器，例如撥號網路存取伺服器、VPN 伺服器以及無線存取點，都會從存取用戶端接收連線要求。 

2. 設定為使用 RADIUS 做為驗證、授權和帳戶處理通訊協定的存取伺服器，會建立存取要求訊息，並將它傳送到 NPS。 

3. NPS 會評估要求存取訊息。 

4. 如有需要，NPS 會將「存取挑戰」訊息傳送至存取伺服器。 存取伺服器會處理挑戰，並將更新的存取要求傳送到 NPS。 

5. 系統會檢查使用者認證，並使用網域控制站的安全連線來取得使用者帳戶的撥入屬性。 

6. 連線嘗試會使用使用者帳戶的撥入內容和網路原則進行授權。 

7. 如果連線嘗試已通過驗證和授權，NPS 就會傳送接受存取訊息給存取伺服器。 如果連線嘗試未通過驗證或未獲授權，NPS 會將拒絕存取訊息傳送至存取伺服器。 

8. 存取伺服器會完成與存取用戶端的連線程式，並將帳戶處理要求訊息傳送至 NPS，其中會記錄訊息。 

9. NPS 會將帳戶處理回應傳送到存取伺服器。 

>[!NOTE]
>存取伺服器也會在建立連線的時間、存取用戶端連線關閉時，以及啟動和停止存取伺服器時，傳送帳戶處理要求訊息。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS 做為 RADIUS proxy 連線要求處理

當 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS proxy 使用時，網路存取連線嘗試的 RADIUS 訊息會以下列方式轉送：

1. 存取伺服器，例如撥號網路存取伺服器、虛擬私人網路（VPN）伺服器以及無線存取點，都會從存取用戶端接收連線要求。

2. 設定為使用 RADIUS 做為驗證、授權和帳戶處理通訊協定的存取伺服器，會建立存取要求訊息，並將它傳送至做為 NPS RADIUS proxy 使用的 NPS。

3. NPS RADIUS proxy 會接收存取要求訊息，而且根據本機設定的連線要求原則，會決定要轉送存取要求訊息的位置。

4. NPS RADIUS proxy 會將存取要求訊息轉送至適當的 RADIUS 伺服器。

5. RADIUS 伺服器會評估要求存取訊息。

6. 如有需要，RADIUS 伺服器會將「存取挑戰」訊息傳送到 NPS RADIUS proxy，以轉送到存取伺服器。 存取伺服器會使用存取用戶端來處理挑戰，並將更新的存取要求傳送到 NPS RADIUS proxy，並將其轉送到 RADIUS 伺服器。

7. RADIUS 伺服器會驗證並授權連接嘗試。

8. 如果連線嘗試已通過驗證並獲得授權，則 RADIUS 伺服器會傳送接受存取訊息到 NPS RADIUS proxy，以轉送到存取伺服器。 如果連線嘗試未通過驗證或未獲授權，RADIUS 伺服器就會將拒絕存取訊息傳送到 NPS RADIUS proxy，以轉送到存取伺服器。

9. 存取伺服器會完成與存取用戶端的連線程式，並將帳戶處理要求訊息傳送至 NPS RADIUS proxy。 NPS RADIUS proxy 會記錄帳戶處理資料，並將訊息轉送到 RADIUS 伺服器。

10. RADIUS 伺服器會將會計回應傳送到 NPS RADIUS proxy，以轉送到存取伺服器。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
