---
title: 管理 NPS 範本
description: 本主題提供有關如何在 Windows Server 2016 中建立、套用、匯出及匯入網路原則伺服器 NPS 範本的指示。
manager: brianlic
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7380f5a712294b1583a7d552ef695811f8cebef4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952114"
---
# <a name="manage-nps-templates"></a>管理 NPS 範本

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用網路原則伺服器 \( NPS \) 範本來建立設定專案（例如遠端驗證撥入使用者服務 \( RADIUS \) 用戶端或共用密碼），您可以在本機 NPS 上重複使用這些設定元素，並匯出以用於其他 nps。

範本管理在 NPS 主控台中提供一個節點，您可以在其中建立、修改、刪除、複製及查看 NPS 範本的使用。 NPS 範本的設計目的是要減少在一或多部伺服器上設定 NPS 所需的時間和成本。

下列 NPS 範本類型可用於 [範本管理] 中的設定。

- **共用密碼**。 此範本類型可讓您指定共用密碼，以便在設定 RADIUS 用戶端和伺服器時，于 NPS 主控台) 的適當位置中選取範本，以重複使用 (。

- **RADIUS 用戶端**。 此範本類型可讓您在 NPS 主控台的適當位置中選取範本，以設定可重複使用的 RADIUS 用戶端設定。

- **遠端 RADIUS 伺服器**。 此範本可讓您在 NPS 主控台的適當位置中選取範本，以設定可重複使用的遠端 RADIUS 伺服器設定。

- **IP 篩選器**。 此範本可讓您建立網際網路通訊協定第4版 (IPv4) 和網際網路通訊協定第6版 \( IPv6 \) 篩選器，您可以在 \( \) 設定網路原則時，于 NPS 主控台的適當位置中選取範本，以重複使用此範例。

## <a name="create-an-nps-template"></a>建立 NPS 範本

設定範本與直接設定 NPS 不同。 建立範本並不會影響 NPS 的功能。 只有當您在 NPS 主控台的適當位置中選取範本，並套用範本來影響 NPS 功能時，才會套用此範本。

例如，如果您在 NPS 主控台的 [ **Radius 用戶端和伺服器**] 底下設定 radius 用戶端，您會改變 nps 設定，並在設定 nps 以與您的其中一個網路存取伺服器通訊時採取一個步驟。 \(下一步是設定網路存取伺服器 NAS 與 \( \) NPS 通訊。\)

不過，如果您在 NPS 主控台的 [**範本管理**] 底下設定新的**radius 用戶端**範本，而不是在 [ **RADIUS 用戶端和伺服器**] 底下建立新的 radius 用戶端，則您已建立範本，但尚未變更 NPS 功能。 若要改變 NPS 功能，您必須從 NPS 主控台的正確位置套用範本。

下列程式提供如何建立新範本的指示。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

### <a name="to-create-an-nps-template"></a>若要建立 NPS 範本


1. 在 NPS 的伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，展開 [**範本管理**]，以滑鼠右鍵按一下範本類型，例如 [ **RADIUS 用戶端**]，然後按一下 [**新增**]。

3. 此時會開啟新的範本 [屬性] 對話方塊，供您用來設定範本。

## <a name="apply-an-nps-template"></a>套用 NPS 範本

您可以使用您在 [**範本管理**] 中建立的範本，方法是流覽至 NPS 主控台中可套用範本的位置。 例如，如果您想要將共用密碼範本套用到 RADIUS 用戶端設定，您可以使用下列程式。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

### <a name="to-apply-an-nps-template"></a>套用 NPS 範本

1. 在 NPS 的伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，展開 [ **Radius 用戶端和伺服器**]，然後展開 [ **radius 用戶端**]。

3.In **RADIUS 用戶端**，在詳細資料窗格中，以滑鼠右鍵按一下您要套用 NPS 範本的 RADIUS 用戶端，然後按一下 [**屬性**]。

4. 在 RADIUS 用戶端的 [內容] 對話方塊的 [**選取現有的共用密碼] 範本**中，從範本清單中選取您想要套用的範本。

## <a name="export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

您可以匯出要在其他 Nps 上使用的範本，也可以將範本匯入範本**管理**以用於本機電腦。

若要完成此程式，至少需要**Administrators**的成員資格或同等許可權。

### <a name="to-export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

1. 若要匯出 NPS 範本，請在 NPS 主控台中，以滑鼠右鍵按一下 [**範本管理**]，然後按一下 [將**範本匯出到**檔案]。

2. 若要匯入 NPS 範本，請在 NPS 主控台中，以滑鼠右鍵按一下 [**範本管理**]，然後按一下 [**從電腦匯入範本**] 或 [從檔案匯**入範本**]。


