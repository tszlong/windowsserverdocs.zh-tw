---
title: NPS 範本
description: 本主題提供 Windows Server 2016 中網路原則伺服器範本的總覽。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bafff7a6a15312ab1bdca2e7b98307bc94731d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405316"
---
# <a name="nps-templates"></a>NPS 範本

>適用於：Windows Server (半年通道)、Windows Server 2016

網路原則伺服器 \(NPS\) 範本可讓您建立設定元素，例如遠端驗證撥入使用者服務 \(RADIUS\) 用戶端或共用密碼，您可以在本機 NPS 上重複使用並匯出以用於其他 Nps。

NPS 範本的設計目的是要減少在一或多部伺服器上設定 NPS 所需的時間和成本。 下列 NPS 範本類型可用於 [範本管理] 中的設定：

- 共用密碼
- RADIUS 用戶端
- 遠端 RADIUS 伺服器
- IP 篩選器
- 補救伺服器群組

設定範本與直接設定 NPS 不同。 建立範本並不會影響 NPS 的功能。 只有當您在 NPS 主控台中的適當位置選取範本時，範本才會影響 NPS 功能。 

例如，如果您在 NPS 主控台的 [RADIUS 用戶端和伺服器] 底下設定 RADIUS 用戶端，則您已改變 NPS 設定，並在設定 NPS 與其中一項網路存取伺服器通訊時，\(NAS 的\)。 \(下一個步驟是設定 NAS 與 NPS 通訊。\) 不過，如果您在 NPS 主控台的 [**範本管理**] 底下設定新的 Radius 用戶端範本，而不是在 [ **RADIUS 用戶端和伺服器**] 底下建立新的 radius 用戶端，則您已建立範本，但尚未變更 NPS 功能。 若要改變 NPS 功能，您必須從 NPS 主控台中正確的位置選取範本。

## <a name="creating-templates"></a>建立範本

若要建立範本，請開啟 NPS 主控台，以滑鼠右鍵按一下範本類型，例如 [ **IP 篩選器**]，然後按一下 [**新增**]。 此時會開啟新的範本 [屬性] 對話方塊，讓您設定範本。

## <a name="using-templates-locally"></a>在本機使用範本

您可以使用您在 [**範本管理**] 中建立的範本，方法是流覽至 NPS 主控台中可套用範本的位置。 例如，如果您建立新的共用秘密範本，並將其套用至 RADIUS 用戶端設定，請在 [ **radius 用戶端和伺服器**和**radius 用戶端**] 中開啟 radius 用戶端屬性。 在 [**選取現有的共用密碼] 範本**中，從可用的範本清單中選取您先前建立的範本。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
