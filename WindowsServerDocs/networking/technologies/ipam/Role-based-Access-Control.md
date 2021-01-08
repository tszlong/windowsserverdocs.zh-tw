---
title: 角色型存取控制
description: 瞭解如何在 IPAM 中使用以角色為基礎的存取控制。
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e2a22019ee68d98f8122c79ebe8146c898164f7f
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039358"
---
# <a name="role-based-access-control"></a>角色型存取控制

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供在 IPAM 中使用角色型存取控制的相關資訊。

> [!NOTE]
> 除了本主題之外，本節也提供下列 IPAM 存取控制檔集。
>
> -   [使用伺服器管理員管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)
> -   [使用 Windows PowerShell 管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)

角色型存取控制可讓您指定各種層級的存取權限，包括 DNS 伺服器、DNS 區域和 DNS 資源記錄層級。
藉由使用角色型存取控制，您可以指定誰對作業有細微的控制，以建立、編輯及刪除不同類型的 DNS 資源記錄。

您可以設定存取控制，讓使用者只能使用下列許可權。

-   使用者只能編輯特定的 DNS 資源記錄

-   使用者可以編輯特定類型的 DNS 資源記錄，例如 PTR 或 MX

-   使用者可以編輯特定區域的 DNS 資源記錄

## <a name="see-also"></a>另請參閱
[使用伺服器管理員](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md) 
 管理角色型存取控制[使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) 
 管理角色型存取控制[管理 IPAM](Manage-IPAM.md)



