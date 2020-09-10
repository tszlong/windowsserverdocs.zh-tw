---
title: openfiles
description: Openfiles 命令的參考文章，可讓系統管理員查詢、顯示或中斷系統上已開啟的檔案和目錄的連線。
ms.topic: reference
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: eb7d67d082c948c5f91d7551033143a2aac79f2a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633688"
---
# <a name="openfiles"></a>openfiles

可讓系統管理員查詢、顯示或中斷已在系統上開啟的檔案和目錄。 此命令也會啟用或停用系統 **維護物件清單** 全域旗標。

## <a name="openfiles-disconnect"></a>openfiles/disconnect

可讓系統管理員中斷透過共用資料夾從遠端開啟的檔案和資料夾。

### <a name="syntax"></a>語法

```
openfiles /disconnect [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] {[/id <openfileID>] | [/a <accessedby>] | [/o {read | write | read/write}]} [/op <openfile>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /s `<system>` | 指定要依名稱或 IP 位址) 連接 (的遠端系統。 請勿使用反斜線。 如果您未使用 **/s** 選項，則預設會在本機電腦上執行命令。 此參數會套用至命令中指定的所有檔案和資料夾。 |
| u `[<domain>\]<username>` | 使用指定之使用者帳戶的許可權來執行命令。 如果您未使用 **/u** 選項，預設會使用系統許可權。 |
| /p `[<password>]` | 指定在 **/u** 選項中指定之使用者帳戶的密碼。 如果您未使用 **/p** 選項，則會在命令執行時出現密碼提示。 |
| /id `<openfileID>` | 將開啟的檔案與指定的檔案識別碼中斷連接。 您可以使用萬用字元 (**&#42;**) 搭配此參數。<p>注意：您可以使用 **openfiles/query** 命令來尋找檔案識別碼。 |
| /a `<accessedby>` | 中斷與 *accessedby* 參數中指定之使用者名稱相關聯的所有開啟的檔案。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /o `{read | write | read/write}` | 將所有開啟的檔案與指定的開啟模式值中斷連接。 有效的值為 **讀取**、 **寫入**或 **讀取/寫入**。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /op `<openfile>` | 中斷特定開啟的檔案名所建立的所有開啟的檔案連接。 您可以使用萬用字元 (**&#42;**) 搭配此參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要將所有開啟的檔案與檔案 *識別碼 26843578*中斷連線，請輸入：

```
openfiles /disconnect /id 26843578
```

若要中斷使用者 *hiropln*存取的所有開啟的檔案和目錄，請輸入：

```
openfiles /disconnect /a hiropln
```

若要將所有開啟的檔案和目錄與 *讀取/寫入模式*中斷連線，請輸入：

```
openfiles /disconnect /o read/write
```

若要將目錄與開啟的檔案名 * C:\testshare 中斷連線， \* 不論誰存取它，請輸入：

```
openfiles /disconnect /a * /op c:\testshare\
```

若要中斷使用者*hiropln*存取的遠端電腦*srvmain*上的所有開啟的檔案，不論其識別碼為何，請輸入：

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
| /s `<system>` | 指定要依名稱或 IP 位址) 連接 (的遠端系統。 請勿使用反斜線。 如果您未使用 **/s** 選項，則預設會在本機電腦上執行命令。 此參數會套用至命令中指定的所有檔案和資料夾。 |
| u `[<domain>\]<username>` | 使用指定之使用者帳戶的許可權來執行命令。 如果您未使用 **/u** 選項，預設會使用系統許可權。 |
| /p `[<password>]` | 指定在 **/u** 選項中指定之使用者帳戶的密碼。 如果您未使用 **/p** 選項，則會在命令執行時出現密碼提示。 |
| [/fo `{TABLE | LIST | CSV}` ] | 以指定的格式顯示輸出。 有效值包括：<ul><li>**資料表** -在資料表中顯示輸出。</li><li>**清單** -顯示清單中的輸出。</li><li>**Csv** -以逗點分隔值顯示 (CSV) 格式的輸出。</li></ul> |
| /nh | 隱藏輸出中的資料行標頭。 只有當 **/fo** 參數設定為 **資料表** 或 **CSV**時，才有效。 |
| /v | 指定輸出中顯示詳細的 (詳細資訊) 資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要查詢和顯示所有開啟的檔案，請輸入：

```
openfiles /query
```

若要查詢並以不含標頭的表格格式顯示所有開啟的檔案，請輸入：

```
openfiles /query /fo table /nh
```

若要查詢並以清單格式顯示所有開啟的檔案，並提供詳細資訊，請輸入：

```
openfiles /query /fo list /v
```

若要使用*maindom*網域上的使用者*hiropln*認證來查詢及顯示遠端系統*srvmain*上的所有開啟檔案，請輸入：

```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> 在此範例中，會在命令列上提供密碼。 若要避免顯示密碼，請省略 **/p** 選項。 系統會提示您輸入密碼，而不會將其送至畫面。

## <a name="openfiles-local"></a>openfiles/local

啟用或停用系統 **維護物件清單** 全域旗標。 如果使用時不含參數， **openfiles/local** 會顯示 **維護物件清單** 全域旗標的目前狀態。

> [!NOTE]
> 除非您重新開機系統，否則使用 [ **開啟** ] 或 [ **關閉** ] 選項所做的變更不會生效。 啟用 **維護物件清單** 全域旗標可能會使系統變慢。

### <a name="syntax"></a>語法

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[on | off]` | 啟用或停用系統 **維護物件清單** 全域旗標，這會追蹤本機檔案控制代碼。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要檢查 **維護物件清單** 全域旗標的目前狀態，請輸入：

```
openfiles /local
```

依預設，會停用 **維護物件清單** 全域旗標，並顯示下列訊息： `INFO: The system global flag 'maintain objects list' is currently disabled.`

若要啟用 **維護物件清單** 全域旗標，請輸入：

```
openfiles /local on
```

啟用全域旗標時，會顯示下列訊息。 `SUCCESS: The system global flag 'maintain objects list' is enabled. This will take effect after the system is restarted.`

若要停用 **維護物件清單** 全域旗標，請輸入：

```
openfiles /local off
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
