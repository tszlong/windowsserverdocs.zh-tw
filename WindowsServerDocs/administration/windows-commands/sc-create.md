---
title: Sc 建立
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7931ddc91b91d5fce01335f4b090d0305790f65c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826499"
---
# <a name="sc-create"></a>Sc 建立



在登錄和服務控制管理員資料庫中，建立子機碼和服務的項目。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定由服務所在的遠端伺服器的名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如\\ \\myserver)。 若要在本機執行 SC.exe，省略這個參數。|
|\<ServiceName>|指定所傳回的服務名稱**getkeyname**作業。|
|類型 = {自己\|分享\|核心\|filesys \| rec\|互動類型 = {自己\|共用}}|指定服務型別。 預設值是**類型 = 自己**。</br>**自己**-指定在自己的處理序中執行服務。 它不會與其他服務共用的可執行檔。 此為預設設定。</br>**共用**-指定服務執行為共用的程序。 它與其他服務共用的可執行檔。</br>**核心**-指定的驅動程式。</br>**filesys** -指定檔案系統驅動程式。</br>**rec** -指定檔案系統可辨識的驅動程式，（識別電腦上使用的檔案系統）。</br>**互動**-指定服務可以互動桌面，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。 此類型必須用於搭配**類型 = 自己**或是**類型 = 共用**。 使用**類型 = 互動**本身會產生 「 無效的參數 」 錯誤。|
|開始 = {開機\|系統\|自動\|需求\|停用}|指定服務的啟動類型。 預設值是**啟動 = 需求**。</br>**開機**-指定裝置驅動程式載入的開機載入器。</br>**系統**-指定啟動核心初始化期間的裝置驅動程式。</br>**自動**-指定會自動啟動 每次電腦重新啟動服務。 請注意，服務會執行，即使沒有人登入電腦。</br>**隨選**-指定的服務必須以手動方式啟動。 這是預設值，如果**啟動 =** 未指定。</br>**停用**-指定的服務無法啟動。 若要啟動已停用的服務，啟動類型變更為其他值。|
|錯誤 = {正常\|嚴重\|重要\|忽略}|如果該服務無法啟動電腦時，請指定錯誤的嚴重性。 預設值是**錯誤 = 正常**。</br>**一般**-指定會記錄錯誤。 會顯示訊息方塊，告知使用者該服務無法啟動。 啟動會繼續。 此為預設設定。</br>**嚴重**-指定錯誤記錄 （如果可能）。 電腦會嘗試使用上次的正確設定重新啟動。 這可能會導致電腦能夠重新啟動，但服務可能仍無法執行。</br>**重要**-指定錯誤記錄 （如果可能）。 電腦會嘗試使用上次的正確設定重新啟動。 如果上次的正確設定失敗，啟動也會失敗，並在開機程序突然停止並停止錯誤。</br>**忽略**-指定記錄錯誤並繼續啟動。 沒有通知給超過事件記錄檔中記錄錯誤的使用者。|
|binpath= \<BinaryPathName>|指定服務二進位檔的路徑。 針對沒有預設值**bin 路徑 =**，而且必須提供此字串。|
|group= \<LoadOrderGroup>|指定此服務為成員的群組的名稱。 在登錄中儲存的群組清單**HKLM\System\CurrentControlSet\Control\ServiceGroupOrder**子機碼。 預設值是 null。|
|標記 = {是\|無}|指定 TagID 是否要將取自 CreateService 呼叫。 標記僅用於開機啟動和系統啟動驅動程式。|
|depend= \<dependencies>|指定服務或啟動此服務必須啟動的群組的名稱。 以正斜線 （/） 分隔名稱。|
|obj= {\<AccountName> \| \<ObjectName>}|指定的帳戶的服務將會執行，或指定的驅動程式將在其中執行的 Windows 驅動程式物件名稱的名稱。|
|displayname= \<DisplayName>|指定可以由使用者介面程式用來識別服務的易記名稱。|
|password= \<Password>|指定的密碼。 這是必要的如果使用 LocalSystem 以外的帳戶。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   針對每個命令列選項，等號是選項名稱的一部分。
-   不需要的選項和其值之間的空間 (例如**類型 = 自己**。 如果省略空間作業將會失敗。

## <a name="BKMK_examples"></a>範例

下列範例示範如何使用**sc 建立**命令：
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
