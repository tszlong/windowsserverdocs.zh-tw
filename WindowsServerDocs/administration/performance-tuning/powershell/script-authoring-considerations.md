---
title: PowerShell 腳本效能考慮
description: PowerShell 中的效能腳本
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: f5ab7fbb1c993192f4626935d2adb73fc9401250
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896256"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell 腳本效能考慮

直接利用 .NET 並避免管線的 PowerShell 腳本，通常會比慣用 PowerShell 快。 慣用 PowerShell 通常會使用 Cmdlet 和 PowerShell 函式（通常會利用管線），並只在必要時才將其拖放到 .NET 中。

>[!Note]
> 這裡所述的許多技巧並不慣用 PowerShell，而且可能會降低 PowerShell 腳本的可讀性。 除非效能規定，否則建議腳本作者使用慣用 PowerShell。

## <a name="suppressing-output"></a>隱藏輸出

有許多方法可避免將物件寫入管線：

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

指派至 `$null` 或轉換為 `[void]` 大致相等，通常在效能上很重要時應該是慣用的。

```PowerShell
$arrayList.Add($item) > $null
```

檔案重新導向與 `$null` 先前的替代專案幾乎一樣，大部分的腳本都不會注意到差異。
根據案例而定，檔案重新導向會造成一些額外負荷。

```PowerShell
$arrayList.Add($item) | Out-Null
```

相較于替代方案，管線到的會 `Out-Null` 有相當大的負擔。
應該避免在效能敏感的程式碼中。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

導入腳本區塊，並使用點 (或其他方式呼叫它，) 然後將結果指派給 `$null` 是一個方便的技術，用來隱藏大型腳本區塊的輸出。
這項技術會大致執行， `Out-Null` 而且應該在效能敏感腳本中避免進行管道。
此範例中的額外負荷來自于建立和叫用先前內嵌腳本的腳本區塊。


## <a name="array-addition"></a>陣列新增

使用具有加法運算子的陣列，通常會產生專案清單：

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

這可能非常 inefficent，因為陣列是不可變的。
陣列的每個新增實際上會建立一個足以容納左右運算元之所有專案的新陣列，然後將這兩個運算元的元素複製到新的陣列中。
針對小型集合，此額外負荷可能並不重要。
對於大型集合而言，這可能是個問題。

有幾個替代方案。
如果您不是真的需要陣列，請考慮使用 ArrayList：

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

如果您需要陣列，您可以使用自己的， `ArrayList` 並 `ArrayList.ToArray` 在想要陣列時直接呼叫。
或者，您可以讓 PowerShell `ArrayList` `Array` 為您建立和：

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

在此範例中，PowerShell 會建立 `ArrayList` 來保存在陣列運算式內寫入管線的結果。
在指派給之前 `$results` ，PowerShell 會將轉換 `ArrayList` 成 `object[]` 。

## <a name="processing-large-files"></a>處理大型檔案

在 PowerShell 中處理檔案的慣用方法可能如下所示：

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

這可能比直接使用 .NET Api 來得慢：

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

## <a name="avoid-write-host"></a>避免寫入主機

一般來說，將輸出直接寫入主控台是不好的作法，但當有意義時，許多腳本會使用 `Write-Host` 。

如果您必須將許多訊息寫入主控台，則 `Write-Host` 的順序可能會比更慢 `[Console]::WriteLine()` 。 不過，請注意， `[Console]::WriteLine()` 對於特定主機（例如 powershell.exe 或 powershell_ise.exe）來說，只是適合的替代方案，不保證在所有主機中都能正常執行。

請 `Write-Host` 考慮使用[寫入輸出](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1)，而不是使用。

