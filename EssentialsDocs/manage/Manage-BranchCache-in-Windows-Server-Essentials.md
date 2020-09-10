---
title: 管理 Windows Server Essentials 中的 BranchCache
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 52493dae886eb8f74a6276854c7b7cce2f77470f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623204"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 BranchCache

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

BranchCache 可協助您優化網際網路使用方式、改善網路應用程式的效能，並減少廣域網路的流量 (WAN) 當 Windows Server Essentials 伺服器位於您辦公室的遠端時，或當用戶端電腦連線到本機伺服器時，使用的是 SharePoint Online 程式庫之類的雲端資源。

 在啟用 BranchCache 的情況下，當用戶端電腦從遠端 Windows Server Essentials 伺服器要求內容時，會在本機辦公室快取內容。 之後，同一個辦公室內的其他電腦便可以在本機取得內容，而不需要透過 WAN 從伺服器重新下載內容。 這會提升網路應用程式的效能，並降低 WAN 上的頻寬使用量。

 無論您的 Windows Server Essentials 伺服器是本機或遠端，BranchCache 都可以改善伺服器共用資料夾的回應時間，以及伺服器上裝載的 Web 內容 (例如 SharePoint Online 文件庫) 。

 由於 BranchCache 不需要新增硬體或網路拓撲變更，因此這項功能可讓您輕鬆地最佳化 WAN 上的頻寬使用量，並改善透過 WAN 存取之服務和資源的回應時間。

## <a name="branchcache-scenarios"></a>BranchCache 案例
 搭配遠端伺服器使用 BranchCache 的基本案例有三種：

-   您的 Windows Server Essentials 伺服器裝載于 Microsoft Azure。

-   您的 Windows Server Essentials 伺服器裝載于協力廠商服務提供者的資料中心。

-   您的 Windows Server Essentials 伺服器位於不同實體位置的另一個辦公室。

## <a name="distributed-cache-mode"></a>分散式快取模式
 在 Windows Server Essentials 中，BranchCache 會以 *分散式*快取模式（branchcache 中可用的兩個快取模式之一）來執行。 在分散式快取模式中，分公司的內容快取會分散在用戶端電腦之間。 由於不需要額外的硬體或拓撲變更，因此這種模式非常適合使用遠端伺服器或本機伺服器來存取 SharePoint Online 等雲端架構服務的小型辦公室。 當您在 Windows Server Essentials 中開啟 BranchCache 時，會實作為分散式快取模式。

> [!NOTE]
>  在有幾個或許多員工使用網路應用程式的較大型分公司中，以「託管快取模式」**(hosted cache mode) 實作 BranchCache 會很有幫助。 在託管快取模式中，內容快取會儲存在分公司的一或多部託管快取伺服器上。

## <a name="requirements"></a>需求
 若要在 Windows Server Essentials 中使用 BranchCache，您的伺服器和用戶端電腦必須符合下列需求：

-   伺服器必須執行 Windows server Essentials 作業系統或 windows server 2012 R2 Standard 或 windows server 2012 R2 Datacenter 作業系統與 Windows Server Essentials 體驗角色。

     在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter Server 上，當您新增 Windows Server Essentials 體驗角色時，就會新增 BranchCache。 若要開啟 BranchCache，您將需要使用網域系統管理員認證登入 Windows Server Essentials 儀表板。

-   用戶端電腦必須執行 Windows 7 企業版、Windows 7 旗艦版、Windows 8 企業版或 Windows 8.1 企業版作業系統。

-   在分散式快取模式中，所有用戶端電腦都必須在同一個子網路上。

    > [!NOTE]
    >  如果您將用戶端電腦連線到 Windows Server Essentials 伺服器，但未將這些電腦加入網域，預設會從快取中排除這些電腦。 若要在快取中包含未加入網域的用戶端電腦，請在用戶端電腦上執行 [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) Windows PowerShell Cmdlet。 如需詳細資訊，請參閱 [Windows PowerShell 中的 BranchCache Cmdlet](https://technet.microsoft.com/library/hh848392.aspx)。


## <a name="turn-branchcache-on"></a>開啟 BranchCache
 若要以分散式快取模式開啟 BranchCache，只要按一下 [Windows Server Essentials 儀表板] 上的按鈕即可。 快取會立即開始並無障礙地執行。

#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>開啟 Windows Server Essentials 中的 BranchCache

1.  使用您的系統管理員帳戶登入 Windows Server Essentials 伺服器。

2.  在 [Windows Server Essentials 儀表板] 上，按一下 [ **設定**]。

     [設定精靈] 隨即開啟。

3.  按一下 [BranchCache]****。

4.  在 [BranchCache 設定] **** 頁面上，按一下 [開啟] ****。

## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>使用 Windows PowerShell 開啟或關閉 BranchCache
 您可以使用 Windows PowerShell 來檢查 BranchCache 的狀態 ([啟用] 或 [停用])，以及開啟或關閉 BranchCache。

#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>使用 Windows PowerShell 開啟或關閉 BranchCache

1.  在伺服器上，以系統管理員身分開啟 Windows PowerShell。 在 [開始]**** 頁面上，以滑鼠右鍵按一下 [Windows PowerShell]****，然後按一下 [以系統管理員身分執行]****，再按一下 [是]****。

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

## <a name="additional-references"></a>其他參考資料

-   [BranchCache 概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831696(v=ws.11))

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)