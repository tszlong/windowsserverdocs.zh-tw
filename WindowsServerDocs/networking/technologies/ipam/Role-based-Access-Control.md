---
title: 角色型存取控制
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cf6b8418482b606c86bac77b2790e3365e80bf5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355154"
---
# <a name="role-based-access-control"></a>角色型存取控制

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供在 IPAM 中使用角色型存取控制的相關資訊。  
  
> [!NOTE]  
> 除了本主題之外，本節也提供下列 IPAM 存取控制檔。  
>   
> -   [使用伺服器管理員管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [使用 Windows PowerShell 管理以角色為基礎的存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
以角色為基礎的存取控制可讓您指定各種層級的存取權限，包括 DNS 伺服器、DNS 區域和 DNS 資源記錄層級。  
藉由使用以角色為基礎的存取控制，您可以指定誰對作業有細微的控制，以建立、編輯和刪除不同類型的 DNS 資源記錄。  
  
您可以設定存取控制，讓使用者限制在下列許可權。  
  
-   使用者只能編輯特定的 DNS 資源記錄  
  
-   使用者可以編輯特定類型的 DNS 資源記錄，例如 PTR 或 MX  
  
-   使用者可以編輯特定區域的 DNS 資源記錄  
  
## <a name="see-also"></a>另請參閱  
[使用伺服器管理員管理角色型存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[使用 Windows PowerShell 管理以角色為基礎的存取控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[管理 IPAM](Manage-IPAM.md)  
  


