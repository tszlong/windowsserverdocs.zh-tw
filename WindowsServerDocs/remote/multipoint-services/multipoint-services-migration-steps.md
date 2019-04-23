---
title: MultiPoint 服務的移轉步驟
description: 將逐步引導您逐步完成移轉至 Windows Server 2016 中的 MultiPoint 服務
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854809"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>移轉至 Windows Server 2016 中的 MultiPoint 服務

>適用於：Windows Server 2016

使用下列步驟，再加上您要移轉至 Windows Server 2016 中的 MultiPoint 服務的移轉規劃工作表中所收集的資訊。

## <a name="transfer-server-settings"></a>傳輸伺服器設定
在目的地伺服器上，開啟 MultiPoint 管理員。 按一下 **編輯伺服器設定**。 適用於根據移轉的規劃工作表的設定。

> [!NOTE]
> 如果您要啟用磁碟保護在目的地伺服器上的，等到設定 MultiPoint 服務之後。

## <a name="transfer-station-settings"></a>傳輸的站台設定
請確定站台，可連接到目的地伺服器，而且所有之前套用的站台設定對應。 站台會自動偵測。 若要定義使用者站台和連接的 USB 裝置的伺服器對應每個站台畫面上，遵循的指示。 移轉的規劃工作表中所述，適用於您慣用的站台的設定。

## <a name="migrate-the-vdi-template"></a>移轉 VDI 範本

您可以從來源伺服器匯入 VDI 範本之前，請使用 MultiPoint 管理員啟用目的地伺服器上的虛擬桌面：

1. 移至**虛擬桌面**MultiPoint 管理員中的索引標籤。
2. 按一下 **啟用虛擬桌面**。 伺服器會安裝 HYPER-V 角色，並再重新啟動。
3. 開啟 MultiPoint 管理員，並瀏覽回到**虛擬桌面**。
4. 按一下 **匯入虛擬桌面範本**。 請遵循從來源伺服器匯入範本的指示。

> [!NOTE]
> 當您匯入虛擬桌面範本時，就會重設任何套用至範本的自訂。 

## <a name="next-step"></a>後續步驟
[驗證新的 MultiPoint 服務部署。](multipoint-services-post-migration-steps.md)