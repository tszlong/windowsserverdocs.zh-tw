---
title: NPS 範本
description: 本主題提供 Windows Server 2016 中的網路原則伺服器範本的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823309"
---
# <a name="nps-templates"></a>NPS 範本

>適用於：Windows Server （半年通道），Windows Server 2016

網路原則伺服器\(NPS\)範本可讓您建立組態項目，例如遠端驗證撥入使用者服務\(RADIUS\)用戶端或可重複使用在本機的共用的密碼NPS 與用於其他 NPSs 的匯出。

NPS 範本的設計目的是減少的時間與一或多個伺服器上設定 NPS 所花費的成本。 下列的 NPS 範本類型可供範本管理 中的組態：

- 共用祕密
- RADIUS 用戶端
- 遠端 RADIUS 伺服器
- IP 篩選器
- 補救伺服器群組

設定範本是不同於直接設定 NPS。 建立範本並不會影響 NPS 的功能。 它是只有當您選取的範本中，範本會影響 NPS 功能的 NPS 主控台中的適當位置。 

例如，如果您在 NPS 主控台的 RADIUS 用戶端和伺服器設定為 RADIUS 用戶端，您變更 NPS 設定並採取設定 NPS 與其中一個網路存取伺服器進行通訊的其中一個步驟\(NAS 的\). \(下一個步驟是設定在 NAS 與 NPS 進行通訊。\)不過，如果您在 NPS 主控台的 設定新的 RADIUS 用戶端範本**範本管理**而不會建立新的 RADIUS 用戶端下**RADIUS 用戶端和伺服器**，您已建立範本，但您並未改變 NPS 功能尚未。 若要改變的 NPS 功能，您必須從正確的位置，在 NPS 主控台中選取範本。

## <a name="creating-templates"></a>建立範本

若要建立範本，請開啟 NPS 主控台，例如以滑鼠右鍵按一下範本類型， **IP 篩選器**，然後按一下**新增**。 此時會開啟新的範本內容對話方塊，可讓您設定您的範本。

## <a name="using-templates-locally"></a>在本機使用範本

您可以使用您在建立範本**範本管理**瀏覽至在 NPS 主控台中可以套用範本的位置。 比方說，如果您建立新的共用祕密範本，您想要在 RADIUS 用戶端設定，適用於**RADIUS 用戶端和伺服器**並**RADIUS 用戶端**，開啟 RADIUS 用戶端內容。 在 **選取現有的共用祕密範本**，選取您先前建立的可用範本清單中的範本。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
