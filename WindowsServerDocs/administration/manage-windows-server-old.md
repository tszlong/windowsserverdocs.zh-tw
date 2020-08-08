---
title: 管理 Windows Server
description: 了解管理 Windows Server 的相關工具、建議和指導方針
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 55b33c7ab74ac9295d42ef884540d551b1ad6ec4
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992431"
---
# <a name="manage-windows-server"></a>管理 Windows Server

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](/search/index?dataSource=previousVersions&search=Windows+Server)以取得特定資訊。

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="manage icon" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>管理</h2>
                <p>在您的環境中部署 Windows Server，包括您所需特性和功能的角色後，接下來就是管理這些伺服器。 Windows Server 包含數個工具，可協助您了解您的 Windows Server 環境、管理特定伺服器、微調效能，以及最終自動化許多管理工作。 </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
</ul>

## <a name="manage-windows-server-systems-and-environments"></a>管理 Windows Server 系統和環境
您用來管理 Windows Server 執行個體的工具大部分取決於您部署的系統類型 (具備桌面體驗或 Server Core 的 Windows Server)、實體或虛擬機器，以及您的伺服器所在位置。 請使用下列資訊來執行 Windows Server 的基本管理工作。

使用下表判斷應於何時使用何種工具。

| 我是   | 安裝和執行 Windows Admin Center | 在 Windows Server 上執行伺服器管理員 | 在 Windows 10 的 RSAT 中執行伺服器管理員 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| 位在 Windows 10 電腦前 | X  |                                      | X                                        |
| 位在執行桌面體驗的 Windows Server 系統前 | X | X | X |
| 位在執行 Server Core 的 Windows Server 系統前 |X (在 Windows 10 上安裝，用於管理 Server Core) | | X |
| 不在 Windows Server 系統前 |X | | X |
| 不在 Windows Server 系統前，但我的系統確實擁有桌面體驗 |X | 使用 RDS 遠端連接到伺服器，然後使用伺服器管理員 | X |

除了如下所述的工具，您也可以使用[遠端桌面服務](../remote/remote-desktop-services/welcome-to-rds.md)來存取內部部署、遠端以及虛擬伺服器。 接著您可以使用伺服器管理員來執行管理工作。

### <a name="manage-on-premises-systems-remote-systems-and-systems-without-ui-with-windows-admin-center"></a>使用 Windows Admin Center 管理管理內部部署系統、遠端系統以及沒有 UI 的系統
[Windows Admin Center](../manage/windows-admin-center/overview.md) 是一個瀏覽器型管理應用程式，可在沒有任何 Azure 或雲端相依性的情況下用來進行 Windows Server 的內部部署管理。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，對於管理未連線至網際網路的私人網路特別有用。 您可以在 Windows 10、閘道伺服器上，或直接在您想要管理的 Windows Server 系統上安裝 Windows Admin Center。

>[!NOTE]
>Windows Admin Center 是我們用來稱呼「Project Honolulu」的正式名稱。

### <a name="manage-on-premises-systems-with-server-manager"></a>使用伺服器管理員管理內部部署系統
[伺服器管理員](server-manager/server-manager.md)是 Windows Server 完整安裝隨附的管理主控台。 (這不適用於沒有 UI 的安裝 - Server Core 不包含伺服器管理員)。使用伺服器管理員來安裝和移除伺服器角色、新增和移除遠端伺服器、啟動和停止服務，以及檢視有關環境的蒐集資料。

### <a name="manage-remote-systems-and-systems-without-ui-with-remote-server-administration-tools-rsat"></a>使用遠端伺服器管理工具 (RSAT) 管理遠端系統和不顯示 UI 的系統
如果您的環境包含安裝 Server Core 或遠端伺服器 (內部部署或虛擬機器)，您可以使用[遠端伺服器管理工具 (RSAT)](../remote/remote-server-administration-tools.md) 來管理這些系統。 RSAT 包含伺服器管理員，因此您可以使用它來管理所有伺服器。

> [!IMPORTANT]
> RSAT 在 Windows 10 上執行。 您無法在 Windows Server Core 安裝 RSAT。

您也可以從命令列管理 Server Core 安裝。 請參閱[Server Core 中的基本管理](server-core/server-core-administer.md)工作。

### <a name="manage-updates-to-windows-server-systems"></a>管理 Windows Server 系統更新
您可以使用 [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) 來管理和部署 Windows Server 環境中的系統更新。

## <a name="gather-information-about-your-environment"></a>收集有關環境的資訊
您以系統管理員身分所做的許多決定，都取決於您環境中的系統和使用者相關資料。 請使用下列資訊及工具來收集這些資料。

從[設定您組織中的 Windows 診斷資料](/windows/configuration/configure-windows-diagnostic-data-in-your-organization)開始，了解可從 Windows 10 與 Windows Server 收集的診斷資料相關資訊。

### <a name="setup-and-boot-event-collection"></a>[安裝並啟動事件收集](get-started-with-setup-and-boot-event-collection.md)
「安裝與開機事件集合」可讓您指定「收集者」電腦，這部電腦會收集其他電腦開機或進行設定程序時所發生的各種重要事件。 您隨後可以使用事件檢視器、訊息分析器、Wevtutil 或 Windows PowerShell Cmdlet 來分析收集到的事件。

### <a name="software-inventory-logging-sil"></a>[軟體清查記錄 (SIL)](software-inventory-logging/get-started-with-software-inventory-logging.md)

Windows Server 中的軟體清查記錄功能有一組簡單的 PowerShell Cmdlet，可協助伺服器系統管理員擷取伺服器上安裝的 Microsoft 軟體清單。 它也提供功能，定期收集並透過網路，使用 HTTPS 通訊協定將此資料轉送到目標 Web 伺服器，以便進行彙總。 管理功能 (主要是每小時的收集和轉送) 也是使用 PowerShell 命令完成。

### <a name="user-access-logging-ual"></a>[使用者存取記錄 (UAL)](user-access-logging/get-started-with-user-access-logging.md)

「使用者存取記錄」會彙總執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 之電腦上記錄的唯一用戶端裝置和使用者要求事件。 然後，這些記錄可供使用 (透過伺服器管理員的查詢)，依伺服器角色、使用者、裝置、本機伺服器以及日期來抓取數量和執行個體。 不僅如此，UAL 也能讓非 Microsoft 軟體開發人員檢測要彙整的 UAL 事件。

## <a name="tune-your-windows-server-environment-for-performance"></a>調整 Windows Server 環境的效能
使用下列資訊可協助調整您環境的效能。

### <a name="performance-tuning-guidelines"></a>[效能調整指導方針](performance-tuning/index.md)
檢視一組可用來調整 Windows Server 2016 中的伺服器設定的指導方針，並取得增量效能或提高能源效率，尤其是當工作負載的本質隨著時間變化很小時。

### <a name="microsoft-server-performance-advisor"></a>[Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

使用 Microsoft Server Performance Advisor (SPA)，您可以悄悄地收集 Windows Server 上的計量來診斷效能問題，無須新增軟體代理程式或重新設定實際執行伺服器。 SPA 會產生完整的效能報告和歷程圖，並提供建議。


## <a name="automate-windows-server-management"></a>自動化 Windows Server 管理

Windows Server 包含一組命令和 Windows PowerShell 模組，可用來自動化管理工作。

### <a name="windows-powershell"></a>[Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Windows PowerShell 是命令列殼層和指令碼語言，專為快速自動化管理工作所設計。

### <a name="windows-commands"></a>[Windows 命令](windows-commands/windows-commands.md)

Windows 命令列工具是用來在 Windows 中執行系統管理工作。 您可以利用命令參考來自行熟悉命令列工具、深入了解命令殼層，以及使用批次檔案或指令碼工具來自動化命令列工作。

## <a name="windows-server-insider-preview"></a>Windows Server Insider Preview
### <a name="system-insights"></a>[系統深入解析](../manage/system-insights/overview.md)
系統深入解析是本身會將預測性分析導入 Windows Server 的新功能。 這些預測性功能會在本機分析 Windows Server 系統資料 (例如效能計數器或 ETW 事件)，主動協助 IT 系統管理員偵測並解決其部署中有問題的行為。