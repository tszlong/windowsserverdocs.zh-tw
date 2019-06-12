---
title: 維護通道
description: Windows Server 服務通道的說明：LTSC 和 SAC
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: dee19cd5a30b7d913a7faeeaa38368cee8a91895
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442290"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Windows Server 服務的通道：LTSC 和 SAC

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

Windows Server 客戶有兩個適用的主要發行通道：長期維護通道和半年通道。

您可以將伺服器保留在長期維護通道 (LTSC)、將其移到半年通道，或在任一管道上各保留一些伺服器，端視何者最適合您的需求。

## <a name="long-term-servicing-channel-ltsc"></a>長期維護通道 (LTSC)

這是您已經熟悉的發行模型 (先前稱為「長期維護*分支*」)，Windows Server 依此模型每隔 2-3 年發行一次新的主要版本。 使用者有權接受 5 年的主要支援和 5 年的延伸支援。 此通道適用於需要較長期維護選項及功能穩定性的系統。 Windows Server 2016 及舊版 Windows Server 的部署不會受到新的半年通道發行影響。 長期維護通道仍將繼續收到安全性及非安全性更新，但無法獲得新特色與新功能。

> [!Note]  
> **目前的長期維護通道 (LTSC) 產品是 Windows Server 2019**。 如果您想要繼續留在這個通道中，就必須安裝 (或繼續使用) Windows Server 2019，而這可使用 [Server Core] 安裝選項或 [含桌面體驗的伺服器] 安裝選項來安裝。

## <a name="semi-annual-channel"></a>半年通道

半年通道非常適合進行創新快速，才能利用新的作業系統功能以更快速地，在應用程式 – 特別是建構在容器和微服務，以及軟體定義的客戶混合式資料中心。 半年通道中的 Windows Server 產品每年會有兩次新版本發行，分別在春季和秋季推出。 在此通道中發行的每個版本都會從初始版本開始提供 18 個月的支援。

半年通道中推出的大多數功能都會彙總到 Windows Server 的下一次長期維護通道發行。 各次發行的版本、功能及支援內容可能會依據客戶意見反應而有所改變。

半年通道會提供給大量授權客戶[軟體保證](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)，以及透過 Azure Marketplace 或其他雲端/主控服務提供者和忠誠度程式這類為 Visual Studio 訂用帳戶。

> [!Note]  
> **目前的半年通道發行版本是 Windows Server 版 1903年**。 如果您想要將伺服器放在此通道，您應該安裝 Windows Server 版 1903，可以安裝在 Server Core 模式或 Nano Server 容器中執行。 從長期服務的通道版本的就地升級不支援，由於它們位於**不同的發行管道**。 半年通道發行版本未更新一樣，它是在半年通道中的下一個 Windows Server 版本。

在此模型中，Windows Server 版本可依發行年份及月份來識別：例如，2017 年 9 月發行的版本會以**版本 1709** 做為識別。 半年通道中的 Windows Server 全新版本會每年發行兩次。 每個發行版本的支援週期為 18 個月。

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>您應該讓伺服器保持在 LTSC 或是移到半年通道？

以下是要考慮的關鍵差異：

- 您需要迅速創新嗎？ 您需要優先取得最新的 Windows Server 功能嗎？ 您需要支援快節奏的混合式應用程式、DevOps 和 Hyper-V 網狀架構嗎？ 如果因此，您應該考慮**加入 半年通道**藉由安裝**Windows Server 版 1903年**。 如本主題所述，您將會每年收到兩次新版本，每個版本各有 18 個月的主要生產環境支援。 您可以透過大量授權、Azure 或 Visual Studio 訂閱服務取得項目。 目前，如果您想在生產環境中執行產品，半年通道發行需要大量授權和軟體保證。
- 您需要穩定性與可預測性嗎？ 您需要在實體伺服器上執行虛擬機器和傳統工作負載嗎？ 若是，您應該考慮**將那些伺服器保持在長期維護通道**。 目前的 LTSC 版本是 **Windows Server 2019**。 如本主題所述，您可以每 2-3 年存取新版本，每個版本具備 5 年主要支援外加 5 年延伸支援。 LTSC 版本可透過所有發行機制提供。 無論使用何種授權模型，任何人都能取得 LTSC 中的發行版本。 

下表摘要說明通道間的主要差異：


|                       |                                                              長期維護通道 (Windows Server 2019)                                                               |                                   半年通道 (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| 建議的案例 | 一般用途檔案伺服器、Microsoft 和非 Microsoft 工作負載、傳統應用程式、基礎結構角色、軟體定義資料中心以及超融合式基礎結構 | 容器化應用程式、容器主機，以及因加快創新而受益的應用程式案例 |
|     最新發行      |                                                                               每隔 2-3 年一次                                                                                |                                              每隔 6 個月一次                                              |
|        支援        |                                                       5 年主要支援，加上 5 年延伸支援                                                        |                                                18 個月                                                 |
|       版本        |                                                                    所有可用的 Windows Server 版本                                                                     |                                     Standard 和 Datacenter Edition                                     |
|      誰可以使用      |                                                                      透過所有通道更新的所有客戶                                                                      |                               僅限軟體保證與雲端客戶                                |
| 安裝選項  |                                                                Server Core 以及具備桌面體驗的伺服器                                                                |                 適用於容器主機和映像以及 Nano Server 容器映像的 Server Core                 |

## <a name="device-compatibility"></a>裝置相容性

除非另有通知，執行半年通道發行版本的最低硬體需求與 Windows Server 最新的長期維護通道發行版本相同。 例如，**長期維護通道目前的版本是 Windows Server 2019**。 大部分的硬體驅動程式仍可繼續在這些版本中運作。

## <a name="servicing"></a>維護

長期維護通道和半年通道發行版本都會有安全性更新及非安全性更新的支援。 如上文所述，差別在於發行版本受支援的時間長度。

### <a name="servicing-tools"></a>維護工具

IT 專業人員有許多工具可以維護 Windows Server。 每個選項都有其優點和缺點，有各種功能和控制項可以簡化和降低系統管理需求。 以下是可用來管理維護更新的維護工具範例︰

- **Windows Update （獨立）** :此選項只適用於已連線到網際網路，且已啟用的 Windows 更新的伺服器。
- **Windows Server Update Services (WSUS)** 提供比 Windows 10 和 Windows Server 更新更加廣泛的控制，而且內建在 Windows Server 作業系統中。 除了延遲更新的能力，組織可以新增更新的核准層，並選擇在電腦就緒時將更新部署至特定的電腦或一組電腦。
- **System Center Configuration Manager** 提供最大限度掌控維護的能力。 IT 專業人員可以延遲更新、核准更新，而且有多個選項可以設定部署，以及管理頻寬的使用方式和部署時間。

您可能已經根據您的資源、人員及專業知識，選擇使用至少其中一個選項。 您仍然可以繼續對半年通道發行版本使用同樣的程序：例如，已經使用 System Center Configuration Manager 來管理更新，還是可以繼續使用。 同樣地，如果您正在使用 WSUS，也可以繼續這麼使用。

## <a name="where-to-obtain-semi-annual-channel-releases"></a>如何取得半年通道釋出

半年通道發行應該安裝為全新安裝。

- 大量授權服務中心 (VLSC):大量授權客戶[軟體保證](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)可以取得此版本中，移至[Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) ，然後按一下**登入**。 然後按一下 **\[下載和金鑰\]** 並搜尋此版本。 

- 半年通道發行版本中也會有[Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)。

- Visual Studio 訂用帳戶：Visual Studio 訂閱者可以下載從取得半年通道發行[Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)。 如果您還不是訂閱者，請前往 [Visual Studio 訂閱](https://www.visualstudio.com/subscriptions/)註冊，然後依上述方式瀏覽 [Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)。 透過 Visual Studio 訂閱取得的版本僅供開發和測試用途。

- 取得透過 Windows Insider 計劃的預覽版本：測試 Windows Server 的早期組建對 Microsoft 及其客戶都有幫助，因為這樣就有機會在發行前發現可能的問題。 同時也能讓客戶把握絕佳機會直接影響產品中的功能。   
Microsoft 對接收整個開發流程的意見反應有所依賴，藉此可以盡快進行調整。 早期測試和意見反應對快速發行模型極其重要，缺一不可。 若要參與 Windows Insider 計劃，請參閱[Server 文件的 Windows 測試人員計畫](https://docs.microsoft.com/windows-insider/at-work/)。

## <a name="activating-semi-annual-channel-releases"></a>啟用 半年通道釋出

- 如果您使用 Microsoft Azure，應該會自動啟動這個版本。
- 如果您已取得這個版本的大量授權服務中心或 Visual Studio 訂用帳戶，您可以使用您的 Windows Server 2019 CSVLK 與金鑰管理系統 (KMS) 環境中啟動它。 如需詳細資訊，請參閱 < [KMS 用戶端安裝識別碼](../get-started/kmsclientkeys.md)。

Windows Server 2019 之前發行的半年通道發行使用 Windows Server 2016 CSVLK。

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>半年通道發行為什麼提供只有 Server Core 安裝選項？

每一次規劃 Windows Server 發行版本時，我們採用的最重要步驟就是傾聽客戶意見反應：您使用 Windows Server 的情況如何？ 哪些功能會對您的 Windows Server 部署造成極大影響，甚至衝擊日常業務？ 您的意見反應讓我們知道盡快且盡可能有效率提供新創新是主要優先考量。 在此同時，最快的速度創新的客戶您告訴我們您主要使用命令列指令碼和 PowerShell 來管理您的資料中心，並因此不需要強式桌面的 GUI 的安裝提供所需Windows 伺服器含桌面體驗，特別是既然[Windows Admin Center](../manage/windows-admin-center/overview.md)是可用來從遠端管理您的伺服器。

我們只要將重點放在 Server Core 安裝選項，就可以將更多資源投向這些新創新，同時還能維持傳統 Windows Server 平台功能及應用程式相容性。 如果您有關於這一點或其他有關 Windows Server 及未來發行版本之問題的意見反應，可以透過[意見反應中樞](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)提出建議和意見。

## <a name="what-about-nano-server"></a>Nano Server 又會是怎樣呢？

Nano Server 是可用容器中的作業系統半年通道。 如需詳細資訊，請參閱[Nano Server 在 Windows Server 半年度管道中的變更](../get-started/nano-in-semi-annual-channel.md)。

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>如何判斷伺服器是否正在執行的 LTSC 或 SAC 的版本

一般而言，例如 Windows Server 2019 發行為新版本的半年通道，比方說，Windows Server，版本 1809年同時釋出長期維護通道。 這可讓較難判斷伺服器是否正在執行半年通道發行。 而不是查看組建編號，您必須查看產品名稱：半年通道發行版本會使用 「 Windows Server Standard 」 或 「 Windows Server Datacenter 」 產品名稱，而不需要的版本號碼，雖然長期維護通道版本包含版本號碼，例如，「 Windows Server 2019 Datacenter 」。

>[!Note]  
> 下列指導方針旨在協助識別和區分 LTSC 以及 SAC，但僅適用於生命週期和一般清查目的。  它不適用於應用程式相容性，也不代表特定的 API 介面。  由於元件、API 和功能在系統的整個生命週期都可以加入或不加入，因此應用程式開發人員應該使用其他地方的指導方針以正確地確保相容性。 [作業系統版本](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version)是適用於應用程式開發人員的更好起始點。

開啟 Powershell，然後使用 Get-ItemProperty Cmdlet 或 Get-ComputerInfo Cmdlet 來檢查登錄中的這些內容。  除了組建編號，這也會透過品牌年份的存在 (或缺乏) 來指出 LTSC 或 SAC，也就是 2019。  LTSC 有這個項目，SAC 沒有。  這也會透過 ReleaseId 或 WindowsVersion 傳回發行時間，也就是 1809，以及安裝的是 Server Core 或是具備桌面體驗的伺服器。 

**Windows Server 2019 Datacenter Edition (LTSC) 桌面體驗的範例：**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server，版本 1809 (SAC) Standard Edition 伺服器核心的範例：**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server 2019 Standard Edition (LTSC) 伺服器核心的範例：**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

若要查詢伺服器上是否存在新的 [Server Core 應用程式相容性功能 FOD](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19)，請使用 [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) Cmdlet 並尋找：
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>另請參閱

[在 Windows Server 半年通道中的 Nano 伺服器的變更](../get-started/nano-in-semi-annual-channel.md)

[Windows Server 支援生命週期](https://support.microsoft.com/lifecycle)

[判斷是否正在執行 Server Core](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[GetProductInfo 函數](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[軟體清查記錄 Cmdlet](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
