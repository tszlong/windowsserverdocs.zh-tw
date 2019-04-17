---
title: "初始化新磁碟"
description: "本文說明如何初始化新磁碟"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>初始化新磁碟

> **適用於：**Windows 10、Windows 8.1、Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

## <a name="to-initialize-new-disks"></a>若要初始化新磁碟
1.  在 [磁碟管理] 中，以滑鼠右鍵按一下您要初始化的磁碟，然後按一下 **\[初始化磁碟\]**。

2.  在 **\[初始化磁碟\]** 對話方塊中，選取要初始化的磁碟。 您可以選取要使用主開機記錄 (MBR) 或 GUID 磁碟分割表格 (GPT) 磁碟分割樣式。

## <a name="additional-considerations"></a>其他考量

-   新的磁碟顯示為 **\[未初始化\]**。 您必須先將磁碟初始化，才能使用的磁碟。 如果您在新增磁碟後啟動 [磁碟管理]，[初始化磁碟精靈] 會出現，讓您可以初始化磁碟。

> [!NOTE]
> 新的磁碟會初始化為基本磁碟。

