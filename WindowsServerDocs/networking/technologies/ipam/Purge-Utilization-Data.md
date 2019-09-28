---
title: 清除使用資料
description: 本主題是 Windows Server 2016 中 IP 位址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9e1db31e4d2d714c358f2a67c2165aef91b314ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405610"
---
# <a name="purge-utilization-data"></a>清除使用資料

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何刪除 IPAM 資料庫中的使用量資料。  

您必須是**IPAM 系統管理員**、本機電腦**管理員**群組或同等許可權的成員，才能執行此程式。

## <a name="to-purge-the-ipam-database"></a>清除 IPAM 資料庫  
1. 開啟伺服器管理員，然後流覽至 IPAM 用戶端介面。
2. 瀏覽至下列其中一個位置：**Ip 位址區塊**、 **ip 位址清查**或**ip 位址範圍群組**。  
3. 按一下 [工作]，**然後按一下** **[清除使用量資料**]。 [**清除使用量資料**] 對話方塊隨即開啟。
4. 在 **[清除或之前的所有使用量資料**] 中，按一下 [**選取日期**]。
5. 選擇您想要在該日期之前和之前刪除所有資料庫記錄的日期。
6. 按一下 [確定]。 IPAM 會刪除您指定的所有記錄。
