---
title: 新增、移除和更新功能
description: 系統深入解析可讓您建立利用現有資料收集和管理功能的新功能。 重要的是，您也必須具備平臺支援，才能管理這些功能的新增、移除和更新。 本主題說明在系統深入解析中新增、移除和更新功能的高階功能。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 17d31b480e013cf0276041a88a86530448071ca5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958686"
---
# <a name="adding-removing-and-updating-capabilities"></a>新增、移除和更新功能

>適用於：Windows Server 2019

系統深入解析可讓您建立利用現有資料收集和管理功能的新功能。 不過，建立這些功能之後，您也必須具備平臺支援，才能管理這些功能的新增、移除和更新。

本主題說明在系統深入解析中新增、移除和更新功能的高階功能。

## <a name="adding-a-capability"></a>新增功能
系統深入解析可讓您使用**InsightsCapability** Cmdlet，隨時新增功能。 **InsightsCapability**會要求您指定功能名稱和功能庫。 功能庫包含功能描述、資料來源和預測邏輯。

```PowerShell
Add-InsightsCapability -Name Sample capability -Library C:\SampleCapability.dll
```

將功能新增至「系統深入解析」之後，您就可以使用 PowerShell 或 Windows 系統管理中心立即叫用及管理此功能。

## <a name="updating-a-capability"></a>更新功能
System Insights 也可讓您使用**InsightsCapability** Cmdlet 來更新功能。

```PowerShell
Update-InsightsCapability -Name Sample capability -Library C:\SampleCapabilityv2.dll
```

更新功能可讓您指定新的功能庫，讓您可以變更功能描述、資料來源，以及與該功能相關聯的預測邏輯。 重要的是，更新功能會保留有關該功能的所有設定和歷程記錄資訊，包括自訂排程、動作和歷程記錄預測結果。

## <a name="removing-a-capability"></a>移除功能
您也可以使用**InsightsCapability** Cmdlet，移除系統深入解析中的功能。

```PowerShell
Remove-InsightsCapability -Name Sample capability
```
>[!NOTE]
>無法移除預設的預測功能。

移除功能會永久刪除功能和所有相關聯的資訊，包括排程、任何補救動作和過去的預測結果。

>[!TIP]
>如果您擔心要永久刪除所有與此功能相關聯的資訊，請考慮停用功能，而不要將它移除。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解「系統深入解析」，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增與開發功能](adding-and-developing-capabilities.md)
- [System Insights 常見問題](faq.md)