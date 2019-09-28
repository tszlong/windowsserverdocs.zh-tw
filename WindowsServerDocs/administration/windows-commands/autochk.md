---
title: autochk
description: 適用于**autochk**的 Windows 命令主題-在電腦啟動時，以及在 Windows Server 開始驗證檔案系統的邏輯完整性之前執行。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 76e54d14879cefd4661a1ca7f1c3b8ee7ec58de2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382348"
---
# <a name="autochk"></a>autochk



會在電腦啟動時執行，且在 Windows Server® 2008 R2 之前，會開始確認檔案系統的邏輯完整性。

**Autochk**是只在 NTFS 磁片上執行的**Chkdsk**版本，而且只有在 Windows Server 2008 R2 啟動之前。 無法直接從命令列執行**Autochk** 。 而是在下列情況下執行**Autochk** ：
-   如果您嘗試在開機磁碟區上執行**Chkdsk**
-   如果**Chkdsk**無法取得磁片區的獨佔使用
-   如果磁片區標示為已變更

## <a name="remarks"></a>備註

> -   [!WARNING]
>     不能從命令列直接執行**Autochk**命令列工具。 相反地，請使用**Chkntfs**命令列工具來設定您想要在啟動時執行**Autochk**的方式。
> -   您可以搭配 **/x**參數使用**Chkntfs** ，以防止在特定磁片區或多個磁片區上執行**Autochk** 。
> -   使用**Chkntfs**命令列工具搭配 **/t**參數，將 Autochk 延遲從0秒變更為最多3天（259200秒）。 不過，長時間延遲表示電腦不會啟動到經過的時間，或直到您按下按鍵來取消**Autochk**為止。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[磁片和檔案系統的疑難排解](https://go.microsoft.com/fwlink/?LinkId=4527)