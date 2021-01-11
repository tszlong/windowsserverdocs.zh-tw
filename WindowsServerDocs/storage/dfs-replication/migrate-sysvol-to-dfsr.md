---
description: 深入了解：將 SYSVOL 複寫移轉至 DFS 複寫
title: 將 SYSVOL 複寫移轉至 DFS 複寫
ms.date: 07/02/2012
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.topic: article
ms.openlocfilehash: 137e8258a8d26792daca5f2148d1de2b7fc62c10
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948184"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>將 SYSVOL 複寫移轉至 DFS 複寫


更新日期：2010 年 8 月 25 日

適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

網域控制站會使用名為 SYSVOL 的特殊共用資料夾，將登入指令碼和群組原則物件檔案複寫至其他網域控制站。 Windows 2000 Server 和 Windows Server 2003 會使用檔案複寫服務 (FRS) 來複寫 SYSVOL，而 Windows Server 2008 則會在使用 Windows Server 2008 網域功能層級的網域中使用較新的 DFS 複寫服務，而將 FRS 用於執行較舊網域功能層級的網域。

若要使用 DFS 複寫來複寫 SYSVOL 資料夾，您可以建立使用 Windows Server 2008 網域功能層級的新網域，也可以使用本文件中討論的程序升級現有的網域，並將複寫遷移至 DFS 複寫。

本文件假設您已具備 Active Directory Domain Services (AD DS)、FRS 和分散式檔案系統複寫 (DFS 複寫) 的基本知識。 如需詳細資訊，請參閱 [Active Directory Domain Services 概觀](https://go.microsoft.com/fwlink/?linkid=147787)、[FRS 概觀](https://go.microsoft.com/fwlink/?linkid=121763)或 [DFS 複寫概觀](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> 要下載本指南的可列印版本，請移至 <a href="https://go.microsoft.com/fwlink/?linkid=150375">SYSVOL 複寫移轉指南：從 FRS 到 DFS 複寫</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>本指南內容

[SYSVOL 移轉概念資訊](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640170(v=ws.10))

  - [SYSVOL 移轉狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641052(v=ws.10))

  - [SYSVOL 移轉程序概觀](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639809(v=ws.10))


[SYSVOL 移轉程序](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639860(v=ws.10))

  - [遷移為就緒狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641193(v=ws.10))

  - [遷移為重新導向狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641340(v=ws.10))

  - [遷移為排除狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640254(v=ws.10))


[SYSVOL 移轉疑難排解](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640395(v=ws.10))

  - [SYSVOL 移轉問題疑難排解](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639976(v=ws.10))

  - [將 SYSVOL 移轉回復到先前的穩定狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640509(v=ws.10))


[SYSVOL 移轉參考資訊](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640293(v=ws.10))

  - [支援的 SYSVOL 移轉](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639854(v=ws.10))

  - [驗證 SYSVOL 移轉狀態](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639789(v=ws.10))

  - [Dfsrmig](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641227(v=ws.10))

  - [SYSVOL 移轉工具動作](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639712(v=ws.10))


## <a name="additional-references"></a>其他參考資料

[SYSVOL 移轉系列：第 1 部分 - SYSVOL 移轉流程簡介](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-1-8211-introduction-to-the-sysvol/ba-p/423456)

[SYSVOL 移轉系列：第 2 部分 - Drsrmig .exe：SYSVOL 移轉工具](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-2-8211-dfsrmig-exe-the-sysvol/ba-p/423470)

[SYSVOL 移轉系列：第 3 部分 - 遷移為「就緒」狀態](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-3-migrating-to-the-prepared-state/ba-p/423503)

[SYSVOL 移轉系列：第 4 部分 - 遷移為「重新導向」狀態](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-4-8211-migrating-to-the-8216/ba-p/423514)

[SYSVOL 移轉系列：第 5 部分 - 遷移為「排除」狀態](https://techcommunity.microsoft.com/t5/storage-at-microsoft/sysvol-migration-series-part-5-8211-migrating-to-the-8216/ba-p/423516)

[Windows Server 2008 的分散式檔案系統逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732863(v=ws.10))

[FRS 技術參考](/previous-versions/windows/it-pro/windows-server-2003/cc759297(v=ws.10))
