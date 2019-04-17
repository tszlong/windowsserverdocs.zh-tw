---
title: 遠端 RADIUS 伺服器群組
description: 本主題提供的網路原則伺服器遠端 RADIUS 伺服器群組 Windows Server 2016 中的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>遠端 RADIUS 伺服器群組

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當您設定的遠端驗證 Dial 使用者服務 (RADIUS) proxy 的網路原則 Server (NPS) 時，您可以使用 NPS RADIUS 伺服器正處理連接要求，因為它們可以在 account 使用者或電腦位於網域中執行驗證和授權的請求連接轉送給。 例如，如果您想要轉送連接要求受信任的網域中的一或多個 RADIUS 伺服器，您可以設定 NPS RADIUS proxy 轉送未受信任的網域中的遠端 RADIUS 伺服器要求為。

>[!NOTE]
>遠端 RADIUS 伺服器群組是無關和分開 Windows 群組。

若要設定 NPS RADIUS proxy 為，您必須建立連接要求原則，其中包含所有的 NPS 評估轉送給的簡訊，以及將訊息傳送所需的資訊。

當您 NPS RADIUS 伺服器群組遠端設定，您可以設定連接要求原則群組您指定轉送連接要求 NPS 的位置。

## <a name="configuring-radius-servers-for-a-group"></a>設定適用於群組 RADIUS 伺服器

遠端 RADIUS 伺服器群組是一個包含一或多個 RADIUS 伺服器命名的群組。 如果您設定一個以上的伺服器，您可以指定負載平衡設定，以確定的訂單伺服器由 proxy，或是 RADIUS 訊息的流量分配避免載太多連接要求伺服器一或多個群組中的所有伺服器。

每個群組中的伺服器具有下列設定。

- **名稱或地址**。 每個群組成員必須群組中的唯一名稱。 名稱可能 IP 位址，或是的名稱解析為 IP 位址。

- **驗證及計量**。 您可以向前驗證要求、計量要求，或兩者設定為每個遠端 RADIUS 伺服器群組成員。

- **負載平衡**。 優先順序設定用來表示群組成員主要伺服器（的優先順序設定為 [1）。 具有相同的優先順序群組成員，減重設定用來頻率計算每個伺服器傳送 RADIUS 訊息。 您可以使用額外的設定來設定 NPS 伺服器偵測到群組成員第一次不使用時，可供使用時已經判斷無法後的方式。

設定遠端 RADIUS 伺服器群組之後，您可以指定群組中驗證計量連接要求原則設定。 因此，您可以設定遠端 RADIUS 伺服器群組第一次。 接下來，您可以設定要使用的新設定遠端 RADIUS 伺服器群組連接要求原則。 或者，您可以使用新的連接要求原則精靈建立連接要求原則時，建立新的遠端 RADIUS 伺服器群組。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
