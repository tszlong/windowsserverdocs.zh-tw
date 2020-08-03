---
title: Windows Server 版本 1803 中的新功能
description: 關於運算、身分識別、管理、自動化、網路功能、安全性、存放裝置的新功能。
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.date: 05/07/2018
ms.openlocfilehash: 32410c9b2645d96ee1190afdda6a2ce75f09feef
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409558"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Windows Server 版本 1803 中的新功能

> 適用於：Windows Server (半年通道)

<img src=../media/landing-icons/new.png style='float:left; padding:.5em;' alt=Icon showing a newspaper>&nbsp;若要深入了解 Windows 的最新功能，請參閱 [Windows Server 的新功能](whats-new-in-windows-server.md)。 本節內容說明 Windows Server 版本 1803 的新功能和變更。 此處所列的新功能和變更是您使用這個版本時最可能帶來最大影響的新功能和變更。 另請參閱 [Windows Server 半年通道更新](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/)。

## <a name="windows-admin-center"></a>Windows Admin Center

Project Honolulu 現稱為 **Windows Admin Center**。
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

[Windows Admin Center](../manage/windows-admin-center/overview.md) 將本機與遠端伺服器的各個管理層面結合在一起。 Windows Admin Center 是以瀏覽器為基礎部署在本機且不需要網際網路連線的管理體驗，讓您完全控制 Windows Server 部署的所有層面。

## <a name="windows-server-release-strategy"></a>Windows Server 發行策略

Windows Server 版本 1709 已於 2017 年 9 月做為半年通道中的第一個版本發行。 半每年通道採用較快的發行頻率，並處理來自那些要每隔幾個月就快速創新的使用者的意見反應。 這與發行頻率為每隔 2-3 年一次的長期維護通道形成互補。

根據遙測及意見反應，這些通道已證明其適切符合下列一般策略：
- 半每年通道非常適合現代應用程式與創新案例，例如容器和微型服務。
- 長期維護通道則是軟體定義資料中心和超融合式基礎結構 (HCI) 等核心基礎結構案例的首選發行版本。

半年通道和長期維護通道的相關特定案例，如下：

| 說明 | 長期維護管道 | 半年通道 |
|--|--|--|
| 建議的案例 | 一般用途檔案伺服器、第一方和第三方工作負載、傳統應用程式、基礎結構角色、軟體定義資料中心以及超融合式基礎結構 | 容器化應用程式、容器主機，以及因加快創新而受益的應用程式案例 |
| 最新發行 | 每隔 2-3 年一次 | 每隔 6 個月一次 |
| 支援 | 5 年主要支援 + 5 年延伸支援 | 18 個月 |
| 版本 | 所有可用的 Windows Server 版本 | Standard 和 Datacenter Edition |
| 誰可以使用 | 透過所有通道更新的所有客戶 | 僅限軟體保證與雲端客戶 |
| 安裝選項 | Server Core 以及具備桌面體驗的伺服器 | 適用於容器主機和容器映像以及 Nano 伺服器容器映像的 Server Core |

## <a name="application-platform-and-containers"></a>應用程式平台和容器

- Optimization
    - Server Core 基本容器映像從 Windows Server 版本 1709 開始已縮減 30%。
    - 應用程式相容性也已改善，可協助進行傳統應用程式容器化。
    - 歸功於各種修正和最佳化，容器開機效能及執行時間效能已改善。
- 容器的網路功能：已新增 Localhost 和 http Proxy 支援，並改善容器延展性和啟動時間。
- 工具：已增強對 Curl.exe、Tar.exe 和 SSH 的支援來補足 PowerShell 運用於建置和偵錯案例的功能。

### <a name="server-core-container-image"></a>Server Core 容器映像

現已推出應用程式相容性更佳的較小型 Server Core 容器。 詳細資訊可在[這裡](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization)找到。

- 未使用的選用功能及角色已移除。 如需詳細資訊，請參閱[不在 Server Core 容器中的角色、角色服務和功能](../administration/server-core/server-core-container-removed-roles.md)。
    - 下載大小已降至 1.58 GB，比 Windows Server 版本 1709 縮減 30%。
    - 佔磁碟的大小已降至 3.61 GB，比 Windows Server 版本 1709 縮減 20%。
- Nano 伺服器容器映像在 100MB 以下

### <a name="windows-subsystem-for-linux-wsl"></a>適用於 Linux 的 Windows 子系統 (WSL)

WSL 可讓伺服器系統管理員從 Windows Server 上的 Linux 使用現有工具及指令碼。 許多在[命令列部落格](https://devblogs.microsoft.com/commandline/tag/wsl/)中展示的改進功能現在已是 Windows Server 中的一部分，包括背景工作、DriveFS、WSLPath 等等。

### <a name="kubernetes"></a>Kubernetes

Kubernetes (通常稱為 K8s) 是在[雲端原生運算基金會 (Cloud Native Computing Foundation)](https://www.cncf.io) 管理下所開發適用於自動化部署、延展和管理容器化應用程式的開放原始碼系統。

在 Windows Server 版本 1709 中，使用者已經能充分利用 Windows 上的 Kubernetes 功能，包括：
- 共用 Pod 區間：基礎架構和背景工作角色 Pod 現在共用網路區間 (類似於 Linux 命名空間)。
- 端點最佳化：由於區間共用，容器服務現在需要追蹤的端點數量是以前的至少一半。
- 資料路徑最佳化：改進虛擬篩選平台和主機網路服務，允許核心型負載平衡。

隨著 Windows Server 版本 1803 的發行，未來的 Kubernetes 版本中將會提供更多功能：
- 由 Kubernetes 所協調適用於 Windows 容器的[儲存空間外掛程式](https://github.com/Microsoft/K8s-Storage-Plugins)。
- 透過如我們與 [Tigera 在 Project Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/) 支援上的合作關係等計劃所佈建的雲端規模網路。
- Windows 平台對於每個容器有多個 Pod 的 Hyper-V 隔離 Pod 的支援。

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>應用程式相容性及功能同位問題已修正

- Microsoft Message Queuing (MSMQ) 現在會安裝在 Server Core 容器中。
- 中斷 ASP.net 效能計數器的問題已修正。
- 執行於容器之服務未收到關機通知的問題已修正。
    - 具體來說，對 Server Core 及 Nano 伺服器容器映像的通知都已變更為 CTRL_SHUTDOWN_EVENT。 此外，這還會延伸 Server Core 容器映像中的通知來影響所有在容器中執行的處理程序，包括傳送服務關閉通知至容器中執行的服務。
- docker pull 及 docker load，與判斷固定資料磁碟機是否必須有 BitLocker 保護才可寫入 (FDVDenyWriteAccess) 之原則設定的相容性問題已修正。

## <a name="storage"></a>存放裝置

在此版本中，可以防止檔案伺服器資源管理員服務於其啟動時在所有磁碟區上建立變更日誌 (也稱為 USN 日誌)。 這可以節省每個磁碟區的空間，但會停用即時檔案分類。 如需詳細資訊，請參閱[檔案伺服器資源管理員概觀](../storage/fsrm/fsrm-overview.md)。

## <a name="features-added-to-server-core"></a>Server Core 已新增功能

已將 Windows 部署服務 (WDS) 角色中的傳輸伺服器角色新增至 Server Core。

傳輸伺服器僅包含 WDS 的核心網路組件。 您可以使用傳輸伺服器來建立可從獨立伺服器傳輸資料 (包括作業系統映像) 的多點傳送命名空間。 如果您想要有可讓用戶端進行 PXE 開機並自行下載自訂安裝應用程式的 PXE 伺服器，也可以使用該功能。 如果您想要使用其中一種案例，則應該使用此選項。

您可以使用下列 Windows PowerShell 命令，在 Server Core 上啟用傳輸伺服器服務：

```
Install-WindowsFeature -Name WDS
```

## <a name="additional-references"></a>其他參考資料

[Windows Server 版本資訊](./windows-server-release-info.md)<br>
[Windows 10 (版本 1803) 新功能 IT 專業人員內容](/windows/whats-new/whats-new-windows-10-version-1803)
