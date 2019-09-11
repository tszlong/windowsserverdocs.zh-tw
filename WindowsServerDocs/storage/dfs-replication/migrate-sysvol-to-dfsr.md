---
title: 將 SYSVOL 複寫移轉至 DFS 複寫
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 30222d0737ac3cd2947fc3b2d70a0df77b606299
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871988"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>將 SYSVOL 複寫移轉至 DFS 複寫


更新日期：2010年8月25日

適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

網域控制站會使用名為 SYSVOL 的特殊共用資料夾，將登入腳本和群組原則物件檔案複寫至其他網域控制站。 Windows 2000 Server 和 Windows Server 2003 會使用檔案複寫服務（FRS）來複寫 SYSVOL，而 Windows Server 2008 則會在使用 Windows Server 2008 網域功能等級的網域中使用較新的 DFS 複寫服務，而將 FRS 用於執行較舊的網域功能等級。

若要使用 DFS 複寫來複寫 SYSVOL 資料夾，您可以建立使用 Windows Server 2008 網域功能等級的新網域，也可以使用本檔中討論的程式升級現有的網域，並將複寫遷移至DFS 複寫。

本檔假設您已具備 Active Directory Domain Services （AD DS）、FRS 和分散式檔案系統複寫（DFS 複寫）的基本知識。 如需詳細資訊，請參閱[Active Directory Domain Services 總覽](http://go.microsoft.com/fwlink/?linkid=147787)、 [FRS 總覽](http://go.microsoft.com/fwlink/?linkid=121763)或[DFS 複寫的總覽](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> 若要下載本指南的可列印版本，請<a href="http://go.microsoft.com/fwlink/?linkid=150375">移至 SYSVOL 複寫遷移指南：要 DFS 複寫</a>的 FRS （ http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>本指南內容

[SYSVOL 遷移概念資訊](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [SYSVOL 遷移狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [SYSVOL 遷移程式的總覽](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[SYSVOL 遷移程式](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [遷移至備妥的狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [遷移至重新導向的狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [遷移至 [已刪除] 狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[針對 SYSVOL 遷移進行疑難排解](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [針對 SYSVOL 遷移問題進行疑難排解](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [將 SYSVOL 遷移復原到先前的穩定狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[SYSVOL 遷移參考資訊](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [支援的 SYSVOL 遷移案例](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [驗證 SYSVOL 遷移的狀態](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Drsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [SYSVOL 遷移工具動作](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>其他參考資料

[SYSVOL 遷移系列：第1部分-SYSVOL 遷移程式簡介](http://go.microsoft.com/fwlink/?linkid=121756)

[SYSVOL 遷移系列：第2部分— Drsrmig：SYSVOL 遷移工具](http://go.microsoft.com/fwlink/?linkid=121757)

[SYSVOL 遷移系列：第3部分：遷移至「備妥」狀態](http://go.microsoft.com/fwlink/?linkid=121758)

[SYSVOL 遷移系列：第4部分—遷移至「重新導向」狀態](http://go.microsoft.com/fwlink/?linkid=121759)

[SYSVOL 遷移系列：第5部分：遷移至「已刪除」狀態](http://go.microsoft.com/fwlink/?linkid=121760)

[Windows Server 2008 中分散式檔案系統的逐步指南](http://go.microsoft.com/fwlink/?linkid=85231)

[FRS 技術參考](http://go.microsoft.com/fwlink/?linkid=121764)

