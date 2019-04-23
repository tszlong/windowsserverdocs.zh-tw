---
title: 連線要求處理
description: 本主題提供處理 Windows Server 2016 中的網路原則伺服器連線要求的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829109"
---
# <a name="connection-request-processing"></a>連線要求處理

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解在 Windows Server 2016 中的網路原則伺服器處理連線要求。

>[!NOTE]
>本主題中，除了下列的連線要求處理文件使用。
> - [連線要求原則](nps-crp-crpolicies.md)
> - [領域名稱](nps-crp-realm-names.md)
> - [遠端 RADIUS 伺服器群組](nps-crp-rrsg.md)

您可以使用連線要求處理至指定位置的連線要求執行驗證-在本機電腦上或遠端 RADIUS 伺服器的遠端 RADIUS 伺服器群組的成員。 

如果您想執行網路原則伺服器 (NPS) 執行連線要求驗證的本機伺服器，您可以使用預設連線要求原則，而不需要額外的設定。 根據預設原則，NPS 會驗證使用者，並在本機網域或信任網域中擁有帳戶的電腦。

如果您想要將連線要求轉送到遠端 NPS 或是其他 RADIUS 伺服器，建立一個遠端 RADIUS 伺服器群組，然後設定 一個連線要求原則，將要求轉送到該遠端 RADIUS 伺服器群組。 使用此設定時，NPS 可驗證要求轉送到任何 RADIUS 伺服器，且在不受信任網域中具有帳戶的使用者可以進行驗證。

下圖顯示 Access-request 訊息從網路存取伺服器到 RADIUS proxy，，然後再到 RADIUS 伺服器的路徑中的遠端 RADIUS 伺服器群組。 在 RADIUS proxy，網路存取伺服器設定為 RADIUS 用戶端;且會在每個 RADIUS 伺服器上，做為 RADIUS 用戶端設定 RADIUS proxy。


![NPS 連線要求處理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>您使用 NPS 網路存取伺服器可以是相容於 RADIUS 通訊協定，例如 802.1x 無線存取點與驗證交換機、 撥號伺服器或伺服器執行遠端存取設定為 VPN 閘道裝置或其他 RADIUS 相容的裝置。

如果您想要 NPS 在本機處理某些驗證要求，同時將其他要求轉送到遠端 RADIUS 伺服器群組，設定一個以上的連線要求原則。

若要設定連線要求原則，指定哪些 NPS 或 RADIUS 伺服器群組處理驗證要求，請參閱連線要求原則。

若要指定 NPS 或其他要求都會轉送到哪一個驗證的 RADIUS 伺服器，請參閱遠端 RADIUS 伺服器群組。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS 作為 RADIUS 伺服器連線要求處理

當您使用 NPS 做為 RADIUS 伺服器時，RADIUS 訊息會提供下列方式驗證、 授權和帳戶處理的網路存取連線：

1. 存取伺服器，例如撥號網路存取伺服器、 VPN 伺服器和無線存取點，從存取用戶端接收連線要求。 

2. 設定使用 RADIUS 做為驗證、 授權和帳戶處理通訊協定，存取伺服器，建立 Access-request 訊息，並將它傳送給 NPS。 

3. NPS 會評估 Access-request 訊息。 

4. 如有需要，NPS 會傳送 Access-challenge 訊息給存取伺服器。 存取伺服器會處理挑戰，並將更新的 Access-request 傳送給 NPS。 

5. 檢查使用者認證和使用者帳戶撥入內容取得方式是使用網域控制站的安全連線。 

6. 使用這兩個的撥入內容的使用者帳戶和網路原則授權連線嘗試。 

7. 如果連線嘗試通過驗證並獲授權，NPS 就會傳送接受存取訊息給存取伺服器。 如果連線嘗試未通過驗證，或未獲授權，NPS 就會傳送 Access-reject 訊息給存取伺服器。 

8. 存取伺服器完成與存取用戶端在連線程序，並傳送 Accounting-request 訊息給 NPS 中，將訊息記錄的位置。 

9. NPS 會傳送 Accounting-response 給存取伺服器。 

>[!NOTE]
>存取伺服器也會連接、 當存取用戶端連線已關閉，以及存取伺服器啟動及停止時的時間內傳送 Accounting-request 訊息。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS 做為 RADIUS proxy 連線要求處理

當 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS proxy 時，RADIUS 訊息的網路存取的連線嘗試轉送方式如下：

1. 存取伺服器，例如撥號網路存取伺服器、 虛擬私人網路 (VPN) 伺服器和無線存取點，從存取用戶端接收連線要求。

2. 設定使用 RADIUS 做為驗證、 授權和帳戶處理通訊協定，存取伺服器，建立 Access-request 訊息，並將它傳送至 NPS 做為 NPS RADIUS proxy 使用。

3. NPS RADIUS proxy 接收 Access-request 訊息，在本機設定的連線要求原則，判斷將 Access-request 訊息轉送的位置。

4. NPS RADIUS proxy 會轉送到適當的 RADIUS 伺服器的 Access-request 訊息。

5. RADIUS 伺服器評估 Access-request 訊息。

6. 如有需要，RADIUS 伺服器會傳送 Access-challenge 訊息給 NPS RADIUS proxy，在此將它轉寄到存取伺服器。 存取伺服器會處理存取用戶端的挑戰，並將更新的 Access-request 傳送到 NPS RADIUS proxy，在此將它轉寄到 RADIUS 伺服器。

7. RADIUS 伺服器驗證和授權連線嘗試。

8. 如果通過驗證並授權連線嘗試，RADIUS 伺服器傳送 Access-accept 訊息到 NPS RADIUS proxy，在此將它轉寄到存取伺服器。 如果連線嘗試未通過驗證，或未獲授權，RADIUS 伺服器會傳送 Access-reject 訊息給 NPS RADIUS proxy，在此將它轉寄到存取伺服器。

9. 存取伺服器完成與存取用戶端在連線程序，並傳送 Accounting-request 訊息給 NPS RADIUS proxy。 NPS RADIUS proxy 記錄帳戶處理資料並轉寄訊息到 RADIUS 伺服器。

10. RADIUS 伺服器將 Accounting-response 傳送到 NPS RADIUS proxy，在此將它轉寄到存取伺服器。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
