---
title: 角色型存取控制
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 41e9e92bf2a6a1536d355590010589d2da4e5c0f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947994"
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



