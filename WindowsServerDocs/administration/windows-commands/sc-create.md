---
title: Sc.exe 建立
description: 瞭解如何使用 sc.exe 公用程式向 Windows Service Manager 註冊新服務
ms.topic: reference
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a022e3b855496825207f4c94f63d20d4530e92b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037546"
---
# <a name="scexe-create"></a>Sc.exe 建立

在登錄和服務控制管理員資料庫中建立服務的子機碼和專案。

## <a name="syntax"></a>語法

```
sc.exe [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ \\ myserver) 。 若要在本機執行 SC.exe，請省略此參數。|
|\<ServiceName>|指定 **getkeyname** 作業所傳回的服務名稱。|
|type = {自有 \| share \| kernel \| filesys \| rec \| 互動類型 = {自有 \| 共用}}|指定服務類型。 預設設定為 **type = 自有**。</br>**自有** -指定服務在其本身的進程中執行。 它不會與其他服務共用可執行檔。 這是預設值。</br>**共用** -指定服務以共用進程的形式執行。 它會與其他服務共用可執行檔。</br>**核心** -指定驅動程式。</br>**filesys** -指定檔案系統驅動程式。</br>**rec** -指定檔案系統辨識的驅動程式 (識別電腦) 上使用的檔案系統。</br>**互動** -指定服務可與桌面互動，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。 這個型別必須搭配 **type = 自有** 或 **type = shared**一起使用。 使用 **type = 互動** 本身將會產生不正確參數錯誤。|
|開始 = {開機 \| 系統 \| 自動 \| 要求 \| 已停用 \| 延遲-自動}|指定服務的啟動類型。 預設設定為 [ **開始] = [需求**]。</br>**開機** -指定開機載入器載入的設備磁碟機。</br>**系統** -指定在核心初始化期間啟動的設備磁碟機。</br>**自動** 指定每次電腦重新開機時自動啟動的服務。 請注意，即使沒有任何人登入電腦，也會執行該服務。</br>**demand** -指定必須手動啟動的服務。 如果未指定 **start =** ，則這是預設值。</br>**disabled** -指定無法啟動的服務。 若要啟動已停用的服務，請將 [啟動類型] 變更為其他值。</br>**延遲-自動** 指定在啟動其他自動服務之後，會自動啟動的服務。|
|錯誤 = {正常的 \| 嚴重 \| 緊急 \| 忽略}|指定當電腦啟動時，服務失敗時的錯誤嚴重性。 預設設定為 [ **錯誤 = 一般**]。</br>**normal** -指定記錄錯誤。 顯示訊息方塊，通知使用者服務無法啟動。 啟動將會繼續。 這是預設值。</br>**嚴重** -指定如果可能) ，就會將錯誤記錄 (。 電腦會嘗試以最後一個已知的正確設定重新開機。 這可能會導致電腦重新開機，但是服務仍無法執行。</br>**critical** -指定在可能的)  (記錄錯誤。 電腦會嘗試以最後一個已知的正確設定重新開機。 如果最後一個已知的良好設定失敗，則啟動也會失敗，而且開機程式會因停止錯誤而停止。</br>**ignore** -指定記錄錯誤並繼續進行。 除了將錯誤記錄在事件記錄檔中之外，使用者不會提供任何通知。|
|bin 路徑 = \<BinaryPathName>|指定服務二進位檔案的路徑。 **Bin 路徑 =** 沒有預設值，而且必須提供這個字串。|
|群組 = \<LoadOrderGroup>|指定此服務為其成員之群組的名稱。 群組清單會儲存在登錄的 **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** 子機碼中。 預設值為 null。|
|tag = {yes \| 否}|指定是否要從 CreateService 呼叫取得 TagID。 標記僅適用于開機啟動和系統啟動驅動程式。|
|相依于 = \<dependencies>|指定必須在此服務啟動之前啟動的服務或群組的名稱。 名稱會以正斜線分隔 (/) 。|
|obj = { \<AccountName> \| \<ObjectName> }|指定服務將在其中執行的帳戶名稱，或指定驅動程式將在其中執行的 Windows 驅動程式物件名稱。|
|displayname = \<DisplayName>|指定可供使用者介面程式用來識別服務的易記名稱。|
|密碼 = \<Password>|指定密碼。 如果使用非 LocalSystem 的帳戶，則這是必要的。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   針對每個命令列選項，等號是選項名稱的一部分。
-   選項和其值之間必須有一個空格 (例如， **type =** required。 如果省略空格，作業將會失敗。

## <a name="examples"></a>範例

下列範例會示範如何使用 **sc.exe create** 命令：
```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
