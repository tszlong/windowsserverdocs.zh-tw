---
title: shutdown
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8a5170fa214d4ed639ff3b817cf949a9f44ebd6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383894"
---
# <a name="shutdown"></a>shutdown



可讓您一次針對一台本機或遠端電腦進行關機或重新啟動。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/i|顯示 [**遠端關機] 對話方塊**。 **/I**選項必須是命令後面的第一個參數。 如果指定了 **/i** ，則會忽略所有其他選項。|
|/l|立即登出目前的使用者，而不會有超時時間。 您不能將 **/l**與 **/m**或 **/t**搭配使用。|
|/s|將電腦關機。|
|/r|關機後重新開機電腦。|
|/a|中止系統關機。 只在超時期間生效。 若要使用 **/a**，您也必須使用 **/m**選項。|
|/p|只會關閉本機電腦（非遠端電腦），而不會有超時時間或警告。 您只能使用 **/p**搭配 **/d**或 **/f**。 如果您的電腦不支援電源關閉功能，則會在您使用 **/p**時關閉，但是電腦的電源仍會保持開啟。|
|/h|如果已啟用休眠，則將本機電腦置於休眠狀態。 您只能搭配使用 **/h**和 **/f**。|
|/e|可讓您記錄目的電腦上未預期關機的原因。|
|/f|強制執行應用程式關閉，而不發出警告使用者。</br>注意：使用 **/f**選項可能會造成未儲存的資料遺失。|
|/m \\ @ no__t-1 @ no__t-2ComputerName >|指定目的電腦。 不能與 **/l**選項一起使用。|
|/t \<XXX >|在重新開機或關機之前，將超時時間或延遲設定為*XXX*秒。 這會導致在本機主控台上顯示警告。 您可以指定0-600 秒。 如果您未使用 **/t**，則超時期間預設為30秒。|
|/d [p @ no__t-0u：] \<XX >： \<YY >|列出系統重新開機或關機的原因。 以下是參數值：</br>**p**表示已規劃重新開機或關機。</br>**u**表示原因是使用者定義的。</br>注意：如果未指定**p**或**u** ，則會有未規劃的重新開機或關機。</br>*XX*指定主要的原因號碼（正小於256的正整數）。</br>*YY*指定次要原因號碼（正整數小於65536）。|
|/c "@no__t 0Comment >"|可以讓您詳細註解關機的原因。 您必須先使用 **/d**選項來提供原因。 您必須將批註括在引號中。 最多可以使用 511 個字元。|
|/?|在命令提示字元中顯示說明，包括在本機電腦上定義的主要和次要原因清單。|

## <a name="remarks"></a>備註

-   使用者必須被指派 [**關閉系統**] 使用者權限，才能關閉使用 [**關機**] 命令的本機或遠端系統管理電腦。
-   使用者必須是 Administrators 群組的成員，才能在本機或遠端系統管理的電腦上標注非預期的關機。 如果目的電腦已加入網域，則 Domain Admins 群組的成員可能可以執行此程式。 如需詳細資訊，請參閱：  
    -   [預設本機群組](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [預設群組](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   如果您想要一次關閉一部以上的電腦，可以使用腳本來呼叫每部電腦的**關機**，或者可以使用**shutdown** **/I**來顯示 [遠端關機] 對話方塊。
-   如果您指定主要和次要原因代碼，您必須先在您打算使用原因的每部電腦上定義這些原因代碼。 如果目的電腦上未定義原因代碼，關機事件追蹤器就無法記錄正確的原因文字。
-   請記得使用**p：** 參數來表示已規劃關機。 省略**p：** 表示關機未計畫。 如果您輸入**p：** ，後面接著未規劃關機的原因代碼，則此命令不會執行關機。 相反地，如果您省略**p：** ，並輸入計畫關閉的原因代碼，則此命令不會執行關機。

## <a name="BKMK_examples"></a>典型

若要強制應用程式在一分鐘延遲之後關閉並重新啟動本機電腦，其原因為「應用程式：維護（已規劃）」和批註「重新設定 myapp .exe」類型：
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
若要使用相同的參數重新開機遠端電腦 \\ @ no__t-1ServerName，請輸入：
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
