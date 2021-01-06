---
title: 管理 NPS 範本
description: 本主題提供有關如何針對 Windows Server 2016 中的網路原則伺服器建立、套用、匯出和匯入 NPS 範本的指示。
manager: brianlic
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9e86c545fb467e7e6e381f0be5e0d12e43ce775c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949344"
---
# <a name="manage-nps-templates"></a>管理 NPS 範本

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用網路原則伺服器 \( NPS \) 範本來建立 \( 您可以在本機 NPS 上重複使用的設定元素，例如遠端驗證撥入消費者服務 RADIUS \) 用戶端或共用密碼，以便在其他 NPSs 上使用。

範本管理在 NPS 主控台中提供一個節點，您可以在其中建立、修改、刪除、複製和查看 NPS 範本的使用。 NPS 範本的設計目的是要減少在一或多部伺服器上設定 NPS 所需的時間和成本。

下列 NPS 範本類型可在範本管理中進行設定。

- **共用秘密**。 此範本類型可讓您指定可重複使用的共用密碼 (方法是，在設定 RADIUS 用戶端和伺服器的 NPS 主控台) 的適當位置中選取範本。

- **RADIUS 用戶端**。 此範本類型可讓您設定 RADIUS 用戶端設定，藉由在 NPS 主控台中的適當位置選取範本，即可重複使用這些設定。

- **遠端 RADIUS 伺服器**。 此範本可讓您透過在 NPS 主控台的適當位置中選取範本，來設定您可以重複使用的遠端 RADIUS 伺服器設定。

- **IP 篩選準則**。 此範本可讓您建立第四版網際網路協定 (IPv4) 和網際網路通訊協定第6版 \( IPv6 \) 篩選器，您可以在 \( \) 設定網路原則時，在 NPS 主控台中的適當位置選取範本以重複使用。

## <a name="create-an-nps-template"></a>建立 NPS 範本

設定範本與直接設定 NPS 不同。 建立範本並不會影響 NPS 的功能。 只有當您在 NPS 主控台中的適當位置選取範本，並套用範本影響 NPS 功能時，才會套用此範本。

例如，如果您在 NPS 主控台的 [ **Radius 用戶端和伺服器**] 下設定 radius 用戶端，您可以更改 nps 設定，並採取一個步驟來設定 nps 與您的其中一個網路存取伺服器通訊。 \(下一步是設定 network access server \( NAS \) 與 NPS 通訊。\)

但是，如果您在 NPS 主控台的 [**範本管理**] 下設定新的 **radius 用戶端** 範本，而不是在 [ **RADIUS 用戶端和伺服器**] 下建立新的 radius 用戶端，則您已建立範本，但尚未改變 NPS 功能。 若要更改 NPS 功能，您必須從 NPS 主控台中的正確位置套用範本。

下列程式提供如何建立新範本的指示。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-create-an-nps-template"></a>建立 NPS 範本


1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，展開 [ **範本管理**]，以滑鼠右鍵按一下範本類型（例如 [ **RADIUS 用戶端**]），然後按一下 [ **新增**]。

3. 您可以使用新的 [範本屬性] 對話方塊來設定範本。

## <a name="apply-an-nps-template"></a>套用 NPS 範本

您可以使用您在 [ **範本管理** ] 中建立的範本，方法是流覽至 NPS 主控台中可套用範本的位置。 例如，如果您想要將共用秘密範本套用至 RADIUS 用戶端設定，您可以使用下列程式。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-apply-an-nps-template"></a>套用 NPS 範本

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，展開 [ **Radius 用戶端和伺服器**]，然後展開 [ **radius 用戶端**]。

3.In **RADIUS 用戶端**，在詳細資料窗格中，以滑鼠 **按右鍵您** 要套用 NPS 範本的 RADIUS 用戶端，然後按一下 [內容]。

4. 在 RADIUS 用戶端的 [內容] 對話方塊中，于 [ **選取現有的共用秘密] 範本** 中，從範本清單中選取您要套用的範本。

## <a name="export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

您可以匯出範本以供其他 NPSs 使用，或者您可以將範本匯入 **範本管理** 中，以在本機電腦上使用。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

1. 若要匯出 NPS 範本，請在 NPS 主控台中，以滑鼠右鍵按一下 [ **範本管理**]，然後按一下 [ **將範本匯出至** 檔案]。

2. 若要匯入 NPS 範本，請在 NPS 主控台中，以滑鼠右鍵按一下 [ **範本管理**]，然後按一下 [ **從電腦匯入範本** ] 或 [從檔案匯 **入範本**]。


