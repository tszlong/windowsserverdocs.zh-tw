---
title: PowerShell 模組撰寫考慮
description: PowerShell 模組撰寫考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 25b202e56286b7c26c3150642a656eb31a120808
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851931"
---
# <a name="powershell-module-authoring-considerations"></a>PowerShell 模組撰寫考慮

本檔包含有關如何撰寫模組以獲得最佳效能的一些指導方針。

## <a name="module-manifest-authoring"></a>模組資訊清單撰寫

不使用下列指導方針的模組資訊清單，可能會對一般 PowerShell 效能產生明顯的影響，即使該模組並未用於會話中也一樣。

命令自動探索會分析每個模組，以判斷模組匯出的命令，而這項分析可能會很耗費資源。
系統會針對每個使用者快取模組分析的結果，但在第一次執行時無法使用快取，這是容器的一般案例。
在模組分析期間，如果可以從資訊清單完全判斷匯出的命令，則可以避免模組的更昂貴分析。

### <a name="guidelines"></a>指導方針

* 在模組資訊清單中，請勿在 `AliasesToExport`、`CmdletsToExport`和 `FunctionsToExport` 專案中使用萬用字元。

* 如果模組不會匯出特定類型的命令，請藉由指定 `@()`在資訊清單中明確指定。
遺失或 `$null` 專案相當於指定萬用字元 `*`。

應盡可能避免下列情況：

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

相反地，請使用：

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>避免 CDXML

在決定如何執行模組時，有三個主要選項：

* Binary （通常C#為）
* 腳本（PowerShell）
* CDXML （XML 檔案包裝 CIM）

如果載入模組的速度很重要，則 CDXML 的順序大致上比二進位模組慢一倍。

Binary 模組的載入速度最快，因為它是在一段時間內編譯，而且可以針對每部電腦使用 NGen 來進行 JIT 編譯。

腳本模組的載入速度通常會比二進位模組慢一點，因為 PowerShell 必須在編譯和執行腳本之前先行剖析。

CDXML 模組的速度通常會比腳本模組慢，因為它必須先剖析 XML 檔案，然後再產生相當多的 PowerShell 腳本，然後進行剖析和編譯。

