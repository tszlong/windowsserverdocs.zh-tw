---
title: regini
description: Regini.exe regedit.exe 命令的參考文章，此命令會從命令列或腳本修改登錄，並套用在一或多個文字檔中預設為預設值的變更。
ms.topic: reference
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a779c41dba46e86f862982de0b203a09dd6c8384
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027436"
---
# <a name="regini"></a>regini

從命令列或腳本修改登錄，並套用一或多個文字檔中預設的變更。 除了修改登錄機碼的許可權之外，您還可以建立、修改或刪除登錄機碼。

如需 regini.exe 用來變更登錄之文字腳本檔格式和內容的詳細資訊，請參閱 [如何從命令列或腳本變更登錄值或許可權](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a)。

## <a name="syntax"></a>語法

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputwidth][-b] textfiles...
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -m `<\\computername>` | 指定包含要修改之登錄的遠端電腦名稱稱。 使用** \\ ComputerName**格式。 |
| -h `<hivefile hiveroot>` | 指定要修改的本機登錄區。 您必須以 **hivefile hiveroot**的格式，指定 hive 檔案的名稱和 hive 的根目錄。 |
| -i `<n>` | 指定在命令輸出中用來表示登錄機碼樹狀結構的縮排層級。 **regdmp.exe**工具 (以二進位格式取得登錄機碼的目前許可權) 使用縮排（以四的倍數表示），因此預設值為**4**。 |
| -o `<outputwidth>` | 指定命令輸出的寬度（以字元為單位）。 如果輸出會出現在命令視窗中，則預設值為視窗的寬度。 如果輸出被導向至檔案，則預設值為 **240** 個字元。 |
| -b | 指定 **regini.exe** 輸出與舊版 **regini.exe**回溯相容。 |
| textfiles | 指定包含登錄資料的一或多個文字檔的名稱。 可以列出任何數目的 ANSI 或 Unicode 文字檔。 |

#### <a name="remarks"></a>備註

下列指導方針主要適用于包含您使用 **regini.exe**所套用之登錄資料的文字檔內容。

- 使用分號做為行尾批註字元。 它必須是行中的第一個非空白字元。

- 使用反斜線來指出行接續。 此命令會忽略反斜線到 (的所有字元，但不包括) 下一行的第一個非空白字元。 如果您在反斜線之前包含一個以上的空格，則會以單一空格取代。

- 使用固定制表符來控制縮排。 此縮排表示登錄機碼的樹狀結構。但是，不論位置為何，這些字元都會轉換成單一空格。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
