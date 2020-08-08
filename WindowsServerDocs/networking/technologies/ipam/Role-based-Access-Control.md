---
title: 角色型存取控制
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f79dc7ab4283de5ab1ec63861d5f543658947964
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944670"
---
# <a name="role-based-access-control"></a>角色型存取控制

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供在 IPAM 中使用角色型存取控制的相關資訊。

> [!NOTE]
> 除了本主題之外，本節也提供下列 IPAM 存取控制檔。
>
> -   [使用伺服器管理員管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)
> -   [使用 Windows PowerShell 管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)

以角色為基礎的存取控制可讓您指定各種層級的存取權限，包括 DNS 伺服器、DNS 區域和 DNS 資源記錄層級。
藉由使用以角色為基礎的存取控制，您可以指定誰對作業有細微的控制，以建立、編輯和刪除不同類型的 DNS 資源記錄。

您可以設定存取控制，讓使用者限制在下列許可權。

-   使用者只能編輯特定的 DNS 資源記錄

-   使用者可以編輯特定類型的 DNS 資源記錄，例如 PTR 或 MX

-   使用者可以編輯特定區域的 DNS 資源記錄

## <a name="see-also"></a>另請參閱
[使用伺服器管理員](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md) 
 管理角色型存取控制[使用 Windows PowerShell 管理以角色為基礎的存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) 
[管理 IPAM](Manage-IPAM.md)



