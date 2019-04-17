---
title: Windows Admin Center 常見問題集
description: 取得有關 Windows Admin Center 的問題解答 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 04/12/2019
ms.prod: windows-server-threshold
ms.openlocfilehash: 2f1591a32147e3c11ba635f1a11d36b7f38a3470
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296740"
---
# Windows Admin Center 常見問題

>適用於：Windows Admin Center、Windows Admin Center 預覽版

以下是有關 Windows Admin Center 最常見問題的解答。

## 什麼是 Windows Admin Center？

Windows Admin Center 是輕量的瀏覽器型 GUI 平台與工具組，供 IT 系統管理員用來管理 Windows Server 和 Windows 10。 這是熟悉的 [伺服器管理員] 和 Microsoft Management Console (MMC) 等系統隨附管理工具逐漸發展成現代簡化整合式資訊安全體驗的演進。

## 我可以在生產環境中使用 Windows Admin Center 嗎？

是。 Windows Admin Center 已正式運作，並且已準備就緒可供廣泛使用和生產部署。 目前的平台功能及核心工具符合 Microsoft 的標準發行準則，以及我們的可用性、 可靠性、 效能、 協助工具、 安全性和採用率的品質列。

[!INCLUDE [support-policy](../includes/support-policy.md)]

## 使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。 您可以在 Windows Server 或 Windows 10 的有效授權下免費使用 Windows Admin Center (可供個別下載)，這已依據 Windows 增補 EULA 取得授權。

## 哪些版本的 Windows Server 可以使用 Windows Admin Center 來管理？

Windows Admin Center 最適合 Windows Server 2019，若要啟用 Windows Server 2019 版本中的索引鍵佈景主題： 是混合式雲端案例和超融合式基礎結構管理特別。 雖然 Windows Admin Center 最適合使用 Windows Server 2019，但也支援管理各種客戶已經使用的版本： Windows Server 2012 和更新版本是完全支援。 也是有限的功能來管理 Windows Server 2008 R2。

## Windows Admin Center 是否會完全取代所有傳統的隨附工具和 RSAT 工具？

否。 Windows Admin Center 可以管理許多常見的案例，但不會完全取代所有傳統 Microsoft Management Console (MMC) 工具。 如需詳細的查看哪些工具隨附於 Windows Admin Center，閱讀更多有關[管理伺服器](..\use\manage-servers.md)在我們的文件中。 Windows Admin Center 在其伺服器管理員解決方案中有下列重要功能：

* 顯示資源和資源使用率
* 憑證管理
* 管理裝置
* 事件檢視器
* 檔案總管
* 防火牆管理
* 管理已安裝的應用程式
* 設定本機使用者和群組
* 網路設定
* 檢視/結束處理序，以及建立處理序傾印
* 登錄編輯
* 管理已排程工作
* 管理 Windows 服務
* 啟用/停用角色和功能
* 管理 Hyper-V VM 和虛擬交換器
* 管理儲存空間
* 管理儲存體複本
* 管理 Windows 更新
* PowerShell 主控台
* 遠端桌面連線

Windows Admin Center 還提供下列解決方案：

* 電腦管理 – 提供管理 Windows 10 用戶端電腦所需的伺服器管理員功能子集
* 容錯移轉叢集管理員 – 提供持續管理容錯移轉叢集和叢集資源的支援
* 超融合式叢集管理員 – 提供專為儲存空間直接存取及 Hyper-V 量身訂作的全新體驗。 它具有儀表板，並強調監控所用的圖表和警示。

Windows Admin Center 與 RSAT (遠端伺服器管理工具) 相輔相成，但不會加以取代，因為 Active Directory、DHCP、DNS、IIS 等角色還沒有 Windows Admin Center 中所提供的對等管理功能。

## 是否可以使用 Windows Admin Center 來管理免費的 Microsoft Hyper-V Server？

是。 Windows Admin Center 可以用來管理 Microsoft Hyper-V Server 2016 和 Microsoft Hyper-V Server 2012 R2。

## 是否可以在 Windows 10 電腦上部署 Windows Admin Center？

是的，可以將 Windows Admin Center 安裝在執行於桌面模式中的 Windows 10 (版本 1709 或更新版本)。  Windows Admin Center 也可以使用 Windows Server 2016 的伺服器上安裝或更大閘道模式中，然後透過從 Windows 10 電腦的網頁瀏覽器進行存取。 [深入了解安裝選項](..\plan\installation-options.md)。

## 我聽過的 Windows Admin Center 會使用 PowerShell 進一步來說，可以請參閱它所使用的實際指令碼？

是 ！ [Showscript 功能](..\use\get-started.md#view-powershell-scripts-used-in-windows-admin-center)已在 Windows Admin Center 預覽版 1806年中新增和現已包含在引進 GA 通道。

## Windows Admin Center 是否有任何方案可以管理 Windows Server 2008 R2 或更早版本？

Windows Admin Center 現已支援**有限**的功能來管理 Windows Server 2008 R2。 Windows Admin Center 需依賴 Windows Server 2008 R2 中所沒有的 PowerShell 功能和平台技術，使完整支援變得可行。 Windows Server 2008/2008 R2 即將於 2020 年 1 月終止支援，因此 Microsoft 建議客戶[移轉至 Azure 或升級至最新版的 Windows Server](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008)。

## 是否有任何要讓 Windows Admin Center 管理 Linux 連線的計劃？

我們因為客戶要求而正在調查，但目前沒有沒有鎖定的計畫来提供，且支援可能主控台連線的只包含透過 SSH。

## Windows Admin Center 支援哪些網頁瀏覽器？

最新版本的 Microsoft Edge (Windows 10 版本 1709 或更新版本) 以及 Google Chrome 瀏覽器已在 Windows 10 上測試過並且受支援。 [檢視瀏覽器特定的已知的問題](..\support\known-issues.md#browser-specific-issues)。 其他新式網頁瀏覽器或其他平台目前未納入我們的測試矩陣，因此沒有*正式*支援。

## Windows Admin Center 如何處理安全性？

從瀏覽器至 Windows Admin Center 閘道的流量使用 HTTPS。 從閘道至受管理伺服器的流量是透過 WinRM 的標準 PowerShell 和 WMI。 我們支援 LAPS (區域系統管理員密碼解決方案)、資源型限制委派、閘道存取控制 (使用 AD 或 Azure AD) 以及角色型存取控制，以進行目標伺服器管理。

## Windows Admin Center 是否使用 CredSSP？

是的在幾個情況下 Windows Admin Center 需要 CredSSP。 這被需要將您的認證進行驗證傳遞至適用於管理您的目標的特定伺服器以外的電腦。 例如，如果您正在管理虛擬機器在**伺服器 B**，但想要儲存在裝載**伺服器 C**的檔案共用這些虛擬機器之 vhdx 檔案，Windows Admin Center 必須使用 CredSSP 向**伺服器 C**存取檔案共用。

Windows Admin Center 會處理 CredSSP 的設定後自動從您的同意提示。 設定之前 CredSSP，Windows Admin Center 會檢查以確定系統有最新的 CredSSP[更新](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。 雖然啟用 CredSSP 時，會有徽章伺服器概觀，以及它-停用的選項

![CredSSP 上伺服器概觀](../media/CredSSP-overview.png)

在下列區域中目前使用 CredSSP:

- 使用分離 SMB 儲存在虛擬機器工具 （上面的範例。）
- 使用更新工具中的容錯移轉 」 或 「 超融合式叢集管理解決方案，這會執行[叢集感知更新](https://docs.microsoft.com/windows-server/failover-clustering/cluster-aware-updating) 

## 是否有任何雲端相依性？

Windows Admin Center 不需要網際網路存取，也不需要 Microsoft Azure。 Windows Admin Center 會在任何位置管理 Windows Server 和 Windows 執行個體：在實體系統、在任何 Hypervisor 中的任何虛擬機器，或在任何雲端執行的。 雖然與各種 Azure 服務的整合將隨著時間陸續新增，但這些會是選用的加值功能，並不是使用 Windows Admin Center 的必要條件。

## 是否有任何其他相依性或必要條件？

可以將 Windows Admin Center 安裝在 Windows 10 秋季年度更新版 (1709) 或更新版本，或是 Windows Server 2016 或更新版本。 若要管理 Windows Server 2008 R2、2012 或 2012 R2，這些伺服器上必須有 Windows Management Framework 5.1 安裝。 沒有任何其他相依性。 不需要 IIS、不需要代理程式，也不需要 SQL Server。

## 擴充性和協力廠商支援呢？

Windows Admin Center 有可用的 SDK，讓任何人都可以撰寫自己的擴充功能。 做為一個平台，擴展生態系統和啟用協力廠商擴充性，自開始以來一直都是我們重要的優先考量。 [深入了解 Windows Admin Center SDK](..\extend\extensibility-overview.md)。

## 是否可以使用 Windows Admin Center 來管理超融合式基礎結構？

是。 Windows Admin Center 支援管理執行 Windows Server 2016 或 Windows Server 2019 的超融合式叢集。 Windows Admin Center 中的超融合式叢集管理員解決方案是先前預覽中，但現在已**正式運作**，有一些新功能預覽中。 如需詳細資訊，請[閱讀更多有關管理超融合式基礎結構的資訊](..\use\manage-hyper-converged.md)。

## Windows Admin Center 是否需要 System Center？

否。 Windows Admin Center 可與 System Center 互補，但 System Center 並非必要的。 [閱讀更多有關 Windows Admin Center 和 System Center](related-management.md#system-center)。

## Windows Admin Center 可以取代 System Center Virtual Machine Manager (SCVMM) 嗎？

Windows Admin Center 與 SCVMM 相輔相成；Windows Admin Center 主要用來取代傳統 Microsoft Management Console (MMC) 嵌入式管理單元和伺服器管理體驗。  Windows Admin Center 無意取代 SCVMM 監視層面的功能。 [閱讀更多有關 Windows Admin Center 和 System Center](related-management.md#system-center)。

## 什麼是 Windows Admin Center 預覽版？哪個版本適合我用？

有兩個 Windows Admin Center 版本可供下載：

### Windows Admin Center

* 如果您是 IT 系統管理員，但無法經常進行更新，或是需要較長時間驗證用於生產的版本時，此版本正合您需要。 我們目前的正式運作 (GA) 版本是 Windows Admin Center 1904。
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* 若要取得最新版本中，[從這裡下載](https://aka.ms/WACDownload)。

### Windows Admin Center 預覽版

>[!NOTE]
>目前的 GA 版本 (Windows Admin Center 1904) 包含所有先前的預覽功能。
>在 Insider Preview 將會在接下來幾個月中傳回。

* 如果您是 IT 系統管理員，想要依一般發行頻率取得最新的絕佳功能時，此版本正合您需要。 我們的目的是提供後續的更新版本的每個月或操作。 核心平台仍繼續為生產就緒產品，而授權會提供生產使用權。 但請注意，您將會看到清楚標示為 PREVIEW 且適合評估及測試的新工具和功能陸續推出。
* 若要取得最新的 Insider Preview 版本，已註冊的測試人員可能會直接從[Windows Server Insider Preview 下載頁面上](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)，[其他下載] 下拉式清單下載 Windows Admin Center 預覽版。 如果您還沒有註冊為測試人員，請參閱商務用 Windows 測試人員入口網站上的[開始使用 Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## 為什麼選擇「Windows Admin Center」做為「Project Honolulu」的最終名稱？

Windows Admin Center 是「Project Honolulu」的正式產品名稱，加強了我們在核心系統管理與管理案例廣泛範圍內對 IT 系統管理員整合式體驗的願景。 同時也昭示我們以客戶為導向、對 IT 系統管理員使用者需求的關注，視之為我們投資方式與提供項目的核心主軸。

## 何處可深入了解 Windows Admin Center，或取得更多有關上述主題的詳細資料？

我們的[啟動頁面](https://aka.ms/WindowsAdminCenter)是最適合的起點，其中有連結連到我們新近分類的文件內容、下載位置、意見反應提供方式、參考資訊等資源。

## 什麼是 Windows Admin Center 版本歷程記錄？

[檢視的版本歷程記錄。](..\overview.md#release-history)

## 我的 Windows Admin Center 發生問題，我可以在哪裡取得協助？

請參閱我們的[疑難排解指南](..\use\troubleshooting.md)和[已知問題](..\use\known-issues.md)清單。
