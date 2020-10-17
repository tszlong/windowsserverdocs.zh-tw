---
title: waitfor
description: Waitfor 命令的參考文章，此命令會傳送或等候系統上的信號。
ms.topic: reference
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 157861a02abe8fbf851a6ef12460b257a7628803
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156121"
---
# <a name="waitfor"></a>waitfor

傳送或等候系統上的信號。 此命令是用來同步處理網路上的電腦。

## <a name="syntax"></a>語法

```
waitfor [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] /si <signalname>
waitfor [/t <timeout>] <signalname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 此參數會套用至命令中指定的所有檔案和資料夾。 如果您未使用此參數，則會將信號廣播到網域中的所有系統。 如果您使用此參數，則信號只會傳送到指定的系統。 |
| u `[<domain>]<user>` | 使用指定之使用者帳戶的認證來執行腳本。 根據預設， **waitfor** 會使用目前使用者的認證。 |
| /p `[\<password>]` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| /si | 將指定的信號傳送到網路上。 此參數也可讓您手動啟動信號。 |
| 一起 `<timeout>` | 指定等候信號的秒數。 **Waitfor**預設會無限期等候。 |
| `<signalname>` | 指定 **waitfor** 等候或傳送的信號。 此參數不區分大小寫，且不能超過225個字元。 有效字元包括 a-z、a-z、0-9 和 ASCII 擴充字元集 (128-255) 。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您可以在單一電腦上執行多個 **waitfor** 實例，但是 **waitfor** 的每個實例都必須等待不同的信號。 只有一個 **waitfor** 實例可以在指定的電腦上等候指定的信號。

- 如果電腦與傳送信號的電腦位於相同的網域，則只會收到信號。

- 當您測試軟體組建時，可以使用此命令。 例如，編譯電腦可以在編譯成功完成之後，將信號傳送到執行 **waitfor** 的數部電腦。 收到信號時，包含 **waitfor** 的批次檔可以指示電腦在編譯的組建上立即開始安裝軟體或執行測試。

## <a name="examples"></a>範例

若要等到收到 *espresso\build007* 信號，請輸入：

```
waitfor espresso\build007
```

根據預設， **waitfor** 會無限期等候信號。

若要等候 *10 秒* ，讓 *espresso\compile007* 信號在超時前被接收，請輸入：

```
waitfor /t 10 espresso\build007
```

若要手動啟用 *espresso\build007* 信號，請輸入：

```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
