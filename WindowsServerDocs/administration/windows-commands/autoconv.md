---
title: autoconv
description: '**Autoconv**的 Windows 命令主題-將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bf36be6bcf3dd8f6c61c6ab0d8780ed77dd8903a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383453"
---
# <a name="autoconv"></a>autoconv

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案分配表（Fat）和 Fat32 磁片區轉換為 NTFS 檔案系統，在執行**autochk**之後，讓現有的檔案和目錄保持不變。 轉換成 NTFS 檔案系統的磁片區無法轉換回 Fat 或 Fat32。
## <a name="remarks"></a>備註
您無法在命令列上執行**autoconv** 。 只有在透過**convert**設定時，才會在啟動時執行。
## <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[autochk](autochk.md)
[轉換](convert.md)
[使用檔案系統](https://go.microsoft.com/fwlink/?LinkId=4509)
