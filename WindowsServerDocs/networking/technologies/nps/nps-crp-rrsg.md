---
title: 遠端 RADIUS 伺服器群組
description: 本主題概要說明 Windows Server 2016 中的網路原則伺服器遠端 RADIUS 伺服器群組。
manager: brianlic
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b41697afe6d4f4e0c80dce8429bd7a4494c89284
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949004"
---
# <a name="remote-radius-server-groups"></a>遠端 RADIUS 伺服器群組

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您設定網路原則伺服器 (NPS) 作為遠端驗證撥入消費者服務 (RADIUS) proxy 時，您可以使用 NPS 將連線要求轉送到能夠處理連線要求的 RADIUS 伺服器，因為它們可以在使用者或電腦帳戶所在的網域中執行驗證和授權。 例如，如果您要將連線要求轉送到不受信任網域中的一或多個 RADIUS 伺服器，可以設定 NPS 做為 RADIUS Proxy，將要求轉送到不受信任網域中的遠端 RADIUS 伺服器。

>[!NOTE]
>遠端 RADIUS 伺服器群組與 Windows 群組無關，而且不同。

若要將 NPS 設定為 RADIUS Proxy，您必須建立連線要求原則，其中包含 NPS 評估哪些訊息要轉送和傳送訊息到何處所需的所有資訊。

在設定 NPS 中的遠端 RADIUS 伺服器群組以及設定群組的連線要求原則時，要指定 NPS 轉送連線要求的目的位置。

## <a name="configuring-radius-servers-for-a-group"></a>設定群組的 RADIUS 伺服器

遠端 RADIUS 伺服器群組是具名群組，其中包含一或多部 RADIUS 伺服器。 如果您設定多部伺服器，可以指定負載平衡設定，以確定 Proxy 使用伺服器的順序，或將 RADIUS 訊息的流量分配到群組中的所有伺服器，以免太多連線要求使一或多部伺服器負載過重。

群組中的每部伺服器都有下列設定。

- **名稱或位址**。 每個群組成員在群組中都必須有唯一的名稱。 此名稱可以是 IP 位址，或是可解析為其 IP 位址的名稱。

- **驗證和帳戶** 處理。 您可以將驗證要求、帳戶處理要求或兩者轉送到每個遠端 RADIUS 伺服器群組成員。

- **負載平衡**。 優先順序設定是用來指出群組成員的主要伺服器 (優先順序設定為 1)。 對於擁有相同優先順序的群組成員，權數設定可用來計算 RADIUS 訊息傳送給每部伺服器的頻率。 您可以使用其他設定來設定當群組成員初次變成無法使用，以及在判斷為無法使用之後變成可用時，NPS 偵測的方式。

設定遠端 RADIUS 伺服器群組之後，您可以在連線要求原則的驗證和帳戶處理設定中指定群組。 因此，您可以先設定遠端 RADIUS 伺服器群組。 接著，您可以設定連線要求原則使用新設定的遠端 RADIUS 伺服器群組。 或者，您可以使用 [新增連線要求原則精靈]，在建立連線要求原則時，建立新的遠端 RADIUS 伺服器群組。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。
