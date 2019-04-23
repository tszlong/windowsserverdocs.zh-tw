---
title: Windows Admin Center 概觀
description: 了解如何使用 Windows Admin Center 管理 Windows Server (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: e23d02fd8da96392f44874ad6ea4d6c40e2be940
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828199"
---
# <a name="windows-admin-center"></a>Windows Admin Center

>適用於：Windows Admin Center，Windows Admin Center 預覽

歡迎使用 Windows Admin Center！

**Windows Admin Center** (代號**專案檀香山**) 已進化版的 Windows Server 內建管理工具; 它是窗口可合併本機和遠端伺服器管理的各個層面。 因為是在本機部署、以瀏覽器為基礎的管理體驗，並不需要網際網路連線和 Azure。 Windows Admin Center 讓您完全控制您的部署的各個層面，包括未連線至網際網路的私人網路。

## <a name="introduction"></a>簡介

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Windows Admin Center 資訊圖表](media/WAC1809Poster_thumb.PNG)

[下載 PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>快速入門

您在數分鐘內就可以讓 Windows Admin Center 在環境中啟動並執行：

1. [下載](https://aka.ms/windowsadmincenter)
2. [安裝](deploy/install.md)
3. [入門](use/get-started.md)

## <a name="contents-at-a-glance"></a>一眼的內容

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>了解</h3>
            <ul>
            <li><a href="understand/what-is.md">什麼是 Windows Admin Center？</a>
            <li><a href="understand/faq.md">常見問題集</a>
            <li><a href="understand/case-studies.md">案例研究</a>
            <li><a href="understand/related-management.md">相關的管理產品</a>
            <li><a href="understand/videos.md">影片</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>規劃</h3>
            <ul>
            <li><a href="plan/installation-options.md">安裝類型是最適合您？</a>
            <li><a href="plan/user-access-options.md">使用者存取選項</a>
            <li><a href="plan/azure-integration-options.md">有哪些 Azure 整合選項？</a>
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
            <li><a href="configure/user-access-control.md">使用者存取控制及權限</a>
            <li><a href="configure/using-extensions.md">延伸模組</a>
            <li><a href="configure/azure-integration.md">與 Azure 相整合</a>
            <li><a href="configure/manage-azure-vms.md">管理 Windows Admin Center 使用的 Azure Vm</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>用法</h3>
            <ul>
            <li><a href="use/get-started.md">啟動及新增連線</a>
            <li><a href="use/manage-servers.md">管理伺服器</a>
            <li><a href="use/manage-hyper-converged.md">管理超交集基礎結構</a>
            <li><a href="use/manage-failover-clusters.md">管理容錯移轉叢集</a>
            <li><a href="use/manage-virtual-machines.md">管理虛擬機器</a>
            <li><a href="use/azure-services.md">運用 Azure 服務</a>
            <li><a href="use/troubleshooting.md">一般疑難排解步驟</a>
            <li><a href="use/logging.md">記錄</a>
            <li><a href="use/known-issues.md">已知問題</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>擴充</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">擴充功能的概觀</a>
            <li><a href="extend/understand-extensions.md">了解擴充功能</a>
            <li><a href="extend/developing-extensions.md">開發延伸模組</a>
            <li><a href="extend/publish-extensions.md">輔助線</a>
            <li><a href="extend/publish-extensions.md">發行擴充功能</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>版本歷程記錄

了解我們最新發行的功能：

- 版本 [1902] (https://aka.ms/wac1902)加入軟體定義網路 (SDN) 管理，包括新的 SDN 工具來管理 Acl、 閘道連線，以及邏輯網路上的共用的連線清單與改善。
- 版本 [1812](https://aka.ms/wac1812) 新增深色佈景主題 (預覽版)、電源組態設定、BMC 資訊和 PowerShell 支援，以管理[擴充功能](./configure/using-extensions.md#manage-extensions-with-powershell)和[連線](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags)。
- 版本 [1809.5](https://aka.ms/wac1809.5) 是正式運作的累積更新，包含整個平台的各種品質與功能改進和錯誤修正，以及超融合式基礎結構管理解決方案中的幾個新功能。
- 版本 [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) 是正式運作的版本，將先前預覽版中的功能引入正式運作的通道。
- 版本 [1808](https://aka.ms/WACPreview1808-InsiderBlog) 新增 [已安裝的應用程式] 工具、許多隱藏版改進功能，以及預覽版 SDK 的重大更新。
- 版本 [1807](https://aka.ms/WACPreview1807-InsiderBlog) 新增簡化的 Azure 連線體驗、VM 詳細目錄頁面改進、檔案共用、Azure 更新管理整合等功能。 
- 版本 [1806](https://aka.ms/WACPreview1806-InsiderBlog) 新增顯示 PowerShell 指令碼、SDN 管理、2008 R2 連線、SDN、排程工作，以及許多其他改進功能。
- 版本 1804.25 - 維護更新以支援使用者將 Windows Admin Center 安裝於完全離線環境。
- 版本 [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/) - Project Honolulu 變成 Windows Admin Center，並新增安全性功能和角色型存取控制。 我們第一個 GA 版本。
- 版本 [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) 新增對 Azure AD 存取控制、詳細記錄、可調整大小內容的支援，以及許多工具改進功能。
- 版本 [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) 新增對協助工具、當地語系化、高可用性部署、標記、Hyper-V 主機設定及閘道驗證的支援。
- 版本 [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) 新增更多在工具各方面的虛擬機器功能和效能增強功能。
- 版本 [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) 新增備受期待的工具 (遠端桌面和 PowerShell) 以及其他改進功能。
- 版本 [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) 開始以我們第一個公開預覽版本的形式推出。

## <a name="stay-updated"></a>掌握最新資訊

<a target="_blank" class="mscom-link twitter-follow-link" title="在 Twitter 上關注我們" aria-label="Follow us on Twitter" data-info="Twitter" href="https://twitter.com/servermgmt"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" alt="Follow us on Twitter" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR"></picture></a>
 | 
<a target="_blank" class="mscom-link blogs-follow-link" title="閱讀我們的部落格" aria-label="Visit our Blogs" data-info="Blogs" href="https://blogs.technet.microsoft.com/servermanagement/"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" alt="Follow us on Blogs" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw"></picture></a>
