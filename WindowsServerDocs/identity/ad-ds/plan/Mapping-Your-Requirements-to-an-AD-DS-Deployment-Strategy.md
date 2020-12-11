---
description: 深入瞭解：將您的需求對應至 AD DS 部署策略
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: 將您的需求和 AD DS 部署策略對應
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e143b3aa277baead63f321195963f220cdc16953
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042746"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>將您的需求和 AD DS 部署策略對應

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在您完成審核並識別 Active Directory Domain Services (AD DS) 設計和部署需求，並判斷哪些是與特定部署有關的專案之後，您可以將這些需求對應至特定的 AD DS 部署策略。

使用下表來判斷哪一個 AD DS 部署策略對應至組織 AD DS 設計和部署需求的適當組合。  ( 「是」表示您的部署策略必須有特定需求;「否」表示您的部署策略不需要特定需求。 ) 

此表只參考本指南中所述的三個主要 AD DS 部署策略：

-   [在新組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)

-   [在 Windows Server 2003 組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)

-   [在 Windows Server 2000 組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)

不過，您可以使用 AD DS 設計和部署需求的任意組合來建立混合式或自訂 AD DS 部署策略，以符合組織的需求。

| AD DS 的設計和部署需求 | 在新組織中部署 AD DS | 在 Windows Server 2003 組織中部署 AD DS | 在 Windows Server 2000 組織中部署 AD DS |
| ---------------------------------------- | ------------------------------------- | ----------------------------------------------------- |----------------------------------------------- |
| [設計 Windows Server 2008 AD DS 的邏輯結構](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770806(v=ws.10)) | 是 | 是 | 是 |
| [設計 Windows Server 2008 AD DS 的站台拓撲](Designing-the-Site-Topology.md) | 是 | 是 | 是 |
| 規劃網域控制站容量 | 是 | 是 | 是 |
| [部署 Windows Server 2008 樹系根網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)) | 是 | 否 | 否 |
| [部署 Windows Server 2008 地區網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755118(v=ws.10)) | 是 | 是 | 是 |
| [啟用 AD DS 的進階功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md) | 是 |是，但是環境中的所有網域控制站都必須執行 Windows Server 2008，您才能將網域或樹系功能等級設定為 Windows Server 2008。 | 是，但是環境中的所有網域控制站都必須執行 Windows Server 2008，您才能將網域或樹系功能等級設定為 Windows Server 2008。 |
| [將 Active Directory 網域升級至 Windows Server 2008 和 Windows Server 2008 R2 AD DS 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731188(v=ws.10)) | 否 | 是 | 是 |
| [ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)) | 可以，如果您想要將試驗網域遷移到生產環境，請與另一個組織合併，並將這兩項資訊技術整合 (IT) 基礎結構，或合併您從 Windows 2000 或 Windows Server 2003 環境就地升級的資源和帳戶網域。 | 可以，如果您想要與另一個組織合併，併合並這兩個 IT 基礎結構，或合併您從 Windows 2000 或 Windows Server 2003 環境就地升級的資源和帳戶網域。 | 可以，如果您想要與另一個組織合併，併合並這兩個 IT 基礎結構，或合併您從 Windows 2000 或 Windows Server 2003 環境就地升級的資源和帳戶網域。 |
| [ADMT 指南：遷移和重建 Active Directory 網域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc974332(v=ws.10)) | 否 | 是的，如果您需要減少網域數目，請減少複寫流量和所需的使用者和群組管理量，或是簡化群組原則的管理工作。 | 是的，如果您需要減少網域數目，請減少複寫流量和所需的使用者和群組管理量，或是簡化群組原則的管理工作。 |
