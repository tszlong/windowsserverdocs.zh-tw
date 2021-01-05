---
title: PowerShell 腳本效能考慮
description: PowerShell 中效能的腳本
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: fc6be9ef894e7d427c6abb7d8f00e77d52610af5
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879527"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell 腳本效能考慮

直接利用 .NET 並避免管線的 PowerShell 腳本，通常比慣用 PowerShell 更快。 慣用 PowerShell 通常會使用 Cmdlet 和 PowerShell 函式，通常只會在必要時利用管線，並將其拖放到 .NET 中。

>[!Note]
> 此處所述的許多技巧並非慣用 PowerShell，而且可能會降低 PowerShell 腳本的可讀性。 建議腳本作者使用慣用 PowerShell，除非效能另有指示。

## <a name="suppressing-output"></a>隱藏輸出

有許多方法可避免將物件寫入管線：

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

指派至 `$null` 或轉換成 `[void]` 大約相等，而且通常是效能重要性的建議。

```PowerShell
$arrayList.Add($item) > $null
```

檔案重新導向與 `$null` 先前的替代方案幾乎一樣，大部分的腳本都不會注意到差異。
根據案例而定，檔案重新導向會帶來一些額外負荷。

```PowerShell
$arrayList.Add($item) | Out-Null
```

`Out-Null`相較于其他替代專案，管線會有相當大的負擔。
它應該要避免效能敏感的程式碼。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

導入腳本區塊並使用點出 (來呼叫它，) 然後將結果指派給， `$null` 是抑制大型腳本區塊輸出的便利技巧。
這項技術大致上會執行，並將管線輸送至 `Out-Null` ，而且應該避免在效能敏感的腳本中。
此範例中的額外負荷來自于建立和叫用先前內嵌腳本的腳本區塊。


## <a name="array-addition"></a>陣列加法

使用具有加法運算子的陣列，來產生專案清單通常是：

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

這可能很 inefficent，因為陣列是不可變的。
陣列中的每個加入都會實際建立一個新的陣列，足以容納左右運算元的所有元素，然後將這兩個運算元的元素複製到新的陣列中。
若為小型集合，此額外負荷可能不重要。
針對大型集合，這可能是一個問題。

有幾個替代方案。
如果您實際上不需要陣列，請考慮使用 ArrayList：

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

如果您需要陣列，您可以使用自己的， `ArrayList` 只要在 `ArrayList.ToArray` 需要陣列時呼叫。
或者，您可以讓 PowerShell `ArrayList` `Array` 為您建立和：

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

在此範例中，PowerShell 會建立， `ArrayList` 以保存寫入至陣列運算式內之管線的結果。
在指派給之前 `$results` ，PowerShell 會將轉換 `ArrayList` 成 `object[]` 。

## <a name="processing-large-files"></a>處理大型檔案

在 PowerShell 中處理檔案的慣用方式可能看起來像這樣：

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

這可能是比直接使用 .NET Api 更慢的數量級：

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>避免 Write-Host

一般來說，將輸出直接寫入主控台是很不好的作法，但是當它合理時，許多腳本都會使用 `Write-Host` 。

如果您必須將許多訊息寫入主控台， `Write-Host` 可能是比更慢的順序 `[Console]::WriteLine()` 。 不過，請注意，這 `[Console]::WriteLine()` 只是特定主機（例如 powershell.exe 或 powershell_ise.exe）的適當替代方案，並不保證會在所有主機上運作。

請 `Write-Host` 考慮使用 [寫入輸出](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1&preserve-view=true)，而不是使用。

