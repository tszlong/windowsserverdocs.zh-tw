---
title: autoconv
description: 適用於 Windows 命令主題**autoconv** -將檔案配置表 (Fat) 與 Fat32 磁碟區的 ntfs 檔案系統。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d135da085558f12a51c8febfd72aa805e1d12f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872489"
---
# <a name="autoconv"></a>autoconv

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將檔案配置表 (Fat) 與 Fat32 磁碟區到 NTFS 檔案系統，讓現有的檔案和目錄保持不變在啟動後**autochk**執行。 轉換為 NTFS 檔案系統的磁碟區無法轉換回 Fat 或 Fat32。
## <a name="remarks"></a>備註
您無法執行**autoconv**命令列上。 這將只會執行在啟動時，如果透過設定**convert.exe**。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[autochk](autochk.md)
[轉換](convert.md)
[使用檔案系統](https://go.microsoft.com/fwlink/?LinkId=4509)
