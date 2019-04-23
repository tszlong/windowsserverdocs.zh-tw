---
title: 管理 Windows Server Essentials 中的 BranchCache
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828799"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 BranchCache

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

BranchCache 可以協助您最佳化網際網路使用方式、 改善的網路上的應用程式的效能及減少您廣域網路 (WAN) 上的流量，當 Windows Server Essentials 伺服器位於遠端在您的辦公室，或用戶端電腦連接本機伺服器使用雲端式資源，例如 SharePoint Online 文件庫。  
  
 啟用 branchcache，當用戶端電腦要求內容從遠端的 Windows Server Essentials 伺服器，內容會快取在當地分公司。 之後，同一個辦公室內的其他電腦便可以在本機取得內容，而不需要透過 WAN 從伺服器重新下載內容。 這會提升網路應用程式的效能，並降低 WAN 上的頻寬使用量。  
  
 無論您的 Windows Server Essentials 伺服器是本機或遠端，BranchCache 可以改善伺服器共用資料夾和 （例如 SharePoint Online 文件庫中） 伺服器裝載的 Web 內容的回應時間。  
  
 由於 BranchCache 不需要新增硬體或網路拓撲變更，因此這項功能可讓您輕鬆地最佳化 WAN 上的頻寬使用量，並改善透過 WAN 存取之服務和資源的回應時間。  
  
## <a name="branchcache-scenarios"></a>BranchCache 案例  
 搭配遠端伺服器使用 BranchCache 的基本案例有三種：  
  
-   Windows Server Essentials 伺服器會裝載於 Microsoft Azure。  
  
-   Windows Server Essentials 伺服器會裝載於協力廠商服務提供者的資料中心。  
  
-   Windows Server Essentials 伺服器處於不同的實體位置中的其他辦公室。  
  
## <a name="distributed-cache-mode"></a>分散式快取模式  
 在 Windows Server Essentials 中實作 BranchCache*分散式快取模式*，其中一種以 BranchCache 所提供的兩個快取模式。 在分散式快取模式中，分公司的內容快取會分散在用戶端電腦之間。 由於不需要額外的硬體或拓撲變更，因此這種模式非常適合使用遠端伺服器或本機伺服器來存取 SharePoint Online 等雲端架構服務的小型辦公室。 當您開啟 Windows Server Essentials 中的 BranchCache 時，被實作分散式快取模式。  
  
> [!NOTE]
>  在有幾個或許多員工使用網路應用程式的較大型分公司中，以「託管快取模式」(hosted cache mode) 實作 BranchCache 會很有幫助。 在託管快取模式中，內容快取會儲存在分公司的一或多部託管快取伺服器上。
  
## <a name="requirements"></a>需求  
 若要使用 Windows Server Essentials 中的 BranchCache，您的伺服器和用戶端電腦必須符合下列需求：  
  
-   伺服器必須 Windows Server Essentials 作業系統或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 作業系統執行 Windows Server Essentials 體驗角色。  
  
     在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 的伺服器，當您新增 Windows Server Essentials 體驗角色時，會新增 BranchCache。 若要開啟 BranchCache，您必須登入 Windows Server Essentials 儀表板以網域系統管理員認證。  
  
-   用戶端電腦必須執行 Windows 7 企業版、 Windows 7 旗艦版、 Windows 8 Enterprise 或 Windows 8.1 Enterprise 作業系統。  
  
-   在分散式快取模式中，所有用戶端電腦都必須在同一個子網路上。  
  
    > [!NOTE]
    >  如果您將用戶端電腦連線到 Windows Server Essentials 伺服器，但未將這些電腦加入網域，預設會從快取中排除這些電腦。 若要在快取中包含未加入網域的用戶端電腦，請在用戶端電腦上執行 [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) Windows PowerShell Cmdlet。 如需詳細資訊，請參閱 [Windows PowerShell 中的 BranchCache Cmdlet](https://technet.microsoft.com/library/hh848392.aspx)。  
 
  
## <a name="turn-branchcache-on"></a>開啟 BranchCache  
 若要開啟 BranchCache 分散式快取模式中，您只要按一下按鈕，以在 Windows Server Essentials 儀表板上。 快取會立即開始並無障礙地執行。  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>開啟 Windows Server Essentials 中的 BranchCache  
  
1.  在 Windows Server Essentials 伺服器與您的系統管理員帳戶登入。  
  
2.  在 Windows Server Essentials 儀表板，按一下 **設定**。  
  
     [設定精靈] 隨即開啟。  
  
3.  按一下 [BranchCache]。  
  
4.  在 [BranchCache 設定]  頁面上，按一下 [開啟] 。  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>使用 Windows PowerShell 開啟或關閉 BranchCache  
 您可以使用 Windows PowerShell 來檢查 BranchCache 的狀態 ([啟用] 或 [停用])，以及開啟或關閉 BranchCache。  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>使用 Windows PowerShell 開啟或關閉 BranchCache  
  
1.  在伺服器上，以系統管理員身分開啟 Windows PowerShell。 在 [開始] 頁面上，以滑鼠右鍵按一下 [Windows PowerShell]，然後按一下 [以系統管理員身分執行]，再按一下 [是]。  
  
2.  在命令提示字元下，輸入下列任何一個 Cmdlet。  
  
    -   若要檢查 BranchCache 的狀態 ([啟用] 或 [停用])，請輸入：  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   若要開啟 BranchCache，請輸入：  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   若要關閉 BranchCache，請輸入：  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>另請參閱  
    
-   [BranchCache 概觀](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
