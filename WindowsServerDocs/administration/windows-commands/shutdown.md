---
title: shutdown
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837249"
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
|/i|顯示**遠端關機對話方塊** 方塊中。 **/I**選項必須是這個命令後面的第一個參數。 如果 **/i**指定，則會忽略所有其他選項。|
|/l|登出目前的使用者立即使用沒有逾時期限。 您無法使用 **/l**具有 **/m**或是 **/t**。|
|/s|關閉電腦。|
|/r|在關機後，重新啟動電腦。|
|/a|中止系統關機。 只有在逾時期間有效。 若要使用 **/a**，您也必須使用 **/m**選項。|
|/p|關閉 本機電腦只 （而不是遠端電腦），不含逾時期限或警告。 您可以使用 **/p**只能搭配 **/d**或是 **/f**。 如果您的電腦不支援電源關閉功能，它就會關閉當您使用 **/p**，但上仍會留在電腦的電源。|
|/h|如果啟用休眠，請進入休眠狀態，將本機電腦。 您可以使用 **/h**只能搭配 **/f**。|
|/e|可讓您能夠記錄在目標電腦上的意外關機的原因。|
|/f|強制執行應用程式關閉，而不警告使用者。</br>注意：使用 **/f**選項可能會導致遺失未儲存的資料。|
|/m \\ \\\<電腦名稱 >|指定目標電腦。 不能搭配 **/l**選項。|
|/t \<XXX>|設定的逾時期限或延遲*XXX*秒後再重新啟動或關機。 這會導致本機主控台上顯示的警告。 您可以指定 0 到 600 秒。 如果您不要使用 **/t**，逾時期限是預設為 30 秒。|
|/d [p\|u:]\<XX>:\<YY>|列出系統重新啟動或關機的原因。 以下是參數值：</br>**p**指出已規劃 重新啟動或關機。</br>**u**指出的原因是使用者定義。</br>注意：如果**p**或是**u**未指定，重新啟動或關機時未計劃。</br>*XX*指定主要原因編號 （小於 256 的正整數）。</br>*YY*指定次要原因編號 （小於 65536 的正整數）。|
|/c"\<註解 > 」|可以讓您詳細註解關機的原因。 您必須先使用提供的原因 **/d**選項。 您必須將註解括在引號中。 最多可以使用 511 個字元。|
|/?|在命令提示字元，包括一份您本機電腦所定義的主要和次要原因顯示說明。|

## <a name="remarks"></a>備註

-   使用者必須被指派**關機**使用者權限來關閉本機或遠端管理使用電腦**關機**命令。
-   使用者必須是 Administrators 群組的成員加上註解的本機或遠端管理電腦意外的關機。 如果目標電腦已加入網域，Domain Admins 群組的成員可以執行此程序。 如需詳細資訊，請參閱：  
    -   [預設本機群組](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [預設群組](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   如果您想要一次關閉一個以上的電腦，您可以呼叫**關機**每一部電腦，使用指令碼，或者您可以使用**關機** **/i**顯示遠端關閉對話方塊。
-   如果您指定主要和次要原因代碼時，您必須先定義這些原因代碼，您打算使用原因的每部電腦上。 如果目標電腦上未定義的原因代碼，關機事件追蹤器無法記錄正確的原因的文字。
-   表示，會規劃關閉之後使用，請記得**p:** 參數。 省略**p:** 表示非計劃性關機。 如果您鍵入**p:** 後面未計劃的關機的原因代碼，命令才會執行關機。 相反地，如果您省略**p:** 和計劃的關機時，此命令的原因代碼中的型別不會執行關機。

## <a name="BKMK_examples"></a>範例

若要強制關閉，並在一分鐘的延遲，原因為之後重新啟動本機電腦的應用程式 」 應用程式：維護 （已計劃）"和"Reconfiguring myapp.exe"類型的註解：
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
若要重新啟動遠端電腦\\ \\ServerName 使用相同的參數，輸入：
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
