---
title: MultiPoint 服務移轉的規劃工作表
description: 提供可協助您移轉至 Windows Server 2016 中的 MultiPoint 服務的規劃工作表
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880579"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>MultiPoint 服務移轉的規劃工作表

>適用於：Windows Server 2016

使用下列清單和資料表來收集您需要在 MultiPoint 服務移轉期間的設定。

## <a name="source-server-settings"></a>來源伺服器設定

您可以上找到的伺服器設定**首頁**MultiPoint 管理員中的索引標籤。 將每個設定旁邊核取記號放在來源伺服器上使用。

- 允許一個帳戶有多個工作階段。
- 允許從遠端管理這部電腦。
- 能夠監視此電腦的桌面。
- 一律以主控台模式啟動。
- 在第一個使用者登入時不要顯示隱私權通知。
- 每個站台指派唯一的 IP。
- 在此電腦上，可讓 MultiPoint 儀表板與使用者工作階段之間的 IM。
- 可讓系統管理員和 MultiPoint 儀表板使用者工作階段的協調流程。
- 允許站台使用 GPU 硬體轉譯。

## <a name="managed-servers-and-computers"></a>受管理的伺服器和電腦

記錄受管理的伺服器和電腦的名稱。 您可以找到此資訊在**首頁**MultiPoint 管理員中的索引標籤。

| Computer | 電腦名稱 |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>站台

記錄本機站台和其設定。 您可以找到此資訊在**站台**MultiPoint 管理員中的索引標籤。

| #  | 站台名稱 | 自動登入的使用者帳戶 | 顯示方向 |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>系統管理員和 MultiPoint 儀表板使用者

將複製的系統管理員和 MultiPoint 儀表板使用者的使用者名稱。 您可以找到此資訊在**使用者**MultiPoint 管理員中的索引標籤。

Administrators：

- 使用者名稱：
- 使用者名稱：
- 使用者名稱：
- 使用者名稱：
- 使用者名稱：
- 使用者名稱：

儀表板使用者：

- 使用者名稱：
- 使用者名稱：
- 使用者名稱：
- 使用者名稱：
- 使用者名稱：

## <a name="vdi-template-and-virtual-desktops"></a>VDI 範本和虛擬桌面

記錄您的 MultiPoint 服務部署中的 VDI 範本資訊，以及虛擬桌面的名稱。 您可以找到此資訊在**虛擬桌面**MultiPoint 管理員中的索引標籤。

**VDI 範本位置**: 

| # | 虛擬桌面的名稱      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |