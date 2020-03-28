---
ms.assetid: ''
title: 高準確度時間的支援界限
description: 本文說明 Windows 時間 (W32Time) 服務在需要高度準確且穩定系統時間之環境中的支援界限。
author: eross-msft
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 5748d598272c8f096876bab0d24142d38c2fd64b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314968"
---
# <a name="support-boundary-for-high-accuracy-time"></a>高準確度時間的支援界限

>適用於：Windows Server 2016 與 Windows 10 版本 1607 或更新版本

本文說明 Windows Time 服務 (W32Time) 在需要高度準確且穩定系統時間之環境中的支援界限。

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Windows 8.1 和 2012 R2 (或之前) 的高準確度支援

舊版的 Windows (Windows 10 1607 或 Windows Server 2016 1607 之前的版本) 無法保證能提供非常準確的時間。 這些系統上的 Windows Time 服務：

-   提供必要的時間準確度以滿足 Kerberos 第 5 版的驗證需求

-   為已加入一般 Active Directory 樹系的 Windows 用戶端和伺服器提供準確度沒那麼高的時間

這些作業系統上的 Windows Time 服務設計規格並不包含較緊密的準確度需求，而且也不支援。

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016

Windows 10 和 Windows Server 2016 中的時間準確度已大幅改善，同時維持與舊版 Windows 的完整 NTP 回溯相容性。 在正確的作業條件下，執行 Windows 10 或 Windows Server 2016 和較新版本的系統可以提供 1 秒、50 ms (毫秒) 或 1 毫秒的準確度。

>[!IMPORTANT]
>**高度準確的時間來源**<br>
>在拓撲中產生的時間，其準確度高度取決於是否使用準確且穩定的根 (組織層 1 ) 時間來源。 第三方廠商也有提供 Windows 型和非 Windows 型的高度準確、與 Windows 相容的 NTP 時間來源硬體。 請洽詢您的廠商以了解其產品的準確度。

>[!IMPORTANT]
>**時間準確度**<br>
>時間準確度必須以端對端的方式，將準確的時間從高度準確的授權時間來源發佈到終端裝置。 導入網路不對稱的任何因素都會對準確度造成負面影響；例如，實體網路裝置或目標系統上的高 CPU 負載。

## <a name="high-accuracy-requirements"></a>高準確度需求

本文件的其餘部分將概述一些環境需求，系統必須滿足這些需求才能支援相對的高準確度目標。

### <a name="target-accuracy-1-second-1s"></a>目標準確度：1 秒 (1s)

與高準確度的時間來源比較時，達到特定目的電腦 1 秒的準確度：

-   目標系統必須執行 Windows 10 或 Windows Server 2016。

-   目標系統必須從時間伺服器的 NTP 階層同步時間，這是高度準確且相容於 Windows 的 NTP 時間來源。

-   上述 NTP 階層中的所有 Windows 作業系統都必須如[設定高準確度系統](configuring-systems-for-high-accuracy.md)文件中所記載之內容設定。

-   目標和來源之間的累計單向網路延遲不得超過 100 毫秒。 累計網路延遲的測量方式，是在階層中的 NTP 用戶端-伺服器節點配對之間新增個別的單向延遲；從目標開始，並在來源結束。 如需詳細資訊，請參閱高準確度時間同步文件。

### <a name="target-accuracy-50-milliseconds"></a>目標準確度：50 毫秒

除了本節概述的較嚴格控制項，**目標準確度：1 秒** 一節中概述的所有需求均適用。

達到特定目標系統 50 毫秒準確度的其他需求如下：

-   目標電腦之時間來源間的網路延遲必須優於 5 毫秒。

-   目標系統的高度準確時間來源不能高於組織層 5

    >[!Note]
    >從命令列執行 "w32tm /query /status" 可以查看組織層。

-   目標系統的高度準確時間來源必須包含 6 個以下的網路躍點

-   所有組織層中的單日平均 CPU 使用率不得超過 90%

-   針對虛擬化系統，主機的單日平均 CPU 使用率不得超過 90%

### <a name="target-accuracy-1-millisecond"></a>目標準確度：1 毫秒

除了本節概述的較嚴格控制項，**目標準確度：1 秒**和**目標準確度：50 毫秒**中概述的所有需求均適用。

達到特定目標系統 1 毫秒準確度的其他需求如下：

-   目標電腦之時間來源間的網路延遲必須優於 0.1 毫秒

-   目標系統的高度準確時間來源不能高於組織層 5

    >[!Note]
    >從命令列執行 `w32tm /query /status' 可以查看組織層

-   目標系統的高度準確時間來源必須包含 4 個以下的網路躍點

-   跨每個組織層中的單日平均 CPU 使用率不得超過 80%

-   針對虛擬化系統，主機的單日平均 CPU 使用率不得超過 80%
