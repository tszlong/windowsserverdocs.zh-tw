---
title: 新增與開發功能
description: 「系統深入解析」可讓您將新的預測功能新增至「系統深入解析」，而不需要任何作業系統更新。 這可讓開發人員（包括 Microsoft 和協力廠商）建立並提供新功能，以處理您關心的案例。 新功能可以指定要收集和分析的自訂資料，而且它們也會與現有的 System Insights 管理平面整合。
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0dd4e24197d5a8c438d70a849e435ce28792dfce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858431"
---
# <a name="adding-and-developing-new-capabilities"></a>加入和開發新功能

>適用於︰Windows Server 2019

「系統深入解析」可讓您將新的預測功能新增至「系統深入解析」，而不需要任何作業系統更新。 這可讓開發人員（包括 Microsoft 和協力廠商）建立並提供新功能，以處理您關心的案例。 

任何新功能都可以與現有的 System Insights 基礎結構整合並加以擴充：

- 新功能可以**指定任何效能計數器或系統事件**，在叫用此功能時，會在本機進行收集、保存，並傳回至分析功能。  
- 新功能可以**利用現有的 Windows 管理中心和 PowerShell 管理平面**。 在系統深入解析中，新功能不但可供探索，同時也受益于自訂排程和修復動作。 

## <a name="manage-new-capabilities"></a>管理新功能
- [瞭解](add-remove-update-capabilities.md)如何使用 PowerShell 來新增、移除及更新功能。 

## <a name="develop-a-capability"></a>開發功能
使用下列資源可協助您開始撰寫自己的自訂功能：
- [瞭解](data-sources.md)您可以收集的資料來源。
- [下載](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/)System Insights NuGet 套件，其中包含撰寫功能所需的類別和介面。
- [請流覽](https://aka.ms/systeminsights-api)API 檔，以瞭解系統深入解析的類別和介面。 
- [使用](https://aka.ms/systeminsights-samplecapability)[System Insights 範例] 功能可協助您開始著手。 這會說明如何註冊功能、指定要收集的資料來源，以及開始分析系統資料。

>[!NOTE]
>這是發行前版本功能。 因為我們新增了新功能並納入意見反應，所以可能會有所變更。

## <a name="see-also"></a>另請參閱
若要深入瞭解「系統深入解析」，請使用下列資源：

- [System Insights 總覽](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增、移除和更新功能](add-remove-update-capabilities.md)
- [System Insights 常見問題](faq.md)