---
title: 新增與開發功能
description: 系統深入解析可讓您將加入系統 Insights 中的新預測功能，而不需要任何 OS 更新。 這可讓開發人員，包括 Microsoft 和第三方，以建立並傳遞新的功能中間版本來解決您關心的案例。 新功能可以指定自訂資料收集和分析，而且也整合現有系統的深入解析管理平面。
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
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817479"
---
# <a name="adding-and-developing-new-capabilities"></a>新增及開發新功能

>適用於：Windows Server 2019

系統深入解析可讓您將加入系統 Insights 中的新預測功能，而不需要任何 OS 更新。 這可讓開發人員，包括 Microsoft 和第三方，以建立並傳遞新的功能中間版本來解決您關心的案例。 

任何新功能可以整合，並擴充現有的系統 Insights 基礎結構：

- 新功能可以**指定任何效能計數器或系統事件**，它將要收集、 保存在本機，並叫用此功能時，傳回用於分析的功能。  
- 新功能可以**運用現有的 Windows Admin Center 和 PowerShell 管理平面**。 不只會新功能可在系統的深入解析中，它們也可以從自訂的排程及補救動作。 

## <a name="manage-new-capabilities"></a>管理新功能
- [了解](add-remove-update-capabilities.md)如何新增、 移除及更新使用 PowerShell 的功能。 

## <a name="develop-a-capability"></a>開發功能
使用下列資源可協助您開始撰寫您自己自訂的功能：
- [了解](data-sources.md)有關您可以收集的資料來源。
- [下載](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/)系統 Insights NuGet 封裝，其中包含的類別和介面，您需要撰寫一項功能。
- [請瀏覽](https://aka.ms/systeminsights-api)API 文件，以了解系統 Insights 類別和介面。 
- [使用](https://aka.ms/systeminsights-samplecapability)系統 Insights 範例功能，協助您開始使用。 這會顯示如何註冊功能，、 指定資料來源，以收集，並開始分析系統的資料。

>[!NOTE]
>這是發行前版本功能。 它可能會變更，我們加入新的功能，並將意見。

## <a name="see-also"></a>另請參閱
若要深入了解系統的深入解析，使用下列資源：

- [系統 Insights 概觀](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [新增、 移除及更新功能](add-remove-update-capabilities.md)
- [系統 Insights 常見問題集](faq.md)