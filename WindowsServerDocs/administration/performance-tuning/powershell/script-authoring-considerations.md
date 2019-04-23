---
title: PowerShell 指令碼的效能考量
description: 在 PowerShell 中效能的指令碼
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830369"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell 指令碼的效能考量

PowerShell 指令碼，直接利用.NET 和避免管線通常較快，慣用的 PowerShell。 慣用的 PowerShell 通常會使用 cmdlet 和 PowerShell 函式高度，通常利用管線，並放.NET 只在必要時。

>[!Note] 
> 此處所述的技巧的許多不是慣用的 PowerShell 和 PowerShell 指令碼的可讀性，可能會降低。 建議除非效能另有規定，使用慣用的 PowerShell 指令碼作者。

## <a name="suppressing-output"></a>隱藏輸出

有許多方法可避免寫入到管線的物件：

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

指派給`$null`或轉型至`[void]`大致上相當和通常應該是慣用位置的效能很重要。

```PowerShell
$arrayList.Add($item) > $null
```

檔案重新導向至`$null`是幾乎一樣好先前的替代方案，最指令碼會永遠不會注意到的差異。
根據案例中，檔案重新導向會透過導致少量的額外負荷。

```PowerShell
$arrayList.Add($item) | Out-Null
```

將傳送至`Out-Null`的額外負荷相較於替代項目。
它應該避免在效能敏感的程式碼。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

介紹的指令碼區塊，然後呼叫 （使用點來源或其他） 則指派結果`$null`是方便的技巧，以抑制指令碼的大型區塊的輸出。
這項技術大致上會執行管線，以以及`Out-Null`，應該予以避免效能敏感的指令碼中。
在此範例中的額外負荷，來自於建立和叫用先前內嵌指令碼的指令碼區塊。


## <a name="array-addition"></a>陣列加法

產生一份項目通常是使用加法運算子的陣列：

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

這可能是非常 inefficent，因為陣列是固定不變。
每次新增至陣列實際上會建立新的陣列不夠大，無法容納這兩個左、 右運算元的所有項目，然後這兩個運算元的元素複製到新的陣列。
適用於小型集合，這個額外負荷可能不重要。
如需更多，這絕對是問題。

有幾個替代項目。
如果您實際上不需要陣列，改為考慮使用 ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

如果您需要陣列，您可以使用您自己`ArrayList`，直接呼叫`ArrayList.ToArray`當您想要的陣列。
或者，您可以讓建立的 PowerShell`ArrayList`和`Array`讓您：

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

在此範例中，PowerShell 會建立`ArrayList`來容納寫入管線內陣列運算式的結果。
之前指派給`$results`，PowerShell 會將轉換`ArrayList`至`object[]`。

## <a name="processing-large-files"></a>處理大型檔案

慣用的方式來處理在 PowerShell 中的檔案可能看起來像：

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

這可以是幾乎重要性順序低於直接使用.NET Api:

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

它通常會被視為不佳的做法，來直接將輸出寫入主控台中，但許多指令碼所使用的時機， `Write-Host`。

如果您必須將許多訊息寫入主控台中，`Write-Host`可以是一個級距低於`[Console]::WriteLine()`。 不過，請注意，`[Console]::WriteLine()`是特定的主機，例如 powershell.exe 或 powershell_ise.exe-它具有不保證可以運作中的所有主機只適合的替代方案。

而不是使用`Write-Host`，請考慮使用[Write-output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1)。

