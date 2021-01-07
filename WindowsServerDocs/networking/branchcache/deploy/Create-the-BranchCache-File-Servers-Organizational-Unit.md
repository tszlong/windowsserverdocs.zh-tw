---
title: 建立 BranchCache 檔案伺服器組織單位
description: 瞭解如何在適用于 BranchCache 檔案伺服器的 Active Directory Domain Services (AD DS) 中，建立組織單位 (OU) 。
manager: brianlic
ms.topic: how-to
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: ff436166eedb4a3214361267f1d508ebe3acc6e4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950294"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>建立 BranchCache 檔案伺服器組織單位

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來建立組織單位 (OU) 在 BranchCache 檔案伺服器的 Active Directory Domain Services (AD DS) 中。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>建立 BranchCache 檔案伺服器組織單位

1.  在安裝 AD DS 的電腦上伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 Active Directory 消費者和電腦主控台隨即開啟。

2.  在 Active Directory 消費者和電腦主控台中，以滑鼠右鍵按一下您要新增 OU 的網域。 例如，如果您的網域命名為 example.com，請以滑鼠右鍵按一下 [ **example.com**]。 指向 [新增]，然後按一下 [組織單位]。 [ **新增物件-組織單位** ] 對話方塊隨即開啟。

3.  在 [ **新增物件-組織單位** ] 對話方塊的 [ **名稱**] 中，輸入新 OU 的名稱。 例如，如果您想要命名 OU BranchCache 檔案伺服器，請輸入 **BranchCache 檔案伺服器**，然後按一下 **[確定]**。



