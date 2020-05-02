---
title: Sc config
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 45a94b3eea78552b61535542d85793bbaffd3df2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722221"
---
# <a name="sc-config"></a>Sc config



修改登錄和服務控制管理員資料庫中服務專案的值。



## <a name="syntax"></a>語法

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例（UNC）格式（ \\ \\例如 myserver）。 若要在本機執行 SC.EXE，請省略此參數。|
|\<ServiceName>|指定**getkeyname**作業所傳回的服務名稱。|
|類型 = {自有\|共用\|核心\| filesys \| rec \|調整\|互動類型 = {自有\|共用}} | 指定服務類型。</br>**自有**-指定在自己的進程中執行的服務。 它不會與其他服務共用可執行檔。 這是預設值。</br>**共用**-指定以共用進程形式執行的服務。 它會與其他服務共用可執行檔。</br>**核心**-指定驅動程式。</br>**filesys** -指定檔案系統驅動程式。</br>**rec** -指定可識別檔案系統的驅動程式，以識別電腦上使用的檔案系統。</br>[**調整**]-指定可識別硬體裝置（例如鍵盤、滑鼠和磁片磁碟機）的介面卡驅動程式。</br>**互動**-指定可與桌面互動的服務，並接收使用者的輸入。 互動式服務必須以 LocalSystem 帳戶執行。 此類型必須搭配**type = 自有**或**type = shared** （例如**type = 互動****類型 = 自有**）使用。 使用**type = 互動**本身將會產生錯誤。|
|開始 = {開機\|系統\|自動\|要求\|已\|停用延遲-自動}|指定服務的啟動類型。</br>**開機**-指定開機載入器所載入的設備磁碟機。</br>**系統**-指定在核心初始化期間啟動的設備磁碟機。</br>**自動**指定每次電腦重新開機時自動啟動的服務，即使沒有一個登入電腦也會執行。</br>**需求**-指定必須手動啟動的服務。 如果未指定**start =** ，這就是預設值。</br>**disabled** -指定無法啟動的服務。 若要啟動已停用的服務，請將 [啟動類型] 變更為其他值。</br>**延遲-自動**指定啟動其他自動服務之後，很快就會自動啟動的服務。|
|錯誤 = {一般\|嚴重\|重大\|忽略}|如果服務在開機時無法啟動，則指定錯誤的嚴重性。</br>**normal** -指定已記錄錯誤並顯示訊息方塊，通知使用者服務無法啟動。 啟動將會繼續。 這是預設值。</br>**嚴重**-指定記錄錯誤（如果可能的話）。 電腦會嘗試使用上次的正確設定重新開機。 這可能會導致電腦能夠重新開機，但服務可能仍無法執行。</br>**重大**-指定記錄錯誤（如果可能的話）。 電腦會嘗試使用上次的正確設定重新開機。 如果最後一個已知的正確設定失敗，啟動也會失敗，且開機程式會中止並出現停止錯誤。</br>**忽略**-指定錯誤已記錄且啟動繼續進行。 除了在事件記錄檔中記錄錯誤以外，不會提供任何通知給使用者。|
|bin 路徑 = \<BinaryPathName>|指定服務二進位檔案的路徑。|
|群組 = \<LoadOrderGroup>|指定此服務為其成員之群組的名稱。 群組清單會儲存在登錄中的**HKLM\System\CurrentControlSet\Control\ServiceGroupOrder**子機碼中。 預設值為 null。|
|標記 = {yes \|否}|指定是否要從 CreateService 呼叫取得 TagID。 標記僅用於開機啟動和系統啟動驅動程式。|
|相依于\<= 相依性>|指定必須在此服務之前啟動的服務或群組的名稱。 名稱是以正斜線（/）分隔。|
|obj = {\<AccountName> \| \<ObjectName>}|指定服務將在其中執行之帳戶的名稱，或指定將在其中執行驅動程式之 Windows 驅動程式物件的名稱。 預設設定為 [ **LocalSystem**]。|
|displayname = \<displayname>|指定在使用者介面程式中用來識別服務的描述性名稱。 例如，其中一個特定服務的子機碼名稱是**wuauserv**，它的顯示名稱會更容易自動更新。|
|password = \<密碼>|指定密碼。 如果使用 LocalSystem 帳戶以外的帳戶，則這是必要的。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   針對每個命令列選項（參數），等號是選項名稱的一部分。
-   選項與值之間必須有空格（例如， **type = 自有**）。 如果省略空間，作業將會失敗。

## <a name="examples"></a>範例

若要指定 NEWSERVICE 服務的二進位路徑，請輸入：
```
sc config NewService binpath= ntsd -d c:\windows\system32\NewServ.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)