---
title: Management
description: 了解管理 Windows Server 的相關工具、建議和指導方針
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: e6a5357e3e33b3d3318a3e281bbb5c80be842155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890509"
---
# <a name="management"></a>Management


>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<hr />

在您的環境中部署 Windows Server，包括您所需特性和功能的角色後，接下來就是管理這些伺服器。 Windows Server 包含數個工具，可協助您了解您的 Windows Server 環境、管理特定伺服器、微調效能，以及最終自動化許多管理工作。 

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

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>管理 Windows Server 系統和環境</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">管理內部部署系統、 遠端系統，以及系統，而不使用 Windows Admin Center UI</a></h3>瀏覽器型管理應用程式，可在沒有任何 Azure 或雲端相依性的情況下用來進行 Windows Server 的內部部署管理。 Windows Admin Center (先前稱為「Project Honolulu」) 可讓您完整控制伺服器基礎結構的各個層面，這在未連線至網際網路的私人網路上管理伺服器特別有用。 您可以在 Windows 10、閘道伺服器上，或直接在您想要管理的 Windows Server 系統上安裝 Windows Admin Center。</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">管理內部部署系統與伺服器管理員</a></h3>Windows Server 完整安裝隨附的管理主控台。 （它不適用於沒有 UI 的安裝-Server Core 不包含伺服器管理員）。使用伺服器管理員來安裝和移除伺服器角色新增和移除遠端伺服器、 開始和停止服務，以及檢視資料收集關於您的環境。</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">管理遠端系統並不顯示 UI 與遠端伺服器管理工具 (RSAT) 的系統</a></h3>如果您的環境包含安裝 Server Core 或遠端伺服器 (內部部署或虛擬機器)，您可以使用 RSAT 來管理這些系統。 RSAT 包含伺服器管理員，因此您可以使用它來管理所有伺服器。 請注意，RSAT 在 Windows 10 上執行。 您無法在 Windows Server Core 安裝 RSAT。 您也可以從命令列管理 Server Core 安裝。 請參閱<a href="server-core/server-core-administer.md">在 Server Core 的基本管理工作</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">管理 Windows Server 系統的更新</a></h3>使用 Windows Server Update Services (WSUS) 來管理和部署 Windows Server 環境中的系統更新。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>收集有關環境的資訊</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">安裝與開機事件集合</a></h3>「安裝與開機事件集合」可讓您指定「收集者」電腦，這部電腦會收集其他電腦開機或進行設定程序時所發生的各種重要事件。 您隨後可以使用事件檢視器、訊息分析器、Wevtutil 或 Windows PowerShell Cmdlet 來分析收集到的事件。 </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">軟體清查記錄 (SIL)</a></h3>Windows Server 中的軟體清查記錄功能有一組簡單的 PowerShell Cmdlet，可協助伺服器系統管理員擷取伺服器上安裝的 Microsoft 軟體清單。 它也提供功能，定期收集並透過網路，使用 HTTPS 通訊協定將此資料轉送到目標 Web 伺服器，以便進行彙總。 管理功能 (主要是每小時的收集和轉送) 也是使用 PowerShell 命令完成。</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">使用者存取記錄 (UAL)</a></h3>「使用者存取記錄」會彙總執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 之電腦上記錄的唯一用戶端裝置和使用者要求事件。 然後，這些記錄可供使用 (透過伺服器管理員的查詢)，依伺服器角色、使用者、裝置、本機伺服器以及日期來抓取數量和執行個體。 不僅如此，UAL 也能讓非 Microsoft 軟體開發人員檢測要彙整的 UAL 事件。 </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>調整 Windows Server 環境的效能</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">效能微調指導方針</a></h3>檢視一組可用來調整 Windows Server 中的伺服器設定的指導方針，並取得增量效能或提高能源效率，尤其是當工作負載的本質隨著時間變化很小時。</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>使用 Microsoft Server Performance Advisor (SPA)，您可以悄悄地收集 Windows Server 上的計量來診斷效能問題，無須新增軟體代理程式或重新設定實際執行伺服器。 SPA 會產生完整的效能報告和歷程圖，並提供建議。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>自動化 Windows Server 管理</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>Windows PowerShell 是命令列殼層和指令碼語言，專為快速自動化管理工作所設計。 </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Windows 命令</a></h3>Windows 命令列工具是用來在 Windows 中執行系統管理工作。 您可以利用命令參考來自行熟悉命令列工具、深入了解命令殼層，以及使用批次檔案或指令碼工具來自動化命令列工作。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>自動化 Windows Server 管理</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">系統的深入解析</h3></a>原生的預測性分析在本機上分析 Windows Server 系統資料，例如效能計數器和 ETW 事件，協助 IT 系統管理員主動偵測並解決在部署中有問題的行為。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>