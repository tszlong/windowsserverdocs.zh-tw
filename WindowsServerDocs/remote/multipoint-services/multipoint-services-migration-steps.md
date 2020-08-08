---
title: 遷移 MultiPoint 服務的步驟
description: 逐步引導您在 Windows Server 2016 中遷移至 MultiPoint 服務的步驟
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 0d76e3518801829b852c94d0b112b906abbd92c0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948931"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>遷移至 Windows Server 2016 中的 MultiPoint 服務

>適用於：Windows Server 2016

使用下列步驟，再加上您在「遷移規劃」工作表中所收集的資訊，以遷移至 Windows Server 2016 中的 MultiPoint 服務。

## <a name="transfer-server-settings"></a>傳輸伺服器設定
在目的地伺服器上，開啟 [MultiPoint 管理員]。 按一下 [**編輯服務器設定**]。 根據 [遷移規劃] 工作表套用設定。

> [!NOTE]
> 如果您需要在目的地伺服器上啟用磁片保護，請等到設定 MultiPoint 服務後再進行。

## <a name="transfer-station-settings"></a>傳輸站設定
請確定工作站已連線到目的地伺服器，並在您套用工作站設定之前全部對應。 系統會自動偵測到該工作站。 依照每個工作站畫面上的指示，定義使用者工作站和連線 USB 裝置的伺服器對應。 套用您慣用的工作站設定，如遷移規劃工作表中所述。

## <a name="migrate-the-vdi-template"></a>遷移 VDI 範本

在您可以從來源伺服器匯入 VDI 範本之前，請先使用 MultiPoint 管理員啟用目的地伺服器上的虛擬桌面：

1. 移至 [MultiPoint 管理員] 中的 [**虛擬桌面**] 索引標籤。
2. 按一下 [**啟用的虛擬桌面**]。 伺服器將安裝 Hyper-v 角色，然後重新開機。
3. 開啟 MultiPoint 管理員，然後流覽回**虛擬桌面**。
4. 按一下 [匯**入虛擬桌面範本**]。 依照指示從來源伺服器匯入範本。

> [!NOTE]
> 當您匯入虛擬桌面範本時，將會重設任何套用至範本的自訂。

## <a name="next-step"></a>後續步驟
[驗證新的 MultiPoint 服務部署。](multipoint-services-post-migration-steps.md)