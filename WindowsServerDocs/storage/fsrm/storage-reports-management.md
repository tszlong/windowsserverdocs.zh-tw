---
title: 存放裝置報告管理
description: 本文說明如何產生、排程和監視存放裝置報告
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5bdcada1b445298c8743bdb39491726b594d0a66
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475475"
---
# <a name="storage-reports-management"></a>存放裝置報告管理

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

在檔案伺服器資源管理員 Microsoft<sup>®</sup> Management Console (MMC) 嵌入式管理單元的 **\[存放裝置報告管理\]** 節點，您可以執行下列工作：

-   排程可讓您識別磁碟使用量趨勢的定期存放裝置報告。
-   監視所有使用者或所選使用者群組儲存未授權檔案的嘗試。
-   立即產生存放裝置報告。

例如，您可以：

-   將報告排程在每個星期天午夜執行，並產生一份包含過去兩天內最新存取的檔案清單。 您可以利用這項資訊來監視週末存放活動，並規劃對整個周末都在家中連線使用者影響較小的伺服器停機時間。
-   隨時執行報告以找出伺服器磁碟區中所有的重複檔案，讓磁碟空間可以在不遺失任何資料的情況下快速回收。
-   執行 [檔案: 依檔案群組列表] 報告以識別存放裝置資源分散在不同檔案群組之間的情形。
-   執行 [檔案: 依擁有者列表] 報告以分析個別使用者使用共用存放裝置資源的情形。

本節包含下列主題：

-   [排程一組報告](schedule-set-of-reports.md)
-   [產生隨選報告](generate-reports-on-demand.md)

> [!Note]
> 若要設定電子郵件通知和某些報告功能，您必須先設定檔案伺服器資源管理員的一般選項。

## <a name="additional-references"></a>其他參考

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)


