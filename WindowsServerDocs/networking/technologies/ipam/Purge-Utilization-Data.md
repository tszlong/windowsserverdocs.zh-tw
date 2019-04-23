---
title: 清除使用資料
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
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
ms.openlocfilehash: 7b471417d4c44c22f115443f1f2dcca6f351e6f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827939"
---
# <a name="purge-utilization-data"></a>清除使用資料

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何從 IPAM 資料庫中刪除 使用量資料。  

您必須是隸屬**IPAM Administrators**，在本機電腦**系統管理員**群組或同等權限，才能執行此程序。

## <a name="to-purge-the-ipam-database"></a>若要清除 IPAM 資料庫  
1. 開啟 [伺服器管理員]，然後瀏覽至 IPAM 用戶端介面。
2. 瀏覽至下列其中一個位置：**IP 位址區塊**， **IP 位址清查**，或**IP 位址範圍群組**。  
3. 按一下 **任務**，然後按一下**清除使用量資料**。 **清除使用率資料**對話方塊隨即開啟。
4. 在 **清除所有的使用量或之前的資料**，按一下**選取日期**。
5. 選擇您想要刪除所有的資料庫記錄上和在該日期之前的日期。
6. 按一下 [確定] 。 IPAM 會刪除您所指定的所有記錄。
