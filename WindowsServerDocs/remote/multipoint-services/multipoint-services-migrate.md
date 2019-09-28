---
title: 遷移至 Windows Server 2016 中的 MultiPoint 服務
description: 瞭解如何從舊版 MultiPoint 服務遷移
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 297de9ee2450856e24b9196a8bfb312991657e6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389036"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Windows Server 2016 中的 MultiPoint 服務遷移
>適用於：Windows Server 2016

您可以從舊版的 Windows Server 2016 MultiPoint 服務遷移至 MultiPoint 服務的 RTM 版本。 下列資訊提供準備資訊以及遷移和驗證步驟。

遷移檔和工具可讓您輕鬆地將伺服器角色設定和資料從現有的伺服器遷移到執行 Windows Server 2016 的目的地伺服器。 使用本指南中所述的程序，即可簡化移轉程序、縮短移轉時間、提高移轉程序的準確性，並且有助於消除可能在移轉過程中發生的衝突。 

## <a name="what-to-know-before-you-begin"></a>開始之前要知道的事項
開始進行遷移程式之前，請注意下列事項：

- 此遷移程式不會自動收集或記錄 MultiPoint 服務角色上的應用程式設定。 您應該為任何您想要遷移的應用程式建立自訂的遷移計畫。 這也適用于在 MultiPoint 服務中使用虛擬桌面功能的情況。
- 本指南不提供將儲存在 MultiPoint 伺服器上的使用者或共用資料夾中的資料移動的指引。 這適用于一般工作站和虛擬桌面工作站。
- 本指南不包含如何在來源伺服器執行多個角色時進行遷移的指示。 如果您的伺服器執行多個角色，您必須根據角色遷移指南中提供的資訊，設計伺服器環境特有的自訂遷移程式。
- 本指南不包含遷移遠端桌面服務 CAL 的資訊。 如需這份資訊，請參閱[遷移遠端桌面服務用戶端存取使用權（RDS cal）](https://technet.microsoft.com/library/dd851844.aspx)。

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Windows Server 2016 中 MultiPoint 服務支援的遷移案例
MultiPoint 服務角色服務適用于 Windows Server 2016 Standard 和 Datacenter。 此遷移指南說明如何將 Multipoint 服務角色服務從執行 Windows Server 2016 的來源伺服器遷移至執行相同版本的目的地伺服器。

## <a name="scenarios-that-are-not-supported"></a>不支援的案例

以下為不支援的移轉案例：

- 從 Windows MultiPoint Server 2012 和2011進行遷移或升級。
- 從來源伺服器遷移到在安裝了不同系統 UI 語言的作業系統上執行的目的地伺服器。
- 將 MultiPoint 服務角色服務從實體伺服器遷移至虛擬機器。
- 從 MultiPoint 伺服器遷移任何應用程式或應用程式設定。

## <a name="the-impact-of-migration-on-multipoint-services"></a>在 MultiPoint 服務上進行遷移的影響
請注意，在遷移期間將無法使用 MultiPoint 服務角色。 請規劃在離峰時間執行資料移轉，盡可能縮短停機時間以降低對使用者的影響， 並且通知使用者在該時間內將無法使用資源。

## <a name="migration-information-and-steps"></a>遷移資訊和步驟
使用下列資訊來規劃和執行 MultiPoint 服務遷移：

- [收集您需要的資訊以進行遷移。](multipoint-services-migration-preparation.md)
- [遷移 MultiPoint 服務角色服務。](multipoint-services-migration-steps.md)
- [驗證遷移，並執行任何遷移後的清除工作](multipoint-services-post-migration-steps.md)