---
title: 新增、移除和更新功能
description: 系統的深入解析可讓您建立新的功能，運用現有的資料收集與管理功能。 請務必也有新增、 移除與這些功能的更新管理的平台支援。 本主題描述高階的功能，以新增、 移除及更新系統 Insights 中的功能。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880159"
---
# <a name="adding-removing-and-updating-capabilities"></a>新增、移除和更新功能

>適用於：Windows Server 2019

系統的深入解析可讓您建立新的功能，運用現有的資料收集與管理功能。 一旦建立這些功能，不過，它是同樣重要的是您也可以新增、 移除與這些功能的更新管理的平台支援。 

本主題描述高階的功能，以新增、 移除及更新系統 Insights 中的功能。 

## <a name="adding-a-capability"></a>新增功能
系統的 application Insights 可讓您新增任何時候使用的新功能**新增 InsightsCapability** cmdlet。 **新增 InsightsCapability**要求您指定的功能名稱和功能程式庫。 功能程式庫會包含功能描述、 資料來源，並預測邏輯。

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

功能已加入至系統 Insights 之後，您可以立即叫用方法，並可以管理使用 PowerShell 或 Windows Admin Center 的功能。 

## <a name="updating-a-capability"></a>更新功能
系統 Insights 也可讓您更新功能，使用**更新 InsightsCapability** cmdlet。

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

更新功能，可讓您指定新的功能程式庫，可讓您變更的功能描述、 資料來源的資料與預測相關聯的邏輯與這項功能。 重要的是，更新功能會保留所有設定和這項功能，包括自訂的排程、 動作和歷程記錄預測結果的歷程記錄資訊。 

## <a name="removing-a-capability"></a>移除功能
您也可以移除功能中使用系統的深入解析**移除 InsightsCapability** cmdlet。 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>無法移除預設的預測功能。

永久移除的功能，會刪除的功能和所有相關聯的資訊，包括排程、 任何補救動作，以及過去的預測結果。 

>[!TIP]
>請考慮停用功能，而不是將它移除，如果您擔心永久刪除 功能與相關聯的所有資訊。 

## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [系統 Insights 概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增和開發功能](adding-and-developing-capabilities.md)
- [系統 Insights 常見問題集](faq.md)