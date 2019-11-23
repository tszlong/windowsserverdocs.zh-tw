---
ms.assetid: ''
title: 高精確度時間的支援界限
description: 本文說明 Windows Time （W32Time）服務在需要高度精確且穩定系統時間的環境中的支援界限。
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405262"
---
# <a name="support-boundary-for-high-accuracy-time"></a>高精確度時間的支援界限

>適用于： Windows Server 2016 和 Windows 10 1607 版或更新版本

本文說明 Windows 時間服務（W32Time）在需要高度精確且穩定系統時間的環境中的支援界限。

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Windows 8.1 和 2012 R2 （或之前）的高準確度支援

舊版的 Windows （在 Windows 10 1607 或 Windows Server 2016 1607 之前）無法保證非常精確的時間。 這些系統上的 Windows 時間服務：

-   提供滿足 Kerberos 第5版驗證需求所需的時間精確度

-   為已加入 common Active Directory 樹系的 Windows 用戶端和伺服器提供了鬆散精確的時間

較緊密的精確度需求不在這些作業系統上的 Windows Time 服務設計規格之外，而且也不受支援。

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016

Windows 10 和 Windows Server 2016 中的時間準確度已大幅改善，同時維持與舊版 Windows 的完整回溯 NTP 相容性。 在正確的作業條件下，執行 Windows 10 或 Windows Server 2016 和更新版本的系統可以提供1秒、50毫秒（毫秒）或1毫秒準確度。

>[!IMPORTANT]
>**高度精確的時間來源**<br>
>在您的拓撲中產生的時間精確度，高度取決於使用精確的穩定根（層次1）時間來源。 協力廠商提供的 Windows 型和非 Windows 型高度精確、Windows 相容、NTP 時間來源硬體。 請洽詢您的廠商以瞭解其產品的準確度。

>[!IMPORTANT]
>**時間精確度**<br>
>時間精確度需要從高度精確的授權時間來源到結束裝置的端對端散發正確時間。 引進網路不對稱的任何專案都會對精確度造成負面影響，例如，實體網路裝置或目標系統上的高 CPU 負載。

## <a name="high-accuracy-requirements"></a>高準確度需求

本檔的其餘部分將概述必須滿足才能支援各自高準確度目標的環境需求。

### <a name="target-accuracy-1-second-1s"></a>目標精確度：1秒（1）

相較于高度精確的時間來源，為特定的目的電腦達到1% 的精確度：

-   目標系統必須執行 Windows 10、Windows Server 2016。

-   目標系統必須從時間伺服器的 NTP 階層同步處理時間，累積在高度精確的 Windows 相容 NTP 時間來源。

-   上述 NTP 階層中的所有 Windows 作業系統都必須依照設定[高精確度的系統](configuring-systems-for-high-accuracy.md)檔中所述進行設定。

-   目標和來源之間的累計單向網路延遲不得超過100毫秒。 累計網路延遲的測量方式，是在階層中的 NTP 用戶端-伺服器節點配對之間新增個別的單向延遲，從目標開始，並在來源結束。 如需詳細資訊，請參閱高準確度時間同步檔。

### <a name="target-accuracy-50-milliseconds"></a>目標精確度：50毫秒

「**目標精確度：1秒**」一節中所述的所有需求，除非此章節中概述嚴格的控制項。

達到特定目標系統50毫秒精確度的其他需求如下：

-   目的電腦的時間來源之間必須比5毫秒網路延遲來得好。

-   目標系統必須從高度精確的時間來源開始，不能高於第5級

    >[!Note]
    >從命令列執行 "w32tm/query/status" 以查看層次。

-   目標系統的網路躍點必須介於高度精確的時間來源內

-   所有 stratums 上的一天平均 CPU 使用率不得超過90%

-   針對虛擬化系統，主機的一天平均 CPU 使用率不得超過90%

### <a name="target-accuracy-1-millisecond"></a>目標精確度：1毫秒

各節中所述的所有需求都是以 [**目標精確度：1秒**] 和 [**目標精確度：50毫秒**] 為限，但本節中所述的嚴格控制項除外。

針對特定目標系統達到1毫秒精確度的額外需求如下：

-   目的電腦的時間來源必須優於0.1 毫秒的網路延遲

-   目標系統必須從高度精確的時間來源開始，不能高於第5級

    >[!Note]
    >從命令列執行 ' w32tm/query/status ' 以查看層次

-   目標系統的網路躍點必須介於高度精確的時間來源內

-   每個層次的一天平均 CPU 使用率不得超過80%

-   針對虛擬化系統，主機的一天平均 CPU 使用率不得超過80%
