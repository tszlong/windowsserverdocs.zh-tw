---
title: MultiPoint 服務-遷移後工作
description: 瞭解如何驗證和關閉您的 MultiPoint 服務遷移
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 4cfd658c8d5ed6109bd18c7ebb06ce6fcf355661
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077575"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint 服務-遷移後工作

>適用於：Windows Server 2016

遷移至 Windows Server 2016 中的 MultiPoint 服務之後，請使用下列資訊來驗證遷移和執行清除步驟。

## <a name="validate-the-migration-by-running-a-pilot-program"></a>執行試驗計畫以驗證遷移

您可以在生產環境中建立試驗專案，以驗證您的 MultiPoint 服務遷移。 先在伺服器上執行試驗專案，然後再將已遷移的角色服務放入生產環境中，以確認您的部署如預期般運作。 請考慮一開始先限制連接數目，以減緩增加存取 MultiPoint 服務的使用者數目。

> [!NOTE]
> 一律使用測試帳戶來測試遷移。 使用具有系統管理許可權的帳戶和有效使用者的帳戶。

## <a name="retire-the-source-server"></a>淘汰來源伺服器
驗證您的遷移之後，您可以關閉或中斷來源伺服器與網路的連線。 如果伺服器已加入網域，請先將它從網域中移除，再中斷連線。

