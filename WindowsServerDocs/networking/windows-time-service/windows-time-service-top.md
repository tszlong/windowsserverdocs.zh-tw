---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 時間服務
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Windows 時間服務 (W32Time)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows 時間服務 (W32Time) 同步處理的日期和時間的所有電腦執行 Active Directory Domain Services (AD DS)。 同步處理時間是重要的許多 Windows 服務和行營運 (LOB) 應用程式的正常運作。 Windows 時間服務會使用同步電腦在網路上的時鐘網路時間通訊協定 (NTP)。 NTP 確保準確時鐘值或戳記，可指派網路存取權的要求驗證和資源。

Windows 時間服務 (W32Time) 主題中下列 content 可：
- **[Windows 2016 正確的時間](accurate-time.md)。** Windows Server 2016 同步處理時間精確度已經大幅，改善維持向前全 NTP 較舊的 Windows 版本的相容性。  您可以在合理操作條件維護 1 ms 正確性 UTC-10 或 Windows Server 2016 和 Windows 10 年度更新版網域成員更好。
- **[Windows 時間服務技術參考](windows-time-service-tech-ref.md)。** W32Time 服務提供的網路時鐘同步處理的電腦，而不需要廣泛設定。 已基本 Kerberos V5 驗證順利運作，因此，到 AD DS 驗證 W32Time 的服務。
    - **[Windows 時間服務如何](How-the-Windows-Time-Service-Works.md)。** Windows 時間服務不精確實作網路時間通訊協定 (NTP)，但它將會使用確保為越準確時鐘電腦在網路上的 NTP 規格中所定義的套件複雜的演算法。
    - **[Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)。** 大多數成員網域的電腦有 NT5DS，這表示同步處理時間從網域層的時間 client 類型。 這只有一般例外是網域控制站為的樹系根網域中，這通常設定為使用外部的時間來源同步處理時間主要網域控制站 (PDC) 模擬器操作主機功能。

## <a name="related-topics"></a>相關的主題
如網域階層和評分系統的相關詳細資訊，請查看["What's Windows 時間服務」？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

Windows 時間提供者增益集模型是[記載 TechNet 在](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

Windows 2016 正確時間文章參考增補可以下載[在此](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Windows 時間服務的快速概述，看看這個[高階概觀視訊](https://aka.ms/WS2016TimeVideo)。

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->

