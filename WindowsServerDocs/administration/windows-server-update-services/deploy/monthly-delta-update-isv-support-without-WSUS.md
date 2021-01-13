---
title: 不含 WSUS 的每月差異更新 ISV 支援
description: Windows Server Update Service (WSUS) 主題 - 獨立軟體廠商 (ISV) 如何使用每月差異更新而非 WSUS 快速更新傳遞來減少套件大小
ms.topic: how-to
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5c15784e7b276605b09eeb3014cd0d823750afa4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947604"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>不含 WSUS 的每月差異更新 ISV 支援

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

Windows 10 更新下載大小可能會很大，因為每個套件都包含所有先前發行的修正，以確保一致性與簡易性。

從第 7 版開始，Windows 已可使用名為[快速 (Express)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708456(v=ws.10)#Anchor_2) 的功能來減少 Windows Update 下載的大小，而雖然取用者裝置依預設可加以支援，但 Windows 10 企業版裝置仍需要 Windows Server Update Services (WSUS) 才能使用「快速 (Express)」。 如果您已有 WSUS，請參閱[快速更新傳遞的 ISV 支援](express-update-delivery-ISV-support.md)。 我們建議使用此功能來啟用快速更新傳遞。

如果您目前未安裝 WSUS，但在過渡期間需要較小的更新套件大小，則可以使用每月差異更新。 差異更新可大幅減少套件大小，但減少的量比 WSUS 快速更新傳遞所減少的量較少。 我們建議您盡可能部署 WSUS 快速更新，以將套件大小減到最小。 以下圖表比較 Windows 10 版本 1607 的差異 (Delta)、累積 (Cumulative)、快速 (Express) 下載的大小：

![下載的大小比較](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>什麼是每月差異更新？

每月安全性更新有兩種變體：差異和累積。

每月差異更新是新的，而且對沒有 WSUS 可協助減少更新套件大小的情況來說，是 ISV 的過渡解決方案。

>[!IMPORTANT]
>**差異更新適用於 Windows 10 版本1607 (年度更新)、版本 1703 (建立者更新)、版本 1709 (秋季建立者更新)。** 針對版本 1709 之後的版本，您必須實作支援[快速更新傳遞](express-update-delivery-ISV-support.md)的部署基礎結構，才能繼續利用累加式更新的優點。

使用每月差異更新，套件只會包含一個月的更新。 每月累積包含至該更新版本為止的所有更新，導致每個月都會加大的一個大型檔案。 差異和累積更新都是在每個月的第二個星期二發行，也稱為「更新星期二」。 下表比較差異和累積更新：

|                    | 每月 **差異** 更新                                                                                                                                                                                                       | 每月 **累積** 更新                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **範圍**          | **僅包含該月份新修正** 的單一更新                                                                                                                                                                           | 包含該月份和先前月份所有新修正的單一更新                                                                                                                                                   |
| **應用程式**    | 只有在已套用上個月的更新 (累積或差異) 時，才能套用                                                                                                                                           | 隨時可以套用                                                                                                                                                                                                |
| **傳遞**       | **僅發佈到 Windows Update 目錄**，此目錄可供下載用於其他工具或程序。 沒有提供給連線到 Windows Update 的電腦                                                         | 發佈至 Windows Update (所有取用者電腦將會安裝)、WSUS 和 Windows Update 目錄                                                                                                                |

差異與累積具有相同的 KB 號碼、相同的分類，且同時發行。 可透過目錄中的更新標題或 msu 的名稱來區別更新：

- 2017-02 *\***差異更新**\** 適用於 x64 型系統的 Windows 10 版本 1607 (KB1234567)
- 2017-02 *\***累積更新**\** 適用於 x86 型系統的 Windows 10 版本 1607 (KB1234567)

### <a name="when-to-use-monthly-delta-update"></a>何時使用每月差異更新

如果更新的大小對用戶端裝置而言是個問題，建議您在具有上個月更新的裝置上使用差異更新，在更新落後的裝置上使用累積更新。 如此一來，所有裝置只需要一次更新，就能使其保持在最新狀態。 這需要對整體更新管理程序做個小調整，因為您必須根據組織中裝置的最新狀態來部署不同的更新：

![下載的大小比較](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>避免在同一個月部署差異和累積更新

由於差異更新和累積更新是同時發行，請務必瞭解如果您在同一個月部署這兩個更新會發生什麼情況。

如果您核准並部署相同版本的差異和累積更新，您將不會產生額外的網路流量，因為這兩者都會下載至電腦，但在重新開機之後，您可能無法將電腦重新開機至 Windows。

如果不小心同時安裝了差異和累積更新，而且您的電腦不再開機，您可以使用下列步驟來復原：

1. 開機進入 WinRE 命令提示字元
2. 列出擱置狀態的套件：

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`

    > **範例**：` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`

3. 開啟您用來傳送 **get-packages** 結果的文字檔。 如果您看到任何安裝擱置修補程式，請對每個套件名稱執行 **remove-package**：

   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`

    > **範例**：`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`

    >[!NOTE]
    >請勿移除解除安裝擱置修補程式。

>[!IMPORTANT]
>**差異更新適用於 Windows 10 版本1607 (年度更新)、版本 1703 (建立者更新)、版本 1709 (秋季建立者更新)。** 針對版本 1709 之後的版本，您必須實作支援[快速更新傳遞](express-update-delivery-ISV-support.md)的部署基礎結構，才能繼續利用累加式更新的優點。