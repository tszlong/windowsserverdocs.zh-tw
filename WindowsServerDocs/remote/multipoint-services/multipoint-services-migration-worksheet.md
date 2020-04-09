---
title: 適用于 MultiPoint 服務遷移的規劃工作表
description: 提供可協助您在 Windows Server 2016 中遷移至 MultiPoint 服務的規劃工作表
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: c0d5976e70bcf8009cd98e54e973dd6f585d7208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858901"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>適用于 MultiPoint 服務遷移的規劃工作表

>適用於︰Windows Server 2016

使用下列清單和資料表，在 MultiPoint 服務遷移期間收集您所需的設定。

## <a name="source-server-settings"></a>來源伺服器設定

您可以在 [MultiPoint 管理員] 的 [**首頁**] 索引標籤上找到伺服器設定。 在來源伺服器上使用的每個設定旁邊放置核取記號。

- 允許一個帳戶有多個會話。
- 允許從遠端系統管理這部電腦。
- 允許監視這部電腦的桌面。
- 一律以主控台模式啟動。
- 請不要在第一次使用者登入時顯示隱私權通知。
- 為每個工作站指派唯一的 IP。
- 允許在 MultiPoint 儀表板與此電腦上的使用者會話之間進行 IM。
- 允許系統管理員和 MultiPoint 儀表板使用者會話的協調流程。
- 允許工作站使用 GPU 硬體呈現。

## <a name="managed-servers-and-computers"></a>受管理的伺服器和電腦

記錄受管理伺服器和電腦的名稱。 您可以在 [MultiPoint 管理員] 的 [**首頁**] 索引標籤上找到這項資訊。

| 電腦 | 電腦名稱 |
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


## <a name="stations"></a>月臺

記錄本機工作站和其設定。 您可以在 MultiPoint 管理員的 [**工作站**] 索引標籤上找到這項資訊。

| #  | 站台名稱 | 自動登入使用者帳戶 | 顯示方向 |
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

複製 [系統管理員] 和 [MultiPoint 儀表板] 使用者的使用者名稱。 您可以在 MultiPoint 管理員的 [**使用者**] 索引標籤中找到這項資訊。

Administrators：

- 使用者名稱:
- 使用者名稱:
- 使用者名稱:
- 使用者名稱:
- 使用者名稱:
- 使用者名稱:

儀表板使用者：

- 使用者名稱:
- 使用者名稱:
- 使用者名稱:
- 使用者名稱:
- 使用者名稱:

## <a name="vdi-template-and-virtual-desktops"></a>VDI 範本與虛擬桌面

記錄您 MultiPoint 服務部署中的 VDI 範本資訊和虛擬桌面名稱。 您可以在 MultiPoint 管理員的 [**虛擬桌面**] 索引標籤中找到這項資訊。

**VDI 範本位置**： 

| # | 虛擬桌面名稱      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |