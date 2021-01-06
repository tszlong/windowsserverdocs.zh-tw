---
title: NPS 範本
description: 本主題概要說明 Windows Server 2016 中的網路原則伺服器範本。
manager: brianlic
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 66fb4d0b7d2fc5bd77fb1b943b45a8c75408866e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947444"
---
# <a name="nps-templates"></a>NPS 範本

>適用於：Windows Server (半年度管道)、Windows Server 2016

網路原則伺服器 \( NPS \) 範本可讓您建立設定元素，例如遠端驗證撥入消費者服務 \( RADIUS \) 用戶端或共用秘密，您可以在本機 NPS 上重複使用，並將其匯出以供其他 NPSs 使用。

NPS 範本的設計目的是要減少在一或多部伺服器上設定 NPS 所需的時間和成本。 下列 NPS 範本類型可在範本管理中進行設定：

- 共用秘密
- RADIUS 用戶端
- 遠端 RADIUS 伺服器
- IP 篩選器
- 補救伺服器群組

設定範本與直接設定 NPS 不同。 建立範本並不會影響 NPS 的功能。 只有當您在 NPS 主控台中的適當位置選取範本時，範本會影響 NPS 功能。

例如，如果您在 [RADIUS 用戶端和伺服器] 下的 NPS 主控台中設定 RADIUS 用戶端，您已變更 NPS 設定，並在設定 NPS 與您的其中一個網路存取伺服器的網路存取伺服器通訊時採取了一步 \( \) 。 \(下一步是設定 NAS 與 NPS 通訊。 \) 但是，如果您在 NPS 主控台的 [ **範本管理** ] 下設定新的 Radius 用戶端範本，而不是在 [ **RADIUS 用戶端和伺服器**] 下建立新的 radius 用戶端，則您已建立範本，但尚未改變 NPS 功能。 若要更改 NPS 功能，您必須從 NPS 主控台中正確的位置選取範本。

## <a name="creating-templates"></a>建立範本

若要建立範本，請開啟 NPS 主控台，以滑鼠右鍵按一下範本類型，例如 [ **IP 篩選器**]，然後按一下 [ **新增**]。 [新範本內容] 對話方塊隨即開啟，讓您設定範本。

## <a name="using-templates-locally"></a>在本機使用範本

您可以使用您在 [ **範本管理** ] 中建立的範本，方法是流覽至 NPS 主控台中可套用範本的位置。 例如，如果您建立新的共用秘密範本以套用至 RADIUS 用戶端設定，請在 [Radius 客戶 **端和伺服器** 和 **radius 用戶端**] 中開啟 radius 用戶端屬性。 在 [ **選取現有的共用秘密範本**] 中，從可用範本的清單中選取您先前建立的範本。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。
