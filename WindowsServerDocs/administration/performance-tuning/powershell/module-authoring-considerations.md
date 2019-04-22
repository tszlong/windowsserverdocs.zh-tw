---
title: 撰寫考量的 PowerShell 模組
description: 撰寫考量的 PowerShell 模組
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818719"
---
# <a name="powershell-module-authoring-considerations"></a>撰寫考量的 PowerShell 模組

本文件包含一些模組以獲得最佳效能的撰寫方式與相關的指導方針。

## <a name="module-manifest-authoring"></a>模組資訊清單撰寫

不會使用下列指導方針的模組資訊清單可以有明顯影響一般的 PowerShell 效能，即使模組不會用於工作階段。

命令自動搜索會分析每個模組，以判斷哪些命令模組匯出與這項分析可能會耗費資源。
每位使用者的快取模組分析的結果，但快取無法使用第一次執行，這是典型的案例，使用容器。
模組在分析期間，如果匯出的命令可完全由資訊清單中，您可以避免昂貴的分析的模組。

### <a name="guidelines"></a>指導方針

* 在模組資訊清單中，不會使用在中的使用萬用字元`AliasesToExport`， `CmdletsToExport`，和`FunctionsToExport`項目。

* 如果模組不會匯出為特定類型的命令，這明確指定資訊清單中指定`@()`。
遺漏或`$null`項目相當於指定萬用字元`*`。

下列應該盡量避免：

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

相反地，使用：

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Avoid CDXML

在決定如何實作您的模組時，有三個主要選項：

* 二進位 (通常是C#)
* 指令碼 (PowerShell)
* CDXML （包裝 CIM 是 XML 檔案）

如果載入模組的速度很重要，但 CDXML 大約是一個級距低於二進位模組。

二進位模組載入速度最快，因為它會預先編譯，JIT 編譯一次每台機器可以使用 NGen。

指令碼模組通常會是二進位模組比更慢載入，因為 PowerShell 必須剖析的指令碼，然後再編譯並執行它。

CDXML 模組是通常比慢很多指令碼模組，因為它必須先剖析接著會產生相當多的 PowerShell 指令碼，然後在剖析和編譯的 XML 檔案。

