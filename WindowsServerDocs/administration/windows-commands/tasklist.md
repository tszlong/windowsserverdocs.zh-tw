---
title: Tasklist
description: 了解如何顯示在本機或遠端電腦上執行的處理程序的清單。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abb36c8c794836387af5547f3706e910dc06fa42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842999"
---
# <a name="tasklist"></a>Tasklist

顯示目前在本機電腦或遠端電腦上正在執行的處理序清單。 **Tasklist**會取代**tlist**工具。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s\<電腦 >|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u [\<網域 >\\\]\<使用者名稱 >|所指定的使用者帳戶權限執行命令*使用者名稱*或是*網域*\*UserName *。 **/u**可以指定只有當 **/s**指定。 預設值是目前登入的電腦，會發出命令之使用者的權限。|
|/p \<Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/m\<模組 >|列出與載入的 DLL 模組符合指定的模式名稱的所有工作。 如果未指定模組名稱，這個選項會顯示每項工作所載入的所有模組。|
|/svc|列出每個處理序，而不會截斷的所有服務資訊。 時才有效 **/fo**參數設為**資料表**。|
|/v|在輸出中，會顯示詳細資訊的工作資訊。 對於完整而不會截斷的詳細資訊輸出，使用 **/v**並 **/svc**在一起。|
|/fo {表格\|清單\|csv}|指定要用於輸出的格式。 有效值**表格**，**清單**，並**csv**。 輸出的預設格式**資料表**。|
|/nh|隱藏在輸出中的資料行標頭。 時才有效 **/fo**參數設為**表格**或是**csv**。|
|/fi \<Filter>|指定處理序中包含或排除的查詢的類型。 請參閱下列表格以取得有效的篩選條件名稱、 運算子和值。|
|/?|在命令提示字元顯示說明。|

### <a name="filter-names-operators-and-values"></a>篩選條件名稱、 運算子和值

|篩選器名稱|有效的運算子|有效的值|
|-----------|---------------|------------|
|狀態|eq、ne|執行 | 沒有回應 | 未知|
|IMAGENAME|eq、ne|映像名稱|
|PID|eq、ne、gt、lt、ge、le|PID 值|
|工作階段|eq、ne、gt、lt、ge、le|工作階段數目|
|工作階段名稱|eq、ne|工作階段名稱|
|CPUTIME|eq、ne、gt、lt、ge、le|CPU 時間，格式為*HH ***:*** MM ***:*** SS*，其中*公釐*並*SS*是介於 0 和 59 到*HH*是任何不帶正負號的數字|
|MEMUSAGE|eq、ne、gt、lt、ge、le|以 kb 為單位的記憶體使用量|
|USERNAME|eq、ne|任何有效的使用者名稱|
|服務|eq、ne|服務名稱|
|WINDOWTITLE|eq、ne|視窗標題|
|模組|eq、ne|DLL 名稱|

## <a name="remarks"></a>備註

當指定遠端系統時，不支援的 WINDOWTITLE 和狀態篩選器。

## <a name="BKMK_examples"></a>範例

若要列出所有的工作處理序識別碼為大於 1000，並將它們顯示在 CSV 格式中，輸入：
```
tasklist /v /fi "PID gt 1000" /fo csv
```
若要列出目前正在執行的系統處理程序，請輸入：
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
若要列出目前正在執行的所有處理程序的詳細的資訊，請輸入：
```
tasklist /v /fi "STATUS eq running"
```
若要列出所有服務資訊的處理程序的 DLL 名稱開頭是"ntdll"的"Srvmain 」 在遠端電腦上，請輸入：
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
若要列出 「 Srvmain，「 使用您目前登入的使用者帳戶的認證在遠端電腦上的處理程序中，輸入：
```
tasklist /s srvmain 
```
若要列出 「 Srvmain，「 使用 Hiropln，使用者帳戶的認證在遠端電腦上的處理程序中，輸入：
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)