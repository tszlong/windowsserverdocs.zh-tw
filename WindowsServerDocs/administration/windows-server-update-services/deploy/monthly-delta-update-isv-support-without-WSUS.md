---
title: 不含 WSUS 的每月差異更新 ISV 支援
description: Windows Server Update Service （WSUS）主題-獨立軟體廠商（ISV）如何暫時使用每月差異更新，而不是 WSUS Express 更新傳遞以減少套件大小
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 4607827d73c34f50f721a2774fa498eb95f9dbb8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361726"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>不含 WSUS 的每月差異更新 ISV 支援

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10

Windows 10 更新下載可能會很大，因為每個套件都包含所有先前發行的修正程式，以確保一致性和簡易性。  

自版本7起，Windows 已能夠使用稱為[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)的功能減少 Windows Update 下載的大小，而雖然消費者裝置預設支援，但 windows 10 企業版裝置需要 WINDOWS SERVER UPDATE SERVICES （WSUS）才能執行Express 的優勢。 如果您有可用的 WSUS，請參閱[快速更新傳遞 ISV 支援](express-update-delivery-ISV-support.md)。 我們建議使用它來啟用快速更新傳遞。 

如果您目前未安裝 WSUS，但您在過渡期間需要較小的更新套件大小，您可以使用每月差異更新。 差異更新會大幅減少套件大小，但不像 WSUS Express 更新傳遞一樣多。 我們建議您盡可能部署 WSUS Express 更新，以最大的方式減少套件大小。 以下圖表比較 Windows 10 1607 版的差異、累計和快速下載大小：

![下載大小比較](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>什麼是每月差異更新？

每月安全性更新有兩種變體：差異與累計。

每月差異更新是新的，而且沒有 WSUS 可協助減少更新套件大小的 Isv 的過渡解決方案。

>[!IMPORTANT]
>**差異更新適用于 Windows 10 版本1607（年度更新版）、版本1703（建立者更新）和版本1709（秋季建立者更新）。** 針對版本1709之後的版本，您必須執行支援[快速更新傳遞](express-update-delivery-ISV-support.md)的部署基礎結構，才能繼續利用累加式更新。

藉由使用每月差異更新，套件只會包含一個月的更新。 [每月累計] 包含該更新版本的所有更新，導致每個月增加一個大型檔案。 差異和每月更新會在每個月的第二個星期二發行，也稱為「更新星期二」。 下表比較差異和累計更新：

|                    | 每月**差異**更新                                                                                                                                                                                                       | 每月**累計**更新                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **範圍**          | **僅針對該月份提供新修正**的單一更新                                                                                                                                                                           | 包含該月份所有新修正和前幾個月的單一更新                                                                                                                                                   |
| **應用程式**    | 只有在套用上個月的更新（累計或差異）時，才能套用                                                                                                                                           | 可以隨時套用                                                                                                                                                                                                |
| **運送**       | **僅發佈至 Windows Update 目錄**，可供下載以用於其他工具或進程。 未提供給連線到 Windows Update 的電腦                                                         | 已發佈至 Windows Update （所有取用者電腦將會安裝）、WSUS 和 Windows Update 目錄                                                                                                                |

差異與累計具有相同的 KB 號碼，具有相同的分類，並同時發行。 更新可透過目錄中的更新標題，或 msu 的名稱來區別：

- 適用于 Windows 10 版本1607（x64 型系統）的 2017-02 * \***差異更新**\* *  （KB1234567）
- 適用于 x86 系統（KB1234567）之 Windows 10 版本1607的 2017-02 * \***累積更新**\* *                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        

### <a name="when-to-use-monthly-delta-update"></a>何時使用每月差異更新

如果用戶端裝置的更新大小是問題，建議您在具有上個月更新的裝置上使用差異更新，並在落後的裝置上進行累計更新。 如此一來，所有裝置只需要一次更新，就能使其保持在最新狀態。 這需要在整體更新管理程式中進行小型調整，因為您必須根據裝置在組織中的最新狀態來部署不同的更新：

![下載大小比較](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>防止在同一個月部署差異和累計更新

由於差異更新和累計更新可同時使用，因此請務必瞭解，如果您在同一個月部署這兩個更新，會發生什麼情況。

如果您核准並部署相同版本的差異和累計更新，您將不會產生額外的網路流量，因為這兩者都會下載至電腦，但在重新開機之後，您可能無法將電腦重新開機至 Windows。

如果差異和累計更新不小心安裝，而且您的電腦已不再開機，您可以使用下列步驟來復原：

1. 開機進入 WinRE 命令提示字元
2. 列出處於暫止狀態的套件：

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **範例**：` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. 開啟您用來輸送**套件**的文字檔。 如果您看到任何安裝擱置修補程式，請執行每個套件名稱的**移除套件**：
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **範例**：`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >請勿移除卸載擱置中的修補程式。

>[!IMPORTANT]
>**差異更新適用于 Windows 10 版本1607（年度更新版）、版本1703（建立者更新）和版本1709（秋季建立者更新）。** 針對版本1709之後的版本，您必須執行支援[快速更新傳遞](express-update-delivery-ISV-support.md)的部署基礎結構，才能繼續利用累加式更新。
