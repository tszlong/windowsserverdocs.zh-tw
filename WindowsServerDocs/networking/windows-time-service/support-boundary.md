---
ms.assetid: ''
title: 高精確度時間的支援界限
description: 本文說明在需要高度精確且穩定的系統時間的環境中的 Windows 時間 (W32Time) 服務的支援界限。
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866269"
---
# <a name="support-boundary-for-high-accuracy-time"></a>高精確度時間的支援界限

>適用於：Windows Server 2016 和 Windows 10 版本 1607年或更新版本

本文說明 Windows 時間服務 (W32Time) 的支援界限需要高度精確且穩定的系統時間的環境中。

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>適用於 Windows 8.1 和 2012 R2 （或之前） 的高精確度支援

舊版的 Windows （Windows 10 1607年或 Windows Server 2016 1607年） 之前，無法保證高度精確的時間。 在這些系統上 Windows 時間服務：

-   提供所需的時間精確度，以滿足 Kerberos 版本 5 驗證需求

-   Windows 用戶端與伺服器加入常見的 Active Directory 樹系提供鬆散準確的時間

更緊密的精確度需求所設計規格，在這些作業系統上的 Windows 時間服務之外，並不支援。

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016

在 Windows 10 和 Windows Server 2016 中的時間精確度已經過大幅改良，同時維持完整回溯 NTP 與較舊的 Windows 版本的相容性。 在右邊的運作狀況下，執行 Windows 10 或 Windows Server 2016 和較新版本的系統可以提供 1 的第二個，50 毫秒 （毫秒），或 1 毫秒精確度。

>[!IMPORTANT]
>**高度精確的時間來源**<br>
>在拓撲中產生的時間精確度，多半取決於使用的精確且穩定的根 (stratum 1) 時間來源。 有 Windows 型和非 Windows 型高精確，Windows 相容，NTP 時間來源硬體銷售的第 3 方供應商。 請洽詢您的供應商，其產品的精確度。

>[!IMPORTANT]
>**時間精確度**<br>
>時間精確度需要端對端分佈的精確時間高精確度的授權時間來源的裝置。 導入網路上的不對稱的任何項目會造成負面影響精確度，例如實體網路裝置或在目標系統上的高 CPU 負載。

## <a name="high-accuracy-requirements"></a>高精確度的需求

這份文件的其餘部分將概述必須滿足才能支援個別的高精確度的目標環境需求。

### <a name="target-accuracy-1-second-1s"></a>目標精確度：1 秒 (1)

若要達到 lt;1s&gt 特定目標的精確度機器相較於高度精確的時間來源：

-   目標系統必須執行 Windows 10，Windows Server 2016。

-   目標系統必須同步處理時間 NTP 時間伺服器階層，重點在高度精確的是，Windows 相容 NTP 時間來源。

-   在上述的 NTP 階層中所有 Windows 作業系統必須都設定中所述[設定為高精確度的系統](configuring-systems-for-high-accuracy.md)文件。

-   來源與目標之間的累計單向的網路延遲不能超過 100 毫秒。 累積的網路延遲會加上個別測量之間的成對 NTP 用戶端-伺服器節點與目標開始與結束，在來源階層中的單向延遲。 如需詳細資訊，請檢閱高精確度的時間同步處理文件。

### <a name="target-accuracy-50-milliseconds"></a>目標精確度：50 毫秒

一節所述的所有需求**目標精確度：1 秒鐘**套用，但會在這一節中所述的控制項更加嚴格。

若要達到 50 毫秒精確度，針對特定目標系統的其他需求如下：

-   目標電腦必須與其時間來源間的優於 5 毫秒的網路延遲。

-   目標系統必須是最理想 stratum 高度精確的時間來源的 5

    >[!Note]
    >從命令列以查看 stratum 執行 「 w32tm /query /status"。

-   目標系統必須在 6 個或更少的網路躍點高度精確的時間來源的資料

-   為期一天平均 CPU 使用率在所有 stratums 不得超過 90%

-   適用於虛擬化系統，主應用程式的一天平均 CPU 使用率不能超過 90%

### <a name="target-accuracy-1-millisecond"></a>目標精確度：1 毫秒

各節中所述的所有需求**目標精確度：1 秒鐘**和**目標精確度：50 毫秒**套用，但會在這一節中所述的控制項更加嚴格。

若要達成特定目標系統的 1 毫秒精確度的其他需求如下：

-   目標電腦必須與其時間來源間有優於 0.1 毫秒的網路延遲

-   目標系統必須是最理想 stratum 高度精確的時間來源的 5

    >[!Note]
    >從命令列以查看 stratum 執行 'w32tm /query /status'

-   目標系統必須在 4 個或更少的網路躍點高度精確的時間來源的資料

-   每個 stratum 為期一天平均 CPU 使用率不能超過 80%

-   虛擬化系統，主應用程式的一天平均 CPU 使用率必須不超過 80%
