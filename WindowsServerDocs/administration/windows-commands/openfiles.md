---
title: openfiles
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83175d8529d9204c6b6d969a3db2aee2775bd0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852379"
---
# <a name="openfiles"></a>openfiles



讓系統管理員可以查詢、 顯示或中斷連線檔案和目錄的系統上已開啟。 也會啟用或停用系統維護的物件清單全域旗標。

本主題包含下列命令的相關資訊：
-   [openfiles /disconnect](#BKMK_disconnect)
-   [openfiles /query](#BKMK_query)
-   [openfiles /local](#BKMK_local)

## <a name="BKMK_disconnect"></a>openfiles /disconnect

可讓系統管理員，才能中斷檔案與開啟的檔案從遠端共用資料夾的資料夾。

### <a name="syntax"></a>語法

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s \<system >|指定連線到 （依名稱或 IP 位址） 的遠端系統。 請勿使用反斜線。 如果您不要使用 **/s**選項時，此命令預設會執行在本機電腦上。 此參數適用於所有檔案和命令中指定的資料夾。|
|/u [\<網域 >\]<UserName>|使用指定的使用者帳戶的權限執行命令。 如果您不要使用 **/u**選項，系統會使用預設的使用權限。|
|/p [\<Password>]|指定在指定的使用者帳戶的密碼 **/u**選項。 如果您不要使用 **/p**選項時，密碼會出現提示時執行命令。|
|/id \<OpenFileID>|中斷連接開啟的檔案所指定的檔案識別碼。 萬用字元 (**&#42;**) 可以搭配這個參數。</br>注意：您可以使用**openfiles /query**命令以找出檔案識別碼。|
|/a \<AccessedBy>|中斷連接所有開啟的檔案中指定的使用者名稱相關聯*開啟檔案的連線*參數。 萬用字元 (**&#42;**) 可以搭配這個參數。|
|/o {讀取\|寫入\|讀/寫}|中斷連接所有開啟的檔案，以指定的開啟模式值。 有效值為讀取、 寫入或讀取/寫入。 萬用字元 (**&#42;**) 可以搭配這個參數。|
|/op \<OpenFile>|中斷連接所有開啟的檔案連線所建立的特定開啟的檔案名稱。 萬用字元 (**&#42;**) 可以搭配這個參數。|
|/?|在命令提示字元顯示說明。|

### <a name="examples"></a>範例

若要中斷所有開啟的檔案，檔案識別碼 26843578，請輸入：
```
openfiles /disconnect /id 26843578
```
若要中斷連線的所有開啟的檔案和目錄 」 hiropln"的使用者存取，請輸入：
```
openfiles /disconnect /a hiropln
```
若要中斷連接所有開啟的檔案和目錄讀取/寫入模式中，輸入：
```
openfiles /disconnect /o read/write
```
若要中斷連線的開啟的檔案名稱的目錄"C:\TestShare\"，無論誰正在存取它，型別：
```
openfiles /disconnect /a * /op "c:\testshare\"
```
若要中斷所有開啟檔案"srvmain 」 在遠端電腦上正在存取的使用者 「"hiropln，不論其識別碼，請輸入：
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles /query

查詢並顯示所有開啟的檔案。

### <a name="syntax"></a>語法

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s \<system >|指定連線到 （依名稱或 IP 位址） 的遠端系統。 請勿使用反斜線。 如果您不要使用 **/s**選項時，此命令預設會執行在本機電腦上。 此參數適用於所有檔案和命令中指定的資料夾。|
|/u [\<網域 >\]<UserName>|使用指定的使用者帳戶的權限執行命令。 如果您不要使用 **/u**選項，系統會使用預設的使用權限。|
|/p [\<Password>]|指定在指定的使用者帳戶的密碼 **/u**選項。 如果您不要使用 **/p**選項時，密碼會出現提示時執行命令。|
|[/fo {TABLE \| LIST \| CSV}]|指定的格式顯示輸出。 有效值*格式*是：</br>資料表：輸出資料表中的顯示。</br>清單：輸出顯示在清單中。</br>CSV:以逗點分隔值格式的顯示輸出。|
|/nh|隱藏在輸出中的資料行標頭。 時才有效 **/fo**參數設為**表格**或是**CSV**。|
|/v|指定在輸出中會顯示詳細的資訊。|
|/?|在命令提示字元顯示說明。|

### <a name="examples"></a>範例

若要查詢，並顯示所有開啟的檔案，請輸入：
```
openfiles /query
```
若要查詢及不含標頭的資料表格式顯示所有開啟的檔案，請輸入：
```
openfiles /query /fo table /nh
```
若要查詢並顯示所有開啟的檔案清單格式的詳細資訊，請輸入：
```
openfiles /query /fo list /v
```
若要查詢並顯示"srvmain 」 在遠端系統上所有開啟的檔案 」 hiropln"，"maindom 」 網域上的使用者使用的認證，請輸入：
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> 在此範例中，會在命令列上提供的密碼。 若要避免顯示密碼，請省略 **/p**選項。 將會提示您輸入密碼，不會回應至畫面。

## <a name="BKMK_local"></a>openfiles /local

啟用或停用系統維護的物件清單全域旗標。 如果未指定參數，使用**openfiles /local**顯示維護的物件清單的全域旗標的目前狀態。

### <a name="syntax"></a>語法

```
openfiles /local [on | off]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[在\|關閉]|啟用或停用系統維護的物件清單全域旗標，它會追蹤本機檔案控制代碼。|
|/?|在命令提示字元顯示說明。|

### <a name="remarks"></a>備註

-   啟用維護的物件清單的全域旗標，可能會降低您的系統。
-   使用所做的變更**上**或是**關閉**選項執行不會生效，直到您重新啟動系統。

### <a name="examples"></a>範例

若要檢查的維護的物件清單的全域旗標的目前狀態，請輸入：
```
openfiles /local
```
根據預設，維護的物件清單的全域旗標停用，而且會顯示下列輸出：
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
若要啟用維護的物件清單的全域旗標，請輸入：
```
openfiles /local on
```
啟用全域旗標時，會顯示下列訊息：
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
若要停用維護的物件清單的全域旗標，請輸入：
```
openfiles /local off
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)