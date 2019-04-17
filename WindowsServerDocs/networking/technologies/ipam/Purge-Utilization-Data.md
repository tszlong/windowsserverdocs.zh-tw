---
title: 清除資料使用量
description: 本主題是在 Windows Server 2016 的 IP 位址管理 (IPAM) 管理組節目表的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>清除資料使用量

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何從 IPAM 資料庫 delete 使用量資料。  

您必須成員的**IPAM 系統管理員**，在本機電腦**系統管理員**群組中或等執行這個程序。

## <a name="to-purge-the-ipam-database"></a>若要清除 IPAM 資料庫  
1. 打開伺服器管理員]，然後瀏覽 IPAM client 介面。
2. 在下列位置的其中一個瀏覽：**IP 位址區塊**，**IP 位址庫存**，或**IP 位址範圍群組**。  
3. 按一下**工作**，然後按一下 [**清除使用量資料**。 **清除使用量資料**對話方塊。
4. 在**清除所有使用量資料或之前**，按一下 [**選取 [日期]**。
5. 選擇您要 delete 所有身份上與該日之前的日期。
6. 按一下**[確定]**。 IPAM 刪除所指定的所有記錄。
