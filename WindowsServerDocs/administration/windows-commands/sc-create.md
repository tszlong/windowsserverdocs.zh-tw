---
title: Sc 建立
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ef1480a7c1ed9fb0aa7e42077565526e5c9f7540
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791176"
---
# <a name="sc-create"></a>Sc 建立



在登錄和服務控制管理員資料庫中，建立服務的子機碼和專案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parameters

|參數|說明|
|---------|-----------|
|\<ServerName >|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例（UNC）格式（例如，\\\\myserver）。 若要在本機執行 SC.EXE，請省略此參數。|
|\<ServiceName >|指定**getkeyname**作業所傳回的服務名稱。|
|類型 = {自有 \| 共用 \| 核心 \| filesys \| rec \| 互動類型 = {自有 \| 共用}}|指定服務類型。 預設設定為**type = 自有**。</br>**自有**-指定服務在自己的進程中執行。 它不會與其他服務共用可執行檔。 這是預設設定。</br>**share** -指定服務以共用進程的形式執行。 它會與其他服務共用可執行檔。</br>**核心**-指定驅動程式。</br>**filesys** -指定檔案系統驅動程式。</br>**rec** -指定檔案系統辨識的驅動程式（識別電腦上使用的檔案系統）。</br>**互動**-指定服務可以與桌面互動，並接收使用者的輸入。 互動式服務必須以 LocalSystem 帳戶執行。 此類型必須與**type = 自有**或**type = shared**搭配使用。 使用**type = 互動**本身會產生「不正確參數」錯誤。|
|啟動 = {boot \| 系統 \| 自動 \| 需求 \| 停用 \| 延遲自動}|指定服務的啟動類型。 預設設定為 [**啟動 = 要求**]。</br>**開機**-指定開機載入器所載入的設備磁碟機。</br>**系統**-指定在核心初始化期間啟動的設備磁碟機。</br>**自動**指定每次電腦重新開機時自動啟動的服務。 請注意，即使沒有任何人登入電腦，服務也會執行。</br>**需求**-指定必須手動啟動的服務。 如果未指定**start =** ，這就是預設值。</br>**disabled** -指定無法啟動的服務。 若要啟動已停用的服務，請將 [啟動類型] 變更為其他值。</br>**延遲-自動**指定啟動其他自動服務之後，很快就會自動啟動的服務。|
|錯誤 = {一般 \| 嚴重 \| 重大 \| 忽略}|指定當電腦啟動時服務失敗時的錯誤嚴重性。 預設設定為 [**錯誤 = 一般**]。</br>**normal** -指定記錄錯誤。 隨即顯示訊息方塊，通知使用者服務無法啟動。 啟動將會繼續。 這是預設設定。</br>**嚴重**-指定記錄錯誤（如果可能的話）。 電腦會嘗試使用上次的正確設定重新開機。 這可能會導致電腦能夠重新開機，但服務可能仍無法執行。</br>**重大**-指定記錄錯誤（如果可能的話）。 電腦會嘗試使用上次的正確設定重新開機。 如果最後一個已知的正確設定失敗，啟動也會失敗，且開機程式會中止並出現停止錯誤。</br>**忽略**-指定錯誤已記錄且啟動繼續進行。 除了在事件記錄檔中記錄錯誤以外，不會提供任何通知給使用者。|
|bin 路徑 = \<BinaryPathName >|指定服務二進位檔案的路徑。 **Bin 路徑 =** 沒有預設值，而且必須提供此字串。|
|群組 = \<LoadOrderGroup >|指定此服務為其成員之群組的名稱。 群組清單會儲存在登錄中的**HKLM\System\CurrentControlSet\Control\ServiceGroupOrder**子機碼中。 預設值為 null。|
|標記 = {是 \| 否}|指定是否要從 CreateService 呼叫取得 TagID。 標記僅用於開機啟動和系統啟動驅動程式。|
|依存 = \<相依性 >|指定在此服務啟動之前必須啟動的服務或群組的名稱。 名稱是以正斜線（/）分隔。|
|obj = {\<AccountName > \| \<ObjectName >}|指定服務將在其中執行之帳戶的名稱，或指定將在其中執行驅動程式之 Windows 驅動程式物件的名稱。|
|displayname = \<DisplayName >|指定可供使用者介面程式用來識別服務的易記名稱。|
|password = \<密碼 >|指定密碼。 如果使用 LocalSystem 以外的帳戶，則這是必要的。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   針對每個命令列選項，等號是選項名稱的一部分。
-   選項與值之間必須有空格（例如， **type = 自有**）。 如果省略空間，作業將會失敗。

## <a name="BKMK_examples"></a>典型

下列範例會示範如何使用**sc create**命令：
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
