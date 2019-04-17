---
title: NPS 範本
description: 本主題提供的網路原則伺服器範本，Windows Server 2016 中的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>NPS 範本

>適用於：Windows Server（以每年次管道）、Windows Server 2016

網路原則伺服器 \(NPS\) 範本可讓您建立設定項目，例如撥號使用者服務遠端驗證 \(RADIUS\) 戶端或共用的密碼，您可以在本機伺服器 NPS 及匯出其他 NPS 伺服器上使用重複使用。

NPS 範本專為減少的時間和成本所需設定 NPS 一或多個的伺服器上。 適用於管理範本 \] 中的設定下列 NPS 範本類型︰

- 共用的密碼
- RADIUS 戶端
- 遠端 RADIUS 伺服器
- IP 篩選
- 補救伺服器群組

設定範本與不同直接設定 NPS 伺服器。 建立範本不會影響 NPS 伺服器的功能。 它是只有當您選取範本中 NPS 主控台範本影響 NPS 功能伺服器的適當位置。 

例如，如果您可以設定 RADIUS client NPS RADIUS 戶端及伺服器主控台中，變更 NPS 伺服器設定也可以拍攝設定 NPS 與您的網路存取伺服器 \(NAS's\) 其中一個步驟。 \ (下一步就是設定通訊具有 NPS。NAS \) 不過，如果您在 NPS 主機中設定新 RADIUS 戶端範本**管理範本**而非建立新的 RADIUS client 在**RADIUS 戶端與伺服器**、建立範本，但您無法更動 NPS 伺服器功能尚未。 若要變更的 NPS server 功能，您必須選取範本 NPS 主控台位置正確。

## <a name="creating-templates"></a>建立範本

若要建立範本，開放 NPS 主機上按一下滑鼠右鍵範本類型，例如**IP 篩選器**，然後按一下 [**新增]**。 新範本屬性對話方塊，可讓您設定您的範本。

## <a name="using-templates-locally"></a>在本機使用範本

您可以使用您所建立的範本**管理範本**藉由瀏覽至範本可在套用 NPS 主機的位置。 例如您建立新分享機密範本，如果您想要 RADIUS client 設定，適用於在**RADIUS 戶端與伺服器**和**RADIUS 戶端**，開放 RADIUS client 屬性。 在**選取現有的共用機密範本**，選取您先前建立的清單中可用的範本範本。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
