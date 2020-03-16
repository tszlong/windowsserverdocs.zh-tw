---
title: Windows Admin Center 概觀
description: 了解如何使用 Windows Admin Center 管理 Windows Server (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 01/07/2020
ms.localizationpriority: high
ms.prod: windows-server
ms.openlocfilehash: bb2f6d7fcbf18ef9bc67534982d1a98fdc5172a1
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79320032"
---
# <a name="windows-admin-center"></a>Windows Admin Center

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 是以瀏覽器為基礎在本機部署的應用程式，用於管理 Windows 伺服器、叢集、超融合式基礎結構以及 Windows 10 電腦。 除了 Windows 本身以外，不需另付費用，而且可以立即使用於生產環境中。

若要瞭解最新功能，請參閱＜[發行歷程記錄](support/release-history.md)＞。

## <a name="download-now"></a>立即下載

請從 Microsoft 評估中心下載 [Windows Admin Center](https://www.microsoft.com/evalcenter/evaluate-windows-admin-center)。  雖然其會顯示「開始評估」，但這是正式運作的生產環境使用版本，內含為 Windows 或 Windows Server 授權的一部分。

如需安裝的說明，請參閱＜[安裝](deploy/install.md)＞。 如需開始使用 Windows Admin Center 的提示，請參閱＜[快速入門](use/get-started.md)＞。

您可以使用 Microsoft Update 或以手動下載並安裝 Windows Admin Center 的方式更新非預覽版的 Windows Admin Center。 每個非預覽版的 Windows Admin Center 在下一個非預覽版發行後，仍可支援 30 天。 如需詳細資訊，請參閱我們的[支援原則](support/index.md)。

## <a name="windows-admin-center-scenarios"></a>Windows Admin Center 案例

您可以將 Windows Admin Center 用於以下事項：

|     |     |
| --- | --- |
| ![](media/simple-icon.png)| **簡化伺服器管理** <br/> 使用現代化版本的熟悉工具 (例如伺服器管理員) 來管理您的伺服器和叢集。 不到五分鐘即可安裝完畢，並可立即管理您環境中的伺服器，而不需要其他設定。 如需詳細資訊，請參閱＜[什麼是 Windows Admin Center？](understand/what-is.md)＞。 |
| ![](media/future-icon.png)| **運用混合式解決方案** <br/> 與 Azure 整合可協助您選擇性地將內部部署伺服器與相關的雲端服務連接。 如需詳細資訊，請參閱＜[Azure 混合式服務](azure/index.md)＞ |
| ![](media/secure-icon.png)| **簡化超融合式管理** <br/> 簡化 Azure Stack HCI 或 Windows Server 超融合式叢集的管理。 使用簡化的工作負載來建立和管理 VM、儲存空間直接存取磁碟區、軟體定義的網路功能等等。 如需詳細資訊，請參閱＜[使用 Windows Admin Center 來管理超融合式基礎結構](use/manage-hyper-converged.md)＞|

這裡有一段影片可讓您大致瞭解，接著是提供更多詳細資料的海報：
>[!VIDEO https://www.youtube.com/embed/WCWxAp27ERk]

[![Windows Admin Center 海報](media/WAC1910Poster_thumb_small.PNG)](media/WAC1910Poster_thumb.png)

[下載 PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1910Poster.pdf)


## <a name="contents-at-a-glance"></a>內容簡介

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>了解</h3>
            <ul>
            <li><a href="understand/what-is.md">什麼是 Windows Admin Center？</a>
            <li><a href="understand/faq.md">常見問題集</a>
            <li><a href="understand/case-studies.md">案例研究</a>
            <li><a href="understand/related-management.md">相關管理產品</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>規劃</h3>
            <ul>
            <li><a href="plan/installation-options.md">什麼類型的安裝最適合您？</a>
            <li><a href="plan/user-access-options.md">使用者存取選項</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>部署</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">準備您的環境</a>
            <li><a href="deploy/install.md">安裝 Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">啟用高可用性</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>設定</h3>
            <ul>
            <li><a href="configure/settings.md">Windows Admin Center 設定</a>
            <li><a href="configure/user-access-control.md">使用者存取控制與權限</a>
            <li><a href="configure/shared-connections.md">共用的連線</a>
            <li><a href="configure/using-extensions.md">擴充</a>
            <li><a href="configure/use-powershell.md">使用 PowerShell 進行自動化</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>用法</h3>
            <ul>
            <li><a href="use/get-started.md">啟動及新增連線</a>
            <li><a href="use/manage-servers.md">管理伺服器</a>
            <li><a href="use/deploy-hyperconverged-infrastructure.md">部署超融合式基礎結構</a>
            <li><a href="use/manage-hyper-converged.md">管理超融合式基礎結構</a>
            <li><a href="use/manage-failover-clusters.md">管理容錯移轉叢集</a>
            <li><a href="use/manage-virtual-machines.md">管理虛擬機器</a>
            <li><a href="use/logging.md">記錄</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>連線到 Azure</h3>
            <ul>
            <li><a href="azure/index.md">Azure 混合式服務</a></li>
            <li><a href="azure/azure-integration.md">將 Windows Admin Center 連線到 Azure </a></li>
            <li><a href="azure/deploy-wac-in-azure.md">在 Azure 中部署 Windows Admin Center</a></li>
            <li><a href="azure/manage-azure-vms.md">使用 Windows Admin Center 管理 Azure VM</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>支援</h3>
            <ul>
            <li><a href="support/release-history.md">發行大事記</a>
            <li><a href="support/index.md">支援原則</a>
            <li><a href="support/troubleshooting.md">一般疑難排解步驟</a>
            <li><a href="support/known-issues.md">已知問題</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>Extend</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">擴充功能概觀</a>
            <li><a href="extend/understand-extensions.md">了解擴充功能</a>
            <li><a href="extend/developing-extensions.md">開發擴充功能</a>
            <li><a href="extend/publish-extensions.md">指南</a>
            <li><a href="extend/publish-extensions.md">發佈擴充功能</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="video-based-learning"></a>以影片為基礎的學習

以下是來自 Microsoft Ignite 2019 研討會的一些影片：

- [Windows Admin Center：發揮 Azure 混合式的價值](https://aka.ms/WAC-BRK3165)
- [Windows Admin Center：新功能與新趨勢](https://aka.ms/WAC-BRK2048)
- [使用 Windows Admin Center 從 Azure 自動監視、保護及更新您的內部部署伺服器](https://aka.ms/WAC-THR2146)
- [透過 Windows Admin Center 協力廠商擴充功能來完成更多工作](https://aka.ms/WAC-THR2140)
- [成為 Windows Admin Center 專家：部署、設定和安全性的最佳作法](https://aka.ms/WAC-THR2135)
- [Windows Admin Center：與 System Center 和 Microsoft Azure 搭配使用的效果更佳](https://aka.ms/WAC-THR2176)
- [如何將 Windows Admin Center 和 Windows Server 與 Microsoft Azure 混合式服務搭配使用](https://aka.ms/WAC-THR2073)
- [即時問與答：使用 Windows Admin Center 管理您的混合式伺服器環境](https://aka.ms/WAC-MLS1055)
- [學習路徑：混合式管理技術](https://aka.ms/WAC-HybridMgmtTech)
- [實習實驗室：Windows Admin Center 和混合式部署](https://aka.ms/WAC-HOL2019)

以下是來自 Windows Server Summit 2019 研討會的一些影片：

- [使用 Windows Admin Center 邁向混合](https://aka.ms/WAC-WSS2019-GoHybridWAC)
- [Windows Admin Center v1904 的最新動向](https://aka.ms/WAC-WSS2019-WhatsNewv1904)

以下是一些其他資源：

- [Windows Admin Center 伺服器管理重新構思](https://aka.ms/WAC-ServerMgmtReimagined)
- [使用 Windows Admin Center 隨處管理伺服器和虛擬機器](https://aka.ms/WAC-Webinar2019)
- [如何開始使用 Windows Admin Center](https://www.youtube.com/embed/PcQj6ZklmK0)

## <a name="see-how-customers-are-benefitting-from-windows-admin-center"></a>了解客戶如何從 Windows Admin Center 中受益

|     |
| --- |
| 「[Windows Admin Center] 在管理我們的管理系統上，已減少我們 75% 以上的時間/精力。」<br> *- Convergent Computing 總裁 Rand Morimoto* |
| 「多虧 [Windows Admin Center] 的幫助，我們才可以透過 HTML5 入口網站從遠端管理客戶而不發生問題，並且歸功於多重要素驗證，我們能夠運用與 Azure Active Directory 的完全整合來提高安全性。」<br/> *- Inside Technologies 創立者暨資深顧問 Silvio Di Benedetto* |
| 「我們已經可以用更有效率的方式部署 [Server Core] SKU，改善資源效率、安全性及自動化，同時仍能達到相當高的生產力，並減少僅依賴指令碼時可能發生的錯誤。」 <br/> *- VaiSulWeb 創立者暨執行長 Guglielmo Mengora* |
| 「[Windows Admin Center] 推出後，特別是在 SMB 市場的客戶，現在已有簡單好用的工具可以來管理其內部基礎結構。 這不僅讓系統管理工作負荷減少至最低限度，同時也節省了大量時間。 更好的是，還不需任何其他授權費用，就能享有 [Windows Admin Center]！」 <br/> *- SecureGUARD 管理總監 Helmut Otto* |

[閱讀更多有關各家公司在其生產環境中使用 Windows Admin Center 的資訊。](understand/case-studies.md)

## <a name="related-products"></a>相關產品

Windows Admin Center 主要用於管理單一伺服器或叢集。 它補充但不取代現有的 Microsoft 監視與管理解決方案，例如遠端伺服器管理工具 (RSAT)、System Center、Intune 或 Azure Stack。

[了解 Windows Admin Center 如何與其他 Microsoft 管理解決方案相輔相成。](understand/related-management.md)

## <a name="stay-updated"></a>持續更新

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[在 Twitter 上關注我們](https://twitter.com/servermgmt)

![](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[閱讀我們的部落格](https://blogs.technet.microsoft.com/servermanagement/)
