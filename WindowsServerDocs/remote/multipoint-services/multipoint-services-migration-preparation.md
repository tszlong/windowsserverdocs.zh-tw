---
title: 準備移轉至 MultiPoint 服務
description: 說明在 Windows Server 2016 中遷移至 MultiPoint 服務之前要收集的資訊
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 3333570aae34f2c102c36382eeffcb5411b7dd83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858701"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>準備遷移至 Windows Server 2016 中的 MultiPoint 服務

>適用於︰Windows Server 2016

使用下列資訊，從執行舊版 Windows Server 2016 的來源伺服器，收集將 MultiPoint 服務角色服務遷移至執行 Windows Server 2016 RTM 的目的地伺服器所需的資訊。

您至少必須是來源伺服器和目的地伺服器上的 Administrators 群組成員，才能安裝、移除或設定 MultiPoint 服務。

>[!NOTE]
> 此處的步驟不會提供將儲存的資料移轉至使用者資料夾或共用資料夾的指引。 開始進行遷移之前，請確定使用者已備份其資料。

使用 MultiPoint 管理員來抓取遷移所需的資訊。 您將需要伺服器管理員許可權，才能使用 MultiPoint 管理員。

在 [[遷移資料收集] 工作表](multipoint-services-migration-worksheet.md)中記錄 MultiPoint 伺服器、使用者和環境設定。 使用下列步驟來收集該資訊。

## <a name="multipoint-server-settings-for-the-local-server"></a>本機伺服器的 MultiPoint 伺服器設定
1. 啟動 MultiPoint 管理員。
2. 在 [**首頁**] 索引標籤上，選取本機伺服器，然後按一下 [**編輯服務器設定]。**
3. 記錄資料工作表中的設定。
4. 關閉 [設定] 視窗。

## <a name="managed-servers-and-computers"></a>受管理的伺服器和電腦

您可以在 [MultiPoint 管理員] 的 [**首頁**] 索引標籤上找到受管理伺服器和電腦的名稱。

## <a name="station-settings"></a>工作站設定
如果已針對工作站設定自動登入或顯示方向，請使用下列步驟來取出該資訊。 否則，您可以略過此步驟。

若要取出工作站設定：

1. 移至 [MultiPoint 管理員] 中的 [**工作站**] 索引標籤。
2. 在 [**自動登**入] 資料行中尋找具有「是」的工作站。
3. 選取該站，然後按一下 [**設定站**]。
4. 記錄用於自動登入的使用者。

若要取出 [顯示方向] 設定，請查看每個工作站的**工作站設定**。

## <a name="list-of-users"></a>使用者清單
1. 按一下 [MultiPoint 管理員] 中的 [**使用者**] 索引標籤。
2. 記錄**系統管理員**和**MultiPoint 儀表板使用者**accoutns。
3. 記錄標準使用者。

## <a name="vdi-template-location"></a>VDI 範本位置
 如果您先前已啟用 VDI 範本功能，請記錄 VDI 範本的位置。 只要來源和目的地伺服器位於相同的網路上，您就可以使用 MultiPoint 管理員匯入範本。
 
## <a name="next-step"></a>後續步驟
您現在已經準備好在 Windows Server 2016 RTM 版本中[遷移至 MultiPoint 服務](multipoint-services-migration-steps.md)。