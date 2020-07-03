---
title: autoconv
description: Autoconv 命令的參考文章，它會將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6d54f413ab9d4f680f59294a3f01c1de02db222
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923566"
---
# <a name="autoconv"></a>autoconv

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統，在執行**autochk**之後，讓現有的檔案和目錄保持不變。 轉換成 NTFS 檔案系統的磁片區無法轉換回 Fat 或 Fat32。

> [!IMPORTANT]
> 您無法從命令列執行**autoconv** 。 這只能在啟動時執行（如果是透過**convert.exe**設定）。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [autochk 命令](autochk.md)

- [轉換命令](convert.md)
