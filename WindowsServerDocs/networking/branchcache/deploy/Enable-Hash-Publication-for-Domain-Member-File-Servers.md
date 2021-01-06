---
title: 啟用網域成員檔案伺服器的雜湊發行
description: 瞭解如何針對多個檔案伺服器啟用 BranchCache 雜湊發行。
manager: brianlic
ms.topic: how-to
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: f09b26b76cfdf343773767067d33a39fbd458736
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948454"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>啟用網域成員檔案伺服器的雜湊發行

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您使用 Active Directory Domain Services (AD DS) 時，可以使用網域群組原則來啟用多個檔案伺服器的 BranchCache 雜湊發行。 若要這樣做，您必須 (OU 建立組織單位) 、將檔案伺服器新增至 OU、 (GPO) 建立 BranchCache 雜湊發佈群組原則物件，然後設定 GPO。

請參閱下列主題，以啟用多個檔案伺服器的雜湊發行。

-   [建立 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [將檔案伺服器移至 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [建立 BranchCache 雜湊發行群組原則物件](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)

-   [設定 BranchCache 雜湊發行群組原則物件](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)



