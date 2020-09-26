---
title: set
description: Set 的參考文章，可顯示、設定或移除 cmd.exe 的環境變數。
ms.topic: reference
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2f53655aa344e1770c9483e5302885734c389837
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389026"
---
# <a name="set-environment-variable"></a>設定 (環境變數) 

顯示、設定或移除 cmd.exe 的環境變數。 如果使用時不含參數，則 **設定** 會顯示目前的環境變數設定。

> [!NOTE]
> 此命令需要預設啟用的命令延伸模組。

**Set**命令也可以使用不同的參數，從 Windows 修復主控台執行。 如需詳細資訊，請參閱 [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
set [<variable>=[<string>]]
set [/p] <variable>=[<promptString>]
set /a <variable>=<expression>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<variable>` | 指定要設定或修改的環境變數。 |
| `<string>` | 指定要與指定的環境變數相關聯的字串。 |
| /p | 將的值設定 `<variable>` 為使用者所輸入的輸入行。 |
| `<promptstring>` | 指定提示使用者輸入的訊息。 此參數必須搭配 **/p** 參數使用。 |
| /a | 設定 `<string>` 為評估的數值運算式。 |
| `<expression>` | 指定數值運算式。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果已啟用命令延伸 (預設) ，而您以值執行 **設定** ，則會顯示以該值開頭的所有變數。

- 字元 `<` 、 `>` 、、 `|` `&` 和 `^` 是特殊的命令 shell 字元，而且前面必須加上 escape 字元 (`^`) 或用在 `<string>` (中，例如 "StringContaining&Symbol" ) 。 如果您使用引號來括住包含特殊字元的字串，則引號會被視為環境變數值的一部分。

- 您可以使用環境變數來控制某些批次檔和程式的行為，並控制 Windows 和 MS-DOS 子系統的顯示方式和運作方式。 **Set**命令通常會在**Autoexec nt**檔案中用來設定環境變數。

- 如果您在沒有任何參數的情況下使用 **set** 命令，則會顯示目前的環境設定。 這些設定通常包含 **COMSPEC** 和 **PATH** 環境變數，可用來協助尋找磁片上的程式。 Windows 所使用的兩個其他環境變數是 **PROMPT** 和 **DIRCMD**。

- 如果您指定和的 `<variable>` 值 `<string>` ，則會將指定的 `<variable>` 值加入至環境，並 `<string>` 與該變數相關聯。 如果變數已經存在於環境中，新的字串值就會取代舊的字串值。

- 如果您只指定一個變數和一個等號 (而沒有 `<string>` **set** 命令的) ，則 `<string>` 會清除與變數相關聯的值 (如同變數不) 。

- 如果您使用 **/a** 參數，則會以優先順序的遞減順序來支援下列運算子：

  | 運算子 | 執行的作業 |
  |--|--|
  | `( )` | 群組 |
  | `! ~ -` | 一元 |
  | `* / %` | 算術 |
  | `+ -` | 算術 |
  | `<< >>` | 邏輯移位 |
  | `&` | 位元 AND |
  | `^` | 位元排除 OR |
  | `= *= /= %= += -= &= ^=` | `= <<= >>=` |
  | `,` | 運算式分隔符號 |

- 如果您使用邏輯 (`&&` 或 `||`) 或模數 (**%**) 運算子，請將運算式字串括在引號中。 運算式中的任何非數值字串都會被視為環境變數名稱，而它們的值會在處理之前轉換成數位。 如果您指定的環境變數名稱未在目前的環境中定義，則會分配零值，讓您可以執行具有環境變數值的算術，而不使用% 來取得值。

- 如果您從命令列以外的命令列執行 **set/a** ，它會顯示運算式的最後一個值。

- 數值是十進位數，除非在十六進位數位前面加上 0 x，否則八進位數位為0。 因此，0×12等同于18，與022相同。

- 延遲的環境變數擴充支援預設為停用，但您可以使用 **cmd/v**來啟用或停用它。

- 建立批次檔時，您可以使用 **set** 來建立變數，然後以使用編號變數 **%0** 到 **%9**的相同方式來使用它們。 您也可以使用變數 **%0** 到 **%9** 作為 **set**的輸入。

- 如果您從批次檔呼叫變數值，請以百分比符號括住值 (**%**) 。 例如，如果您的 batch 程式會建立名為*波特*的環境變數，您可以在命令提示字元中輸入 **% 波特%** ，以使用與*波特*相關聯的字串做為可取代的參數。

## <a name="examples"></a>範例

若要設定名為 *TEST ^ 1*的環境變數，請輸入：

```
set testVar=test^^1
```

**Set**命令會將等號 (=) 之後的所有專案，指派給變數的值。 因此，如果您輸入 `set testVar=test^1` ，將會得到下列結果 `testVar=test^1` 。

若要設定名為 *TEST&1*的環境變數，請輸入：

```
set testVar=test^&1
```

若要設定名為 *INCLUDE* 的環境變數，以便將字串 *c:\directory* 與它相關聯，請輸入：

```
set include=c:\directory
```

然後，您可以在批次檔中使用字串 *c:\directory* ，方法是將名稱括住，並以百分比符號 (**%**) 。 例如，您可以 `dir %include%` 在批次檔中使用，以顯示與 INCLUDE 環境變數相關聯的目錄內容。 處理此命令之後，字串 c:\directory 會取代 **% include%**。

若要在 batch 程式中使用 **set** 命令以將新目錄加入 *PATH* 環境變數中，請輸入：

```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```

若要顯示以字母 *P*開頭之所有環境變數的清單，請輸入：

```
set p
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)