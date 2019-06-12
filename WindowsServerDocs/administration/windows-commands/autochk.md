---
title: autochk
description: 適用於 Windows 命令主題**autochk** -電腦啟動時執行，並在 Windows Server 之前開始驗證檔案系統的邏輯完整性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c26d42410e5466950ede4f9aa059e315030588
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435033"
---
# <a name="autochk"></a>autochk



當電腦啟動時執行和較早版本為 Windows Server® 2008 R2 開始驗證檔案系統的邏輯完整性。

**Autochk.exe**是一份**Chkdsk**執行只能在 NTFS 磁碟上，並只在 Windows Server 2008 R2 開始之前執行。 **Autochk**無法直接從命令列執行。 相反地， **Autochk**在下列情況下執行：
-   如果您嘗試執行**Chkdsk**開機磁碟區上
-   如果**Chkdsk**無法獲得獨佔使用的磁碟區
-   如果磁碟區標示為已變更

## <a name="remarks"></a>備註

> -   [!WARNING]
>     **Autochk**命令列工具無法直接從命令列執行。 請改用**Chkntfs**命令列工具來設定您想要的方式**Autochk**在啟動時執行。
> -   您可以使用**Chkntfs**具有 **/x**參數，以防止**Autochk**從特定的磁碟區或多個磁碟區上執行。
> -   使用**Chkntfs.exe**命令列工具搭配 **/t** Autochk 延遲 0 秒從變更最多 3 天 （259,200 秒為單位） 的參數。 不過，長時間的延遲表示電腦無法啟動之前經過的時間，或直到您按任意鍵即可取消**Autochk**。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[chkntfs](chkntfs.md)

[疑難排解磁碟和檔案系統](https://go.microsoft.com/fwlink/?LinkId=4527)