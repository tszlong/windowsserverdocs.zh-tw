---
title: 移轉至 Windows Server 2016 中的 MultiPoint 服務
description: 了解如何從舊版的 MultiPoint 服務移轉
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861199"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Windows Server 2016 中的 multiPoint 服務移轉
>適用於：Windows Server 2016

您可以從舊版 Windows Server 2016 MultiPoint 服務移轉到 RTM 版本的 MultiPoint 服務。 下列資訊提供資訊和移轉和驗證步驟的準備。

移轉文件和工具可簡化將伺服器角色設定與現有的伺服器執行 Windows Server 2016 的目的地伺服器的資料移轉。 使用本指南中所述的程序，即可簡化移轉程序、縮短移轉時間、提高移轉程序的準確性，並且有助於消除可能在移轉過程中發生的衝突。 

## <a name="what-to-know-before-you-begin"></a>要在您開始前所知道的事項
在開始移轉程序之前，請注意下列：

- 移轉程序不會不會自動收集，或記錄在 MultiPoint 服務角色上的應用程式的設定。 您應該建立自訂的移轉計劃針對任何您想要移轉的應用程式。 這也是，則為 true 時使用 MultiPoint 服務中的 [虛擬桌面] 功能。
- 本指南不提供指引來移動資料儲存在使用者或 MultiPoint server 上的共用資料夾。 這適用於一般的站台和虛擬桌面站台。
- 本指南不包含有關如何移轉來源伺服器執行多個角色時的指示。 如果您的伺服器執行多個角色，您需要設計是根據角色移轉指南提供資訊的伺服器環境特有的自訂移轉程序。
- 本指南不包含移轉遠端桌面服務 CAL 的資訊。 這項資訊，請參閱[移轉遠端桌面服務用戶端存取使用權 (RDS Cal)](https://technet.microsoft.com/library/dd851844.aspx)。

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Windows Server 2016 中的 MultiPoint 服務的支援的移轉案例
Windows Server 2016 Standard 和 Datacenter 中使用 MultiPoint 服務角色服務。 本移轉指南說明如何從執行 Windows Server 2016 到目的地伺服器執行相同版本的來源伺服器移轉的 Multipoint 服務角色服務。

## <a name="scenarios-that-are-not-supported"></a>不支援的案例

以下為不支援的移轉案例：

- 移轉，或從 Windows MultiPoint Server 2012 和 2011年升級。
- 從來源伺服器移轉到目的地伺服器使用不同的系統安裝的 UI 語言的作業系統上執行。
- 從實體伺服器的 MultiPoint 服務角色服務移轉至虛擬機器中。
- 從 MultiPoint Server 進行移轉的任何應用程式或應用程式設定。

## <a name="the-impact-of-migration-on-multipoint-services"></a>在 MultiPoint 服務上的移轉的影響
請注意，MultiPoint 服務角色將無法在移轉期間。 請規劃在離峰時間執行資料移轉，盡可能縮短停機時間以降低對使用者的影響， 並且通知使用者在該時間內將無法使用資源。

## <a name="migration-information-and-steps"></a>移轉的資訊和步驟
使用下列資訊來規劃及執行您的 MultiPoint 服務移轉：

- [收集您需要移轉的資訊。](multipoint-services-migration-preparation.md)
- [移轉 MultiPoint 服務角色服務。](multipoint-services-migration-steps.md)
- [驗證移轉，並執行任何移轉後的清除工作](multipoint-services-post-migration-steps.md)