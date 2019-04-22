---
title: 準備移轉至 MultiPoint 服務
description: 描述資訊來收集您才能移轉至 Windows Server 2016 中的 MultiPoint 服務
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824379"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>準備移轉到 Windows Server 2016 中的 MultiPoint 服務

>適用於：Windows Server 2016

使用下列資訊來收集您需要從執行舊版的 Windows Server 2016 執行 Windows Server 2016 RTM 的目的地伺服器的來源伺服器移轉的 MultiPoint 服務角色服務的資訊。

至少，您必須是來源伺服器與目的地伺服器上的 Administrators 群組的成員，才能安裝、 移除或設定 MultiPoint 服務。

>[!NOTE]
> 移轉資料儲存至使用者資料夾或共用資料夾的步驟不會提供指引。 請確定您在開始移轉之前使用者存備份其資料。

使用 MultiPoint 管理員來擷取移轉所需的資訊。 您必須使用 MultiPoint 管理員的伺服器系統管理員權限。

記錄中的 MultiPoint 伺服器、 使用者和環境設定[移轉資料收集工作表](multipoint-services-migration-worksheet.md)。 使用下列步驟來收集該資訊。

## <a name="multipoint-server-settings-for-the-local-server"></a>在本機伺服器的 multiPoint 伺服器設定
1. 啟動 MultiPoint 管理員。
2. 在 **首頁**索引標籤上，選取 本機伺服器，然後按一下 **編輯伺服器設定。**
3. 資料工作表中記錄的設定。
4. 關閉 [設定] 視窗。

## <a name="managed-servers-and-computers"></a>受管理的伺服器和電腦

您可以找到受管理的伺服器和電腦的名稱上**首頁**MultiPoint 管理員中的索引標籤。

## <a name="station-settings"></a>站台設定
如果自動登入或顯示方向會設定為站台，請使用下列步驟來擷取該資訊。 否則您可以略過此步驟。

若要擷取的站台設定：

1. 移至**站台**MultiPoint 管理員中的索引標籤。
2. 在中尋找具有"yes"的站台**自動登入**資料行。
3. 選取該站台，然後再按一下**設定站台**。
4. 記錄使用者用來自動登入。

若要擷取的顯示方向設定，請檢視**的站台設定**的每個站台。

## <a name="list-of-users"></a>使用者清單
1. 按一下 **使用者**MultiPoint 管理員中的索引標籤。
2. 資料錄**系統管理員**並**MultiPoint 儀表板使用者**accoutns。
3. 記錄的標準使用者。

## <a name="vdi-template-location"></a>VDI 範本位置
 如果您先前啟用的 VDI 範本功能，記錄 VDI 範本的位置。 只要在來源與目的地伺服器都在相同網路上，您可以使用 MultiPoint 管理員匯入範本。
 
## <a name="next-step"></a>後續步驟
您現在準備好[移轉至 MultiPoint 服務](multipoint-services-migration-steps.md)在 Windows Server 2016 RTM 版本。