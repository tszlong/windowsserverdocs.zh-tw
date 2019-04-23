---
Title: 將 SYSVOL 複寫移轉至 DFS 複寫
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7ea883730fcab83d064fa41f610bde4837510276
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836639"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>將 SYSVOL 複寫移轉至 DFS 複寫


更新日期：2010 年 8 月 25日日

適用於：Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008

網域控制站會使用名為 SYSVOL 的特殊共用的資料夾，將登入指令碼與群組原則物件檔案複寫到其他網域控制站。 Windows 2000 Server 和 Windows Server 2003 使用檔案複寫服務 (FRS) 複寫 SYSVOL，而 Windows Server 2008 使用較新的 DFS 複寫服務，當網域中，使用 Windows Server 2008 網域功能等級和 FRS 的網域，執行較舊的網域功能等級。

若要使用 DFS 複寫來複寫 SYSVOL 資料夾中，您可以建立新的網域使用 Windows Server 2008 網域功能等級，或您可以使用此升級現有的網域，並移轉至複寫的文件所述的程序DFS 複寫。

本文件假設您有 Active Directory 網域服務 (AD DS)、 FRS 以及和分散式檔案系統複寫 （DFS 複寫） 的基本知識。 如需詳細資訊，請參閱 < [Active Directory 網域服務概觀](http://go.microsoft.com/fwlink/?linkid=147787)， [FRS 概觀](http://go.microsoft.com/fwlink/?linkid=121763)，或[DFS 複寫概觀](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> 若要下載本指南的可列印版本，請移至<a href="http://go.microsoft.com/fwlink/?linkid=150375">SYSVOL 複寫移轉指南：FRS 到 DFS 複寫</a>(http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>本指南內容

[SYSVOL 移轉的概念資訊](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [SYSVOL 移轉狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [SYSVOL 移轉程序的概觀](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[SYSVOL 移轉程序](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [移轉至已備妥狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [移轉至重新導向的狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [移轉至已排除的狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[疑難排解的 SYSVOL 移轉](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [SYSVOL 移轉進行疑難排解](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [SYSVOL 移轉回復至先前的穩定狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[SYSVOL 移轉的參考資訊](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [支援的 SYSVOL 移轉案例](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [驗證 SYSVOL 移轉的狀態](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [SYSVOL 移轉工具的動作](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>其他參考資料

[SYSVOL 移轉系列：第 1 部分-SYSVOL 移轉程序簡介](http://go.microsoft.com/fwlink/?linkid=121756)

[SYSVOL 移轉系列：組件 2—Dfsrmig.exe:SYSVOL 移轉工具](http://go.microsoft.com/fwlink/?linkid=121757)

[SYSVOL 移轉系列：第 3 部-移轉至 '備妥' 的狀態](http://go.microsoft.com/fwlink/?linkid=121758)

[SYSVOL 移轉系列：第 4 部-移轉至 'REDIRECTED' 狀態](http://go.microsoft.com/fwlink/?linkid=121759)

[SYSVOL 移轉系列：第 5 部分-移轉至 '刪除' 的狀態](http://go.microsoft.com/fwlink/?linkid=121760)

[適用於 Windows Server 2008 中的分散式的檔案系統的逐步指南](http://go.microsoft.com/fwlink/?linkid=85231)

[FRS 技術參照](http://go.microsoft.com/fwlink/?linkid=121764)

