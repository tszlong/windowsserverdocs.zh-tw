---
title: 將檔案伺服器移至 BranchCache 檔案伺服器組織單位
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c44fb11a2935c11474abd3b3cc39cfd0eeb34734
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962342"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移至 BranchCache 檔案伺服器組織單位

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在 Active Directory Domain Services (AD DS) 中，將 BranchCache 檔案伺服器新增至 (OU) 的組織單位。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

> [!NOTE]
> 您必須先在 [Active Directory 使用者和電腦] 主控台中建立 BranchCache 檔案伺服器 OU，才能使用此程式將電腦帳戶新增至 OU。 如需詳細資訊，請參閱[建立 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。

### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移到 BranchCache 檔案伺服器組織單位

1.  在安裝 AD DS 的電腦上，按一下伺服器管理員中的 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 [Active Directory 使用者和電腦] 主控台隨即開啟。

2.  在 [Active Directory 使用者和電腦] 主控台中，找出 BranchCache 檔案伺服器的電腦帳戶，以滑鼠左鍵按一下以選取帳戶，然後將電腦帳戶拖放到先前建立的 BranchCache 檔案伺服器 OU 上。 例如，如果您先前已建立名為**BranchCache 檔案伺服器**的 OU，請將電腦帳戶拖放到**branchcache 的 [檔案伺服器**] ou 上。

3.  針對您想要移至 OU 的網域中的每個 BranchCache 檔案伺服器，重複執行前一個步驟。



