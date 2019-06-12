---
title: regini
description: 了解如何修改登錄中的，從命令提示字元，或使用指令碼。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e3f8454572b662c9327aeb4783c5e9651ad2022b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441894"
---
# <a name="regini"></a>regini

修改從命令列或指令碼的登錄，並套用一或多個文字檔案中已預先設定的變更。 您可以建立、 修改或刪除登錄機碼，以及修改登錄機碼的權限。

如需 Regini.exe 用以對登錄進行變更之文字指令碼檔案的內容與格式的詳細資訊，請參閱[如何從命令列或指令碼中變更登錄值或權限](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a)。

## <a name="syntax"></a>語法

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>參數

|參數 |描述 |

|-m \< \\\\電腦名稱 >|指定要修改的登錄庫的遠端電腦名稱。 使用格式 **\\ \\ComputerName**。|
|---------------------|-|
|-h \<hivefile hiveroot >|指定要修改的本機登錄 hive。 您必須在格式中指定 hive 檔案的名稱和 hive 的根**hivefile hiveroot**。|
|-i \<n>|指定要用來代表命令輸出中的登錄機碼的樹狀結構的縮排層級。 **Regdmp.exe**工具 （也就以二進位格式取得登錄機碼的目前權限） 會使用縮排 64kb 的四個，因此預設值是**4**。|
|-o \<outputwidth>|指定命令輸出中，的寬度，以字元為單位。 如果輸出會出現在 [命令] 視窗中，預設值是視窗的寬度。 如果將輸出導向至檔案時，預設值是**240**字元。|
|-b|指定**Regini.exe**輸出會與舊版的回溯相容**Regini.exe**。 請參閱 < 備註 > 一節，如需詳細資訊。|
|textfiles|指定包含登錄資料的一或多個文字檔案的名稱。 可以列出任何數目的 ANSI 或 Unicode 文字檔案。|

## <a name="remarks"></a>備註

下列指導方針主要適用於文字檔案包含您使用套用的登錄資料的內容**Regini.exe**。
-   您可以使用分號作為行結尾註解字元。 它必須是一條線的第一個非空格字元。
-   您可以使用反斜線來表示的行接續。 此命令將會忽略反斜線 （但不是包括） 的所有字元的下一行的第一個非空格字元。 如果您包含反斜線前面有一個以上的空間時，它會取代以單一空格。
-   您可以使用永久索引標籤字元來控制縮排。 此縮排指示的登錄機碼; 的樹狀結構不過，這些字元會轉換成單一空格，不論其位置。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)