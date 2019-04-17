---
title: 讓 Hash 發行網域成員檔案伺服器
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 318879eae82d37f68acbc18cdb21ae5290f6d02b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>讓 Hash 發行網域成員檔案伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當您正在使用 Active Directory Domain Services (AD DS) 時，您可以使用群組原則網域以便 BranchCache hash 發行的多個檔案伺服器。 若要這樣做，您必須建立組織單位（組織單位，）將檔案伺服器新增到組織單位，建立 BranchCache hash 發行群組原則物件 (GPO)，然後設定 GPO。  
  
請查看下列主題，以便在多個檔案伺服器 hash 發行。  
  
-   [建立組織單位 BranchCache 檔案伺服器](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [將檔案伺服器移到 BranchCache 檔案伺服器單位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [建立 BranchCache Hash 發行群組原則物件](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [設定 BranchCache Hash 發行群組原則物件](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


