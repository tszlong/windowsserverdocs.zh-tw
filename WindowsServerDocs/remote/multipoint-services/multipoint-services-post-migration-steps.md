---
title: MultiPoint 服務-移轉後工作
description: 了解如何驗證和關閉 MultiPoint 服務移轉
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863499"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint 服務-移轉後工作

>適用於：Windows Server 2016

移轉至 Windows Server 2016 中的 MultiPoint 服務之後，請使用下列資訊來驗證移轉，以及執行清除步驟。

## <a name="validate-the-migration-by-running-a-pilot-program"></a>藉由執行試驗計劃驗證移轉

您可以藉由建立一個試驗專案，在生產環境中驗證您的 MultiPoint 服務移轉。 移轉的角色服務進入生產環境，以驗證您的部署如預期般運作之前，請在伺服器上執行試驗的專案。 請考慮限制在第一個，緩慢增加存取 MultiPoint 服務使用者的連線數目。

> [!NOTE] 
> 一律使用測試帳戶來測試移轉。 使用具有系統管理權限和帳戶的帳戶，有效的使用者。

## <a name="retire-the-source-server"></a>淘汰來源伺服器
驗證您的移轉之後，您可以關閉或中斷網路連線的來源伺服器。 如果伺服器是已加入網域，它從網域移除之前中斷其連線。

