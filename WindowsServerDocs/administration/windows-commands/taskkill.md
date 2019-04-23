---
title: taskkill
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db264181ef8e5e3632f3312ade61183cac3fc8f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853079"
---
# <a name="taskkill"></a>taskkill

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

結束一個或多個工作或處理序。 處理序可以依處理序識別碼或映像名稱結束。 **taskkill**會取代**kill**工具。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/s \<computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u\<網域 >\\\<使用者名稱 >|所指定的使用者帳戶權限執行命令*使用者名稱*或是*網域*\\*UserName*。 **/u**可以指定只有當 **/s**指定。 預設值是目前登入的電腦，會發出命令之使用者的權限。|
|/p \<Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/fi \<Filter>|將篩選套用至選取的一組工作。 您可以使用一個以上的篩選器，或使用萬用字元 (**\***) 來指定所有的工作或映像名稱。 請參閱下面的[有效的篩選條件名稱的資料表](#BKMK_table)，運算子和值。|
|/pid \<ProcessID>|指定要終止之處理序的處理序識別碼。|
|/im \<ImageName>|指定要終止之處理序的映像名稱。 使用萬用字元 (**\***) 來指定所有映像名稱。|
|/f|指定強制終止處理序。 遠端處理程序; 會忽略這個參數所有的遠端程序會強制終止。|
|/t|終止指定的處理序和任何子處理序啟動它。|

#### <a name="BKMK_table"></a>篩選條件名稱、 運算子和值
|篩選器名稱|有效的運算子|有效的值|
|--------|----------|----------|
|STatUS|eq、ne|RUNNING &#124; NOT RESPONDING &#124; UNKNOWN|
|IMAGENAME|eq、ne|映像名稱|
|PID|eq、ne、gt、lt、ge、le|PID 值|
|工作階段|eq、ne、gt、lt、ge、le|工作階段數目|
|CPUtime|eq、ne、gt、lt、ge、le|CPU 時間，格式為*HH ***:*** MM ***:*** SS*，其中*公釐*並*SS*是介於 0 和 59 到*HH*是任何不帶正負號的數字|
|MEMUSAGE|eq、ne、gt、lt、ge、le|以 kb 為單位的記憶體使用量|
|USERNAME|eq、ne|任何有效的使用者名稱 (*使用者*或是*網域*\\*使用者*)|
|服務|eq、ne|服務名稱|
|WINDOWTITLE|eq、ne|視窗標題|
|模組|eq、ne|DLL 名稱|

## <a name="remarks"></a>備註
* 當指定遠端系統時，不支援的 WINDOWTITLE 和狀態篩選器。
* 萬用字元 (**\***) 會接受 **/im**選項只有在已套用篩選時，才。
* 遠端處理序終止時永遠執行強制，無論 **/f**指定選項。
* 提供電腦名稱，主機名稱篩選條件會導致關閉，停止所有處理程序。
* 您可以使用**tasklist**來判斷處理序識別碼 (PID) 處理序終止。

## <a name="examples"></a>範例
若要結束的處理程序，與處理序識別碼 1230年，1241 和 1253年類型：
```
taskkill /pid 1230 /pid 1241 /pid 1253
```
若要強制結束處理程序 「 Notepad.exe"，如果它已由系統啟動，請輸入：
```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```
若要結束"Srvmain 」 與 「 請注意，「 映像名稱開頭的遠端電腦上的所有處理程序時使用的認證的使用者帳戶 Hiropln，輸入：
```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```
若要結束處理序識別碼 2134年與任何子處理程序的程序，它已啟動，但只有當這些處理序啟動的系統管理員帳戶中，輸入：
```
taskkill /pid 2134 /t /fi "username eq administrator"
```
若要結束所有處理程序的處理序識別碼大於或等於 1000，不論其映像名稱，輸入：
```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
