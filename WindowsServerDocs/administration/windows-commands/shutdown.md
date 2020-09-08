---
title: shutdown
description: 關機的參考文章，可讓您一次關閉或重新開機本機或遠端電腦。
ms.topic: reference
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f83788b4d8e8f92ea1375b9a0f245f9bfa63bc85
ms.sourcegitcommit: 34f9577ef32cbdc7ef96040caabc9d83517f9b79
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554381"
---
# <a name="shutdown"></a>shutdown

可讓您一次針對一台本機或遠端電腦進行關機或重新啟動。



## <a name="syntax"></a>語法

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "descriptive comment"]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/i|顯示 [ **遠端關機] 對話方塊** 。 **/I**選項必須是命令後面的第一個參數。 如果指定了 **/i** ，則會忽略所有其他選項。|
|/l|立即登出目前的使用者，而不會有超時時間。 您無法將 **/l** 與 **/m** 或 **/t**搭配使用。|
|/s|關閉電腦。|
|/r|關機後重新開機電腦。|
|/a|中止系統關機。 只有在超時期間才會生效。 若要使用 **/a**，您也必須使用 **/m** 選項。|
|/p|關閉本機電腦的 (不是遠端電腦) —沒有超時時間或警告。 您只能使用 **/p** 搭配 **/d** 或 **/f**。 如果您的電腦不支援電源關閉功能，它會在您使用 **/p**時關機，但電腦的電源會保持開啟。|
|/h|如果啟用休眠功能，則將本機電腦置於休眠狀態。 您只能使用 **/h** 搭配 **/f**。|
|/e|可讓您記錄目的電腦上未預期關機的原因。|
|/f|強制執行應用程式關閉而不發出警告使用者。</br>注意：使用 **/f** 選項可能會導致未儲存的資料遺失。|
|一樣 \\\\\<ComputerName>|指定目的電腦。 無法與 **/l** 選項搭配使用。|
|一起 \<XXX>|在重新開機或關機之前，將超時時間或延遲設定為 *XXX* 秒。 這會導致在本機主控台顯示警告。 您可以指定0-600 秒。 如果您未使用 **/t**，則超時期限預設為30秒。|
|/d [p \| u：] \<XX> ：\<YY>|列出系統重新開機或關機的原因。 以下是參數值：</br>**p** 表示已規劃重新開機或關機。</br>**u** 指出原因是使用者定義的。</br>注意：如果未指定 **p** 或 **u** ，則會重新開機或關機。</br>*XX* 指定主要原因號碼 (小於 256) 的正整數。</br>*YY* 指定小於 65536)  (正整數的次要原因號碼。|
|/c \<Comment>|可以讓您詳細註解關機的原因。 您必須先使用 **/d** 選項提供原因。 您必須以引號括住批註。 最多可以使用 511 個字元。|
|/?|在命令提示字元中顯示說明，包括在本機電腦上定義的主要和次要原因的清單。|

## <a name="remarks"></a>備註

- 使用者必須被指派 [ **關閉系統** ] 使用者權限，以關閉使用 **shutdown** 命令的本機或遠端系統管理的電腦。
- 使用者必須是 Administrators 群組的成員，才能針對本機或遠端系統管理的電腦，標注非預期的關機。 如果目的電腦已加入網域，則 Domain Admins 群組的成員可能可以執行此程式。 如需詳細資訊，請參閱：
    - [預設本機群組](/previous-versions/windows/it-pro/windows-server-2003/cc785098(v=ws.10))
    - [預設群組](/previous-versions/windows/it-pro/windows-server-2003/cc756898(v=ws.10))
- 如果您想要一次關閉一部以上的電腦，您可以使用腳本針對每部電腦呼叫 **shutdown** ，也可以使用 **shutdown** **/I** 來顯示 [遠端關機] 對話方塊。
- 如果您指定主要和次要原因代碼，您必須先在規劃使用原因的每部電腦上定義這些原因代碼。 如果目的電腦上未定義原因代碼，則關機事件追蹤器無法記錄正確的原因文字。
- 請記得使用 **p：** 參數來表示已規劃關機。 省略 **p：** 表示關閉未計畫。 如果您在未規劃的關機中輸入 **p：** 後面接著原因碼，命令將不會執行關機。 相反地，如果您省略 **p：** 並輸入規劃關機的原因代碼，則命令不會執行關機。

## <a name="examples"></a>範例

若要強制應用程式在一分鐘的時間延遲之後關閉並重新啟動本機電腦，原因是應用程式：維護 (規劃的) ，以及重新設定 myapp.exe 類型的批註：
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
若要使用相同的參數重新開機遠端電腦 \\ \\ 伺服器名稱，請輸入：
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
