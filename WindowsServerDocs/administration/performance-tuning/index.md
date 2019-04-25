---
title: Windows Server 2016 效能調整指導方針
description: Windows Server 2016 的效能調整指導方針
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1445ae40428bc8626f8e2a12cbfc2a1e6f41592f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891959"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Windows Server 2016 的效能調整指導方針

當您在您的組織中執行伺服器系統時，使用預設伺服器設定可能無法滿足您的商業需求。 例如，您的伺服器可能需要可能的最低能源消耗、可能的最低延遲，或可能的最高輸送量。 此指南提供一組可用來調整 Windows Server 2016 中的伺服器設定的指導方針，並取得增量效能或提高能源效率，尤其是當工作負載的本質隨著時間變化很小時。

您的調整變更必須考量您伺服器的硬體、工作負載、電源預算與效能目標。 此指南說明每個設定與其潛在效果，以協助您做出有關系統、工作負載、效能與能源使用目標相關性的明智決策。

> [!warning]
> 登錄設定與調整參數在不同版本的 Windows Server 之間已大幅變更。 請務必使用最新的調整指導方針以避免非預期的結果。

## <a name="in-this-guide"></a>本指南內容
此指南將 Windows Server 2016 效能與調整指導方針組織在三個調整類別中：

|伺服器硬體 | 伺服器角色 | 伺服器子系統 |
|:---:|:---:|:---:|
|[硬體效能考量](hardware/index.md) |[Active Directory 伺服器](role/active-directory-server/index.md) |[快取與記憶體管理](subsystem/cache-memory-management/index.md)|
|[硬體電源考量](hardware/power.md)|[檔案伺服器](role/file-server/index.md)|[網路子系統](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Hyper-V 伺服器](role/hyper-v-server/index.md)|[儲存空間直接存取](subsystem/storage-spaces-direct/index.md)|
||[遠端桌面服務](role/remote-desktop/session-hosts.md)|[軟體定義網路 (SDN)](subsystem/software-defined-networking/index.md)|
||[網頁伺服器](role/web-server/index.md)||
||[Windows Server 容器](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>此版本中的變更

### <a name="sections-added"></a>新增的區段
- [Nano 伺服器安裝類型設定考量](../../get-started/getting-started-with-nano-server.md)


- [軟體定義網路](subsystem/software-defined-networking/index.md)，包括 [HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) 與 [SLB 閘道設定指導方針](subsystem/software-defined-networking/slb-gateway-performance.md)

- [儲存空間直接存取](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 與 HTTP2](role/web-server/http-performance.md)

- [Windows Server 容器](role/windows-server-container/index.md)

### <a name="sections-changed"></a>變更的區段

- 更新為 [Active Directory 指導方針](role/active-directory-server/index.md)區段

- 更新為[檔案伺服器指導方針](role/file-server/index.md)區段

- 更新為[網頁伺服器指導方針](role/web-server/index.md)區段

- 更新為[硬體電源指導方針](hardware/power.md)區段

- 更新為 [PowerShell 調整指導方針](powershell/index.md)區段

- 大幅更新為 [Hyper-V 指導方針](role/hyper-v-server/index.md)區段

- *已移除工作負載的效能調整*、對相關資源的指標已新增至[其他調整資源文章](additional-resources.md)

- *移除專用儲存體區段*，改為新的[儲存空間直接存取](subsystem/storage-spaces-direct/index.md)區段與標準 Technet 內容

- *移除專用網路區段*，改為新的標準 Technet 內容  
