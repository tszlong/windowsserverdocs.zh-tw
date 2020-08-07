---
title: openfiles
description: Openfiles 命令的參考文章，可讓系統管理員查詢、顯示或中斷已在系統上開啟的檔案和目錄。
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79163a717746cfb43d0195cf30c7bbf7e1766623
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885123"
---
# <a name="openfiles"></a>openfiles

讓系統管理員能夠查詢、顯示或中斷已在系統上開啟的檔案和目錄。 此命令也會啟用或停用 [系統**維護物件清單**] 全域旗標。

## <a name="openfiles-disconnect"></a>openfiles/disconnect

可讓系統管理員透過共用資料夾中斷遠端開啟的檔案和資料夾的連線。

### <a name="syntax"></a>語法

```
openfiles /disconnect [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] {[/id <openfileID>] | [/a <accessedby>] | [/o {read | write | read/write}]} [/op <openfile>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /s`<system>` | 指定要依名稱或 IP 位址) 連接 (的遠端系統。 請勿使用反斜線。 如果您不使用 **/s**選項，預設會在本機電腦上執行命令。 這個參數會套用至命令中指定的所有檔案和資料夾。 |
| u`[<domain>\]<username>` | 使用指定之使用者帳戶的許可權來執行命令。 如果您不使用 **/u**選項，預設會使用系統許可權。 |
| /p`[<password>]` | 指定 **/u**選項中所指定使用者帳戶的密碼。 如果您未使用 **/p**選項，則會在命令執行時顯示密碼提示。 |
| /id`<openfileID>` | 將開啟的檔案與指定的檔案識別碼中斷連接。 您可以使用萬用字元 (**&#42;**) 搭配此參數。<p>注意：您可以使用**openfiles/query**命令來尋找檔案識別碼。 |
| /a`<accessedby>` | 中斷與*accessedby*參數中指定的使用者名稱相關聯的所有已開啟檔案。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /o`{read | write | read/write}` | 中斷所有開啟的檔案與指定的開啟模式值的連線。 有效值為「**讀取**」、「**寫入**」或「**讀取/寫入**」。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /op`<openfile>` | 中斷特定開啟的檔案名所建立的所有開啟的檔案連接。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將所有開啟的檔案與檔案*識別碼 26843578*中斷連線，請輸入：

```
openfiles /disconnect /id 26843578
```

若要中斷使用者*hiropln*存取的所有已開啟檔案和目錄，請輸入：

```
openfiles /disconnect /a hiropln
```

若要中斷所有開啟的檔案和目錄與*讀取/寫入模式*的連線，請輸入：

```
openfiles /disconnect /o read/write
```

若要將目錄與開啟的檔案名 * C:\testshare 中斷連接，無論存取的物件為何 \* ，請輸入：

```
openfiles /disconnect /a * /op c:\testshare\
```

若要中斷使用者*hiropln*存取的遠端電腦*srvmain*上所有開啟的檔案，不論其識別碼為何，請輸入：

```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="openfiles-query"></a>openfiles/query

查詢並顯示所有開啟的檔案。

### <a name="syntax"></a>語法

```
openfiles /query [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

#### <a name="parameters"></a>參數


| 參數 | 描述 |
|--|--|
| /s`<system>` | 指定要依名稱或 IP 位址) 連接 (的遠端系統。 請勿使用反斜線。 如果您不使用 **/s**選項，預設會在本機電腦上執行命令。 這個參數會套用至命令中指定的所有檔案和資料夾。 |
| u`[<domain>\]<username>` | 使用指定之使用者帳戶的許可權來執行命令。 如果您不使用 **/u**選項，預設會使用系統許可權。 |
| /p`[<password>]` | 指定 **/u**選項中所指定使用者帳戶的密碼。 如果您未使用 **/p**選項，則會在命令執行時顯示密碼提示。 |
| [/fo `{TABLE | LIST | CSV}` ] | 以指定的格式顯示輸出。 有效值包括：<ul><li>**資料表**-顯示資料表中的輸出。</li><li>**清單**-顯示清單中的輸出。</li><li>**Csv** -以逗號分隔值顯示 (CSV) 格式的輸出。</li></ul> |
| /nh | 隱藏輸出中的資料行標頭。 只有當 **/fo**參數設定為**TABLE**或**CSV**時，才有效。 |
| /v | 指定在輸出中顯示詳細 (詳細資料) 資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要查詢並顯示所有開啟的檔案，請輸入：

```
openfiles /query
```

若要在沒有標頭的情況下查詢並顯示資料表格式的所有開啟檔案，請輸入：

```
openfiles /query /fo table /nh
```

若要以詳細資訊查詢並顯示清單格式的所有開啟檔案，請輸入：

```
openfiles /query /fo list /v
```

若要使用*maindom*網域上的使用者*hiropln*認證，在遠端系統*srvmain*上查詢和顯示所有開啟的檔案，請輸入：

```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> 在此範例中，會在命令列上提供密碼。 若要避免顯示密碼，請省略 **/p**選項。 系統會提示您輸入密碼，這不會送到螢幕上。

## <a name="openfiles-local"></a>openfiles/local

啟用或停用 [系統**維護物件清單**] 全域旗標。 如果在沒有參數的情況下使用， **openfiles/local**會顯示 [**維護物件清單**] 全域旗標的目前狀態。

> [!NOTE]
> 使用 [**開啟**] 或 [**關閉**] 選項所做的變更，必須等到您重新開機系統後才會生效。 啟用 [**維護物件清單**] 全域旗標可能會讓您的系統變慢。

### <a name="syntax"></a>語法

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[on | off]` | 啟用或停用 [系統**維護物件清單**] 全域旗標，它會追蹤本機檔案控制代碼。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要檢查 [**維護物件清單**] 全域旗標的目前狀態，請輸入：

```
openfiles /local
```

預設會停用 [**維護物件清單**] 全域旗標，並顯示下列訊息：`INFO: The system global flag 'maintain objects list' is currently disabled.`

若要啟用 [**維護物件清單**] 全域旗標，請輸入：

```
openfiles /local on
```

啟用全域旗標時，會出現下列訊息：`SUCCESS: The system global flag 'maintain objects list' is enabled. This will take effect after the system is restarted.`

若要停用 [**維護物件清單**] 全域旗標，請輸入：

```
openfiles /local off
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
