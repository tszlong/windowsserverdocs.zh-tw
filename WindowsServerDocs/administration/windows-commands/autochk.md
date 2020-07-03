---
title: autochk
description: '[Autochk] 命令的參考文章，會在電腦啟動時以及 Windows Server 開始驗證檔案系統的邏輯完整性時執行。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cfb034f37f85b1e54e0cee2a8a4d128518c775a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923623"
---
# <a name="autochk"></a>autochk

在電腦啟動時，以及在 Windows Server 開始驗證檔案系統的邏輯完整性之前執行。

**Autochk.exe**是只在 NTFS 磁片上執行，且僅在 Windows Server 啟動之前的**chkdsk**版本。 無法直接從命令列執行**autochk** 。 而是在下列情況下執行**autochk** ：

- 如果您嘗試在開機磁碟區上執行**chkdsk** 。

- 如果**chkdsk**無法取得磁片區的獨佔使用。

- 如果磁片區標示為「中途」。

## <a name="remarks"></a>備註

> [!WARNING]
> 不能從命令列直接執行**autochk**命令列工具。 相反地，請使用**chkntfs**命令列工具來設定您想要在啟動時執行**autochk**的方式。
>
> - 您可以搭配 **/x**參數使用**chkntfs** ，以防止在特定磁片區或多個磁片區上執行**autochk** 。
>
> - 使用**chkntfs.exe**命令列工具搭配 **/t**參數，將 autochk 延遲從0秒變更為最多3天（259200秒）。 不過，長時間延遲表示電腦不會啟動到經過的時間，或直到您按下按鍵來取消**autochk**為止。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [chkdsk 命令](chkdsk.md)

- [chkntfs 命令](chkntfs.md)
