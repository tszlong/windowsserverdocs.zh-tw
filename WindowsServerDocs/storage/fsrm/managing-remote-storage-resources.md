---
title: 管理遠端儲存資源
description: 本文說明如何管理遠端電腦上的儲存資源
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 29870c33e17c75fe25601237d7de47302662d21f
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476172"
---
# <a name="managing-remote-storage-resources"></a>管理遠端儲存資源

> 適用於：Windows Server 2019，Windows Server 2016、 Windows Server （半年通道）、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

若要管理遠端電腦上的儲存資源，您有兩個選擇：

-   從檔案伺服器資源管理員 Microsoft<sup>®</sup> Management Console (MMC) 嵌入式管理單元 (這可接著用來管理遠端資源) 連線至遠端電腦。
-   使用隨檔案伺服器資源管理員一併安裝的命令列工具。

任一項選擇皆可讓您處理配額、檢測檔案、管理分類、排程檔案管理工作，以及使用那些遠端資源產生報告。

> [!Note]
> 檔案伺服器資源管理員可以管理本機電腦或遠端電腦上的資源，但無法兩者同時都管理。

例如，您可以：

-   使用檔案伺服器資源管理員 MMC 嵌入式管理單元連線至網域中的另一台電腦，並檢閱位於遠端電腦之磁碟區或資料夾上的儲存體使用率。
-   在本機伺服器建立配額及檔案檢測範本，然後使用的命令列工具將這些範本匯入至位於分公司的檔案伺服器。

本節包含下列主題：

-   [連線至遠端電腦](connect-to-remote-computer.md)
-   [命令列工具](command-line-tools.md)
