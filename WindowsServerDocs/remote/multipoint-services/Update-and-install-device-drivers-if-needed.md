---
title: 視需要更新及安裝裝置驅動程式
description: 瞭解如何檢查和更新 MultiPoint 服務中的設備磁碟機
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 0a6d389b0c34e7e1b794431dc1b1fefa8fef3fb4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949041"
---
# <a name="update-and-install-device-drivers-if-needed"></a>視需要更新及安裝裝置驅動程式
如果您使用 USB 零用戶端或需要驅動程式的週邊設備，您應該在這段時間安裝驅動程式。 也最好檢查是否有任何驅動程式警示的**Device Manager** ，並安裝這些裝置的驅動程式。

通常，下列裝置類型需要最新的驅動程式：

-   USB 零用戶端

-   USB-乙太網路的零用戶端

-   磁碟控制器

-   網路介面卡

-   音效控制器

-   USB 主機控制器

-   圖形配接器


## <a name="to-check-for-driver-alerts-in-device-manager"></a>若要檢查 Device Manager 中的驅動程式警示

1.  開啟 [開始] 畫面。

2.  輸入 [**電腦管理**]，然後按一下結果中的 [**電腦管理**]。

3.  在 [電腦管理] 主控台樹中，按一下 [ **Device Manager**]。

4.  在右側的系統裝置中，檢查可能影響 MultiPoint Server 的驅動程式警示。

## <a name="to-install-device-drivers-in-multipoint-manager"></a>在 MultiPoint 管理員中安裝設備磁碟機

1.  若要開啟 MultiPoint 管理員，請搜尋「MultiPoint 管理員」，然後在結果中按一下 [ **Multipoint 管理員**]。

2.  在 [MultiPoint 管理員] 中，按一下 [**首頁**] 索引標籤，然後按一下 [**切換至主控台模式**]。

3.  若要安裝設備磁碟機，請按兩下驅動程式檔案，然後遵循指示來安裝驅動程式。

4.  重複上述步驟來安裝所有必要的驅動程式。

    > [!NOTE]
    > 如果安裝需要重新開機電腦，則在安裝下一個驅動程式之前，您必須切換回主控台模式。 MultiPoint server 一律以站模式啟動。 若要切換至主控台模式，請移至 MultiPoint 管理員中的 [**首頁**] 索引標籤，然後按一下 [**切換至主控台模式**]。