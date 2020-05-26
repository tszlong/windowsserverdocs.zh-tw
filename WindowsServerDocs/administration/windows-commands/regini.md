---
title: regini
description: 瞭解如何從命令提示字元或使用腳本來修改登錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 33e0dcaa59be3c1748763cce5c9979fe318b271a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820148"
---
# <a name="regini"></a>regini

從命令列或腳本修改登錄，並套用在一或多個文字檔中預設為預設值的變更。 除了修改登錄機碼的許可權之外，您還可以建立、修改或刪除登錄機碼。

如需 Regini.exe regedit.exe 用來對登錄進行變更之文字腳本檔案格式和內容的詳細資訊，請參閱[如何從命令列或腳本變更登錄值或許可權](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a)。

## <a name="syntax"></a>語法

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

#### <a name="parameters"></a>參數

|參數 |描述 |

|-m \< \\ \\ ComputerName>|指定包含要修改之登錄的遠端電腦名稱稱。 使用** \\ \\ ComputerName**格式。|
|---------------------|-|
|-h \< hivefile hiveroot>|指定要修改的本機登錄 hive。 您必須以**hivefile hiveroot**的格式指定 hive 檔案的名稱和 hive 的根目錄。|
|-i \< n>|指定要用來表示命令輸出中登錄機碼樹狀結構的縮排層級。 **Regdmp .exe**工具（以二進位格式取得登錄機碼的目前許可權）會使用四倍的縮排，因此預設值為**4**。|
|-o \< outputwidth>|指定命令輸出的寬度（以字元為單位）。 如果輸出會出現在 [命令] 視窗中，則預設值為視窗的寬度。 如果將輸出導向至檔案，預設值為**240**個字元。|
|-b|指定**regini.exe regedit.exe**的輸出與舊版的**regini.exe regedit.exe**回溯相容。 如需詳細資訊，請參閱＜備註＞一節。|
|textfiles|指定包含登錄資料的一或多個文字檔的名稱。 可以列出任何數目的 ANSI 或 Unicode 文字檔。|

## <a name="remarks"></a>備註

下列指導方針主要適用于包含使用**regini.exe regedit.exe**套用之登錄資料的文字檔內容。
-   使用分號做為行結尾的批註字元。 它必須是行中的第一個非空白的字元。
-   使用反斜線表示行接續。 此命令將會忽略反斜線的所有字元，直到下一行的第一個非空白字元為止（但不包括在內）。 如果您在反斜線前面加上一個以上的空格，則會以單一空格取代。
-   使用 [硬式] 索引標籤字元來控制縮排。 此縮排表示登錄機碼的樹狀結構;不過，這些字元會轉換成單一空格，而不論其位置為何。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)