---
title: 新增與開發功能
description: 系統深入解析可讓您將新的預測功能新增至系統深入解析，而不需要任何作業系統更新。 這可讓開發人員（包括 Microsoft 和協力廠商）建立並提供新功能的新功能，以解決您所關心的案例。 新的功能可以指定要收集和分析的自訂資料，也可以整合現有的系統深入解析管理平面。
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: d85016fd3f338ab87d6516815ee04c652875bdf5
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766761"
---
# <a name="adding-and-developing-new-capabilities"></a>加入和開發新功能

>適用於：Windows Server 2019

系統深入解析可讓您將新的預測功能新增至系統深入解析，而不需要任何作業系統更新。 這可讓開發人員（包括 Microsoft 和協力廠商）建立並提供新功能的新功能，以解決您所關心的案例。

任何新功能都可以整合並擴充現有的系統深入解析基礎結構：

- 新功能可指定在叫用功能時，將收集的 **任何效能計數器或系統事件**、保存在本機，並傳回給分析功能。
- 新功能可以 **利用現有的 Windows Admin Center 和 PowerShell 管理平面**。 在系統深入解析中，不僅可探索新功能，還可受益于自訂排程和補救動作。

## <a name="manage-new-capabilities"></a>管理新功能
- [瞭解](add-remove-update-capabilities.md) 如何使用 PowerShell 來新增、移除及更新功能。

## <a name="develop-a-capability"></a>開發功能
使用下列資源可協助您開始撰寫自己的自訂功能：
- [瞭解](data-sources.md) 您可以收集的資料來源。
- [下載](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) System Insights NuGet 封裝，其中包含您撰寫功能所需的類別和介面。
- [請造訪](/dotnet/api/microsoft.systeminsights.capability) API 檔以瞭解系統深入解析類別和介面。
- [使用](https://aka.ms/systeminsights-samplecapability) 「系統深入解析範例」功能來協助您開始著手。 這會說明如何註冊功能、指定要收集的資料來源，以及開始分析系統資料。

>[!NOTE]
>這是發行前版本功能。 由於我們新增了新功能並納入意見反應，因此可能會有所變更。

## <a name="additional-references"></a>其他參考資料
若要深入瞭解系統深入解析，請使用下列資源：

- [系統深入解析概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增、移除和更新功能](add-remove-update-capabilities.md)
- [系統深入解析常見問題](faq.md)