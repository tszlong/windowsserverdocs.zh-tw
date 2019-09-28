---
title: MultiPoint 服務-遷移後工作
description: 瞭解如何驗證和關閉您的 MultiPoint 服務遷移
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: 3102a442b4668856050f603f30f57f6bbed20654
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389024"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint 服務-遷移後工作

>適用於：Windows Server 2016

在您遷移至 Windows Server 2016 中的 MultiPoint 服務之後，請使用下列資訊來驗證遷移和執行清理步驟。

## <a name="validate-the-migration-by-running-a-pilot-program"></a>藉由執行試驗計畫來驗證遷移

您可以在生產環境中建立試驗專案，以驗證您的 MultiPoint 服務遷移。 將遷移的角色服務放入生產環境之前，先在伺服器上執行試驗專案，以確認您的部署如預期般運作。 請考慮先限制連線數目，慢慢增加存取 MultiPoint 服務的使用者數目。

> [!NOTE] 
> 請一律使用測試帳戶來測試遷移。 使用具有系統管理許可權的帳戶，以及有效使用者的帳戶。

## <a name="retire-the-source-server"></a>淘汰來源伺服器
在驗證您的遷移之後，您可以關閉或中斷來源伺服器與網路的連線。 如果伺服器已加入網域，請先將它從網域中移除，再中斷其連線。

