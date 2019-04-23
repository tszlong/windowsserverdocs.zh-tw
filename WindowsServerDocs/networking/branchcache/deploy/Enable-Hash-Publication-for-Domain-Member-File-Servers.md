---
title: 啟用網域成員檔案伺服器的雜湊發行
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 174e83c950d2aff8afba4f05641a74861b9a7938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865459"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>啟用網域成員檔案伺服器的雜湊發行

>適用於：Windows Server （半年通道），Windows Server 2016

當您使用 Active Directory 網域服務 (AD DS) 時，您可以使用網域群組原則，若要啟用 BranchCache 的雜湊發行集的多個檔案伺服器。 若要這樣做，您必須建立組織單位 (OU)，將檔案伺服器新增到 OU，建立 BranchCache 的雜湊發行群組原則物件 (GPO)，然後設定 GPO。  
  
請參閱下列主題，以啟用多個檔案伺服器的雜湊發行。  
  
-   [建立 BranchCache 的檔案伺服器的組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [將檔案伺服器移至 BranchCache 的檔案伺服器的組織單位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [建立 BranchCache 的雜湊發行集的群組原則物件](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [設定 BranchCache 的雜湊發行集的群組原則物件](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


