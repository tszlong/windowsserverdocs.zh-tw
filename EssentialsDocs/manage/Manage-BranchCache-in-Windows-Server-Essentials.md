---
title: "管理 BranchCache 在 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>管理 BranchCache 在 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

BranchCache 可協助您最佳化網際網路使用量、提升效能的網路的應用程式，與 Windows Server Essentials 伺服器時位於遠端從您的 office 減少 (WAN) 整個區域網路上的資料傳輸或 client 電腦連接到本機伺服器時使用以雲端為基礎的資源，例如 SharePoint Online 媒體櫃。  
  
 與支援 BranchCache，當 client 電腦從遠端 Windows Server Essentials 伺服器，content 要求 content 快取中本機 office。 之後，在相同的 office 其他電腦就可以取得本機而 content 從伺服器下載再試一次透過 WAN content。 這可以改進的應用程式網路效能，並透過 WAN 降低頻寬消耗。  
  
 無論您的 Windows Server Essentials 伺服器是本機或遠端，BranchCache 改善回應伺服器共用資料夾與網頁上（例如 SharePoint Online 媒體櫃）伺服器的時間。  
  
 因為 BranchCache 不需要新的硬體或網路拓撲變更，這個功能為您提供簡單的方式最佳化的頻寬，並改善服務，透過 WAN 存取資源回應時間。  
  
## <a name="branchcache-scenarios"></a>BranchCache 案例  
 您有使用 BranchCache 遠端伺服器的三個基本案例：  
  
-   在 Microsoft Azure 裝載您 Windows Server Essentials 的伺服器。  
  
-   您的 Windows Server Essentials 伺服器裝載第三方服務提供者的資料中心。  
  
-   您的 Windows Server Essentials 伺服器是在另一部 office 在不同的所在位置。  
  
## <a name="distributed-cache-mode"></a>快取分散式的模式  
 在 Windows Server Essentials，BranchCache 實作的*分散式快取模式*，其中一個可用 BranchCache 快取模式有兩個。 快取分散式模式，內容快取中分公司分散在 client 的電腦。 因為需要不額外的硬體或拓撲變更時，此模式會適用於小型辦公室使用遠端伺服器或存取以雲端為基礎的服務，例如 SharePoint Online 使用本機伺服器。 當您將在 Windows Server Essentials BranchCache 上時，係分散式快取模式。  
  
> [!NOTE]
>  在大分公司使用多個子網路，或使用大量員工使用網路應用程式，可以是很有幫助實作 BranchCache 在*裝載快取模式*。 裝載快取模式，內容快取儲存分公司中的一或多個裝載快取伺服器上。
  
## <a name="requirements"></a>需求  
 若要在 Windows Server Essentials 使用 BranchCache，您的伺服器及 client 的電腦必須符合下列需求：  
  
-   必須執行伺服器作業系統 Windows Server Essentials 或 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter 作業系統與 Windows Server Essentials 體驗角色。  
  
     在 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter server BranchCache 中新增了當您加入「Windows Server Essentials 體驗角色。 若要在 BranchCache，您必須登入 Windows Server Essentials 儀表板的網域系統管理員認證。  
  
-   Client 的電腦必須執行 Windows 7 企業版、Windows 7 旗艦版、Windows 8 企業版或 Windows 8.1 企業版的作業系統。  
  
-   快取分散式模式，client 的所有電腦必須都是相同子網路上。  
  
    > [!NOTE]
    >  如果您未加入網域連接您的 Windows Server Essentials 伺服器的 client 電腦，這些電腦會排除預設快取。 若要在快取中包含 client 是未加入網域的電腦，請執行[讓-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) Windows PowerShell cmdlet client 電腦上的。 如需詳細資訊，請查看[Windows PowerShell 中的 BranchCache Cmdlet](https://technet.microsoft.com/library/hh848392.aspx)。  
 
  
## <a name="turn-branchcache-on"></a>打開 BranchCache  
 若要打開 BranchCache 分散式快取模式中，您只要按一下 Windows Server Essentials 儀表板上的按鈕。 快取會立即開始，會自動執行。  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>若要在 Windows Server Essentials BranchCache 上  
  
1.  Windows Server Essentials 的系統管理員帳號伺服器上登入。  
  
2.  Windows Server Essentials 儀表板，請按一下**設定**。  
  
     [設定] 精靈會開啟。  
  
3.  按一下**BranchCache**。  
  
4.  在**BranchCache 設定**頁面上，按**打開**。  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>使用 Windows PowerShell 將 BranchCache 關閉  
 若要檢查 BranchCache（功能或停用）的狀態，並將 BranchCache 關閉，您可以使用 Windows PowerShell。  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>若要將開關 BranchCache 使用 Windows PowerShell  
  
1.  在伺服器上，請以系統管理員身分開放 Windows PowerShell。 在**[開始]**頁面上，以滑鼠右鍵按一下**Windows PowerShell**，按一下 [**以系統管理員身分執行**，，然後按一下**[是]**。  
  
2.  輸入下列 cmdlet 在命令提示字元中的任何問題。  
  
    -   若要查看的狀態 BranchCache（功能或停用），請輸入：  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   若要關閉 BranchCache，請輸入：  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   若要關閉 BranchCache，請輸入：  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>也了  
    
-   [BranchCache 概觀](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
