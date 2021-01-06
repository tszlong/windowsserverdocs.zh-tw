---
title: 將檔案伺服器移至 BranchCache 檔案伺服器組織單位
description: 瞭解如何將 BranchCache 檔案伺服器新增至 Active Directory Domain Services (AD DS) 中 (OU) 的組織單位。
manager: brianlic
ms.topic: how-to
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: b96f417078d46ccdcc75621b24d4853a6695c69f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949694"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移至 BranchCache 檔案伺服器組織單位

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，將 BranchCache 檔案伺服器新增至 Active Directory Domain Services (AD DS) 中 (OU) 的組織單位。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

> [!NOTE]
> 在使用此程式將電腦帳戶新增至 OU 之前，您必須在 Active Directory 消費者和電腦主控台中建立 BranchCache 檔案伺服器 OU。 如需詳細資訊，請參閱 [建立 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。

### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移至 BranchCache 檔案伺服器組織單位

1.  在安裝 AD DS 的電腦上伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 Active Directory 消費者和電腦主控台隨即開啟。

2.  在 Active Directory 消費者和電腦主控台中，找出 BranchCache 檔案伺服器的電腦帳戶，按一下滑鼠左鍵選取帳戶，然後將電腦帳戶拖放到您先前建立的 BranchCache 檔案伺服器 OU 上。 例如，如果您先前已建立名為 **BranchCache 檔案伺服器** 的 OU，請將電腦帳戶拖放到 **branchcache 檔案伺服器** OU 上。

3.  針對您想要移至 OU 之網域中的每部 BranchCache 檔案伺服器，重複上述步驟。



