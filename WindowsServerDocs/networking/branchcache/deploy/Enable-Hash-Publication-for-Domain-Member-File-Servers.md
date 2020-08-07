---
title: 啟用網域成員檔案伺服器的雜湊發行
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: df03945a80ae86aad91a004ea710ac6eff10c3ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971855"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>啟用網域成員檔案伺服器的雜湊發行

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您使用 Active Directory Domain Services (AD DS) 時，您可以使用網域群組原則來啟用多個檔案伺服器的 BranchCache 雜湊發行。 若要這樣做，您必須建立組織單位 (OU) 、將檔案伺服器新增到 OU、建立 BranchCache 雜湊發行集群組原則物件 (GPO) ，然後設定 GPO。

請參閱下列主題，以啟用多個檔案伺服器的雜湊發行。

-   [建立 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [將檔案伺服器移至 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [建立 BranchCache 雜湊發行群組原則物件](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)

-   [設定 BranchCache 雜湊發行群組原則物件](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)



