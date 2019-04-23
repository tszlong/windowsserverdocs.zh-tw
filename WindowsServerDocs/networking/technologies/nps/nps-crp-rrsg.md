---
title: 遠端 RADIUS 伺服器群組
description: 本主題提供網路原則伺服器遠端 RADIUS 伺服器群組中 Windows Server 2016 的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855259"
---
# <a name="remote-radius-server-groups"></a>遠端 RADIUS 伺服器群組

>適用於：Windows Server （半年通道），Windows Server 2016

當您設定網路原則伺服器 (NPS) 做為遠端驗證撥號使用者服務 (RADIUS) proxy 時，您會使用 NPS 將連線要求轉送到 RADIUS 伺服器，可處理連線要求，因為它們可以執行驗證和授權的使用者或電腦帳戶所在的網域。 例如，如果您想要連線要求轉送到不受信任網域中的一或多個 RADIUS 伺服器，您可以設定 NPS 做為 RADIUS proxy，將要求轉送到遠端 RADIUS 伺服器不信任網域中。

>[!NOTE]
>遠端 RADIUS 伺服器群組是無關，且獨立的 Windows 群組。

若要設定 NPS 做為 RADIUS proxy，您必須建立一個連線要求原則，其中包含所有 NPS 評估哪些訊息要轉送和傳送訊息的位置所需的資訊。

當您在 NPS 中設定遠端 RADIUS 伺服器群組，您可以設定連線要求原則與群組，您會指定 NPS 轉送連線要求所在的位置。

## <a name="configuring-radius-servers-for-a-group"></a>設定 RADIUS 伺服器群組

遠端 RADIUS 伺服器群組是具名的群組，其中包含一或多個 RADIUS 伺服器。 如果您設定多部伺服器，您可以指定負載平衡設定，以確定 proxy 所使用之伺服器的順序，或將 RADIUS 訊息的流量分散到以防止多載化一或多個伺服器群組中所有伺服器使用太多的連線要求。

每個群組中的伺服器具有下列設定。

- **名稱或位址**。 每個群組成員必須在群組內的唯一名稱。 名稱可以是 IP 位址或可解析成它的 IP 位址的名稱。

- **驗證和帳戶處理**。 您可以將轉送驗證要求、 帳戶處理要求，或兩者設定為每個遠端 RADIUS 伺服器群組成員。

- **負載平衡**。 優先順序設定用來指出哪些群組的成員是主要伺服器 （優先順序設定為 1）。 對於具有相同優先順序的群組成員，權數設定用來計算頻率 RADIUS 訊息傳送至每一部伺服器。 若要設定 NPS 偵測以及可供使用之後已由判定為無法使用時第一次群組成員變成無法使用的方式，您可以使用其他設定。

設定遠端 RADIUS 伺服器群組之後，您可以在驗證與帳戶處理設定的連線要求原則中指定的群組。 基於這個原因，您可以先設定遠端 RADIUS 伺服器群組。 接下來，您可以設定連線要求原則，以使用新設定的遠端 RADIUS 伺服器群組。 或者，您可以使用新的連線要求原則精靈建立連線要求原則時，建立新的遠端 RADIUS 伺服器群組。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
