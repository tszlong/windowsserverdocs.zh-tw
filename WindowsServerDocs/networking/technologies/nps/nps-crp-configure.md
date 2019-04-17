---
title: 設定連接要求原則
description: 本主題提供如何在 Windows Server 2016 的網路原則伺服器連接要求原則設定的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>設定連接要求原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題來建立和設定指定是否本機伺服器 NPS 處理連接要求或轉寄給處理遠端 RADIUS 伺服器連接要求原則。

連接要求原則是設定的條件和設定，可讓網路系統管理員，若要指定哪些遠端驗證 Dial 使用者服務 (RADIUS) 伺服器執行驗證和執行的網路原則伺服器 \(NPS\) 伺服器接收從 RADIUS 連接要求的授權。

預設連接要求原則使用 NPS RADIUS 伺服器，並處理所有驗證要求本機。

若要設定伺服器執行 NPS 做為 RADIUS proxy 和往後連接要求的其他 NPS 或 RADIUS 伺服器，您必須設定遠端 RADIUS 伺服器群組除了新增指定條件和設定連接要求必須符合的新連接要求原則。

建立新的連接要求原則精靈的新連接要求原則時，您可以建立新的遠端 RADIUS 伺服器群組。

如果您不想做為 RADIUS 伺服器及處理程序連接本機要求 NPS 伺服器，您可以 delete 預設連接要求原則。

如果您想做為 RADIUS 伺服器 NPS 伺服器，處理連接要求本機，以及 RADIUS proxy，送給遠端 RADIUS 伺服器群組，某些連接要求新增新的原則，使用下列程序，然後確認 [預設連接要求原則是最後處理放置原則的清單中的最後一次的原則。

## <a name="add-a-connection-request-policy"></a>新增連接要求原則

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-add-a-new-connection-request-policy"></a>若要新增新的連接要求原則 

1. 在伺服器管理員中，按一下**工具**，然後按一下 [**的網路原則伺服器**打開 NPS 主機。 
2. 在 [主控台按兩下 [**原則**。
3. 以滑鼠右鍵按一下**連接要求原則**，然後按**新增連接要求原則**。
4. 使用新設定連接到連接要求原則精靈要求原則，如果之前未設定、遠端 RADIUS 伺服器群組。


如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。

