---
title: 停用在 NPS 中的 NAS 通知轉寄
description: 本主題提供有關 Windows Server 2016 中設定網路原則伺服器的同時驗證的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882259"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>停用在 NPS 中的 NAS 通知轉寄

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來停用轉送的開始和停止從網路存取伺服器 (Nas) 的訊息，在 NPS 中設定的遠端 RADIUS 伺服器群組的成員。

當您有設定的遠端 RADIUS 伺服器群組，並且在 NPS**連線要求原則**，清除**帳戶處理要求轉送給這個遠端 RADIUS 伺服器群組**核取方塊，這些群組分別是仍已傳送的 NAS 啟動和停止通知訊息。 

這會建立不必要的網路流量。 若要排除這個流量，停用的每個遠端 RADIUS 伺服器群組中的個別伺服器轉送的 NAS 通知。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-disable-nas-notification-forwarding"></a>若要停用 NAS 通知轉寄

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，按兩下**RADIUS 用戶端和伺服器**，按一下**遠端 RADIUS 伺服器群組**，然後按兩下您想要設定遠端 RADIUS 伺服器群組。 遠端 RADIUS 伺服器群組**屬性**對話方塊隨即開啟。

3. 按兩下您想要設定，然後按一下 [群組成員**驗證/帳戶處理**] 索引標籤。

4. 在  **Accounting**，清除**轉送網路存取伺服器啟動和停止通知到這個伺服器**核取方塊，然後按一下**確定**。

5. 針對您想要設定的所有群組成員重複步驟 3 和 4。

如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
