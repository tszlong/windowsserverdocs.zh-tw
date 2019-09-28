---
title: 遠端 RADIUS 伺服器群組
description: 本主題提供 Windows Server 2016 中網路原則伺服器遠端 RADIUS 伺服器群組的總覽。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63a6eb5f0f78ed8dbcc0144602f16274fd6ec213
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396319"
---
# <a name="remote-radius-server-groups"></a>遠端 RADIUS 伺服器群組

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您將網路原則伺服器（NPS）設定為遠端驗證撥入使用者服務（RADIUS） proxy 時，您可以使用 NPS 將連線要求轉送到能夠處理連線要求的 RADIUS 伺服器，因為它們可以執行使用者或電腦帳戶所在網域中的驗證和授權。 例如，如果您想要將連線要求轉送到不受信任網域中的一或多個 RADIUS 伺服器，您可以將 NPS 設定為 RADIUS proxy，將要求轉送到不受信任網域中的遠端 RADIUS 伺服器。

>[!NOTE]
>遠端 RADIUS 伺服器群組與 Windows 群組無關，而且與不同。

若要將 NPS 設定為 RADIUS proxy，您必須建立連線要求原則，其中包含 NPS 評估要轉寄哪些訊息以及要將訊息傳送到何處的所有必要資訊。

當您在 NPS 中設定遠端 RADIUS 伺服器群組，並使用群組設定連線要求原則時，您會指定 NPS 要轉送連接要求的位置。

## <a name="configuring-radius-servers-for-a-group"></a>為群組設定 RADIUS 伺服器

遠端 RADIUS 伺服器群組是一個名為的群組，其中包含一或多個 RADIUS 伺服器。 如果您設定多部伺服器，您可以指定負載平衡設定，以判斷 proxy 使用伺服器的順序，或將 RADIUS 訊息的流程分散到群組中的所有伺服器，以避免多載一部或多部伺服器連接要求過多。

群組中的每部伺服器都具有下列設定。

- **名稱或位址**。 每個群組成員在群組內都必須有唯一的名稱。 名稱可以是 IP 位址或可解析為其 IP 位址的名稱。

- **驗證和帳戶**處理。 您可以將驗證要求、帳戶處理要求或兩者轉送到每個遠端 RADIUS 伺服器群組成員。

- **負載平衡**。 優先順序設定是用來指出群組的哪個成員是主伺服器（優先順序設定為1）。 對於具有相同優先順序的群組成員，權數設定會用來計算 RADIUS 訊息傳送至每部伺服器的頻率。 您可以使用其他設定來設定當群組成員第一次變成無法使用時，以及在判斷為無法使用之後何時可使用時，NPS 會偵測到的方式。

設定遠端 RADIUS 伺服器群組之後，您可以在連線要求原則的驗證和帳戶處理設定中指定群組。 因此，您可以先設定遠端 RADIUS 伺服器群組。 接下來，您可以設定連線要求原則，以使用新設定的遠端 RADIUS 伺服器群組。 或者，您也可以在建立連線要求原則時，使用 [新增連線要求原則] [建立新的遠端 RADIUS 伺服器群組]。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
