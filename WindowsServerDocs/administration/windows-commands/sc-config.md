---
title: Sc.exe 設定
description: 瞭解如何使用 sc.exe 公用程式來變更服務設定
ms.topic: reference
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 06/05/2018
ms.openlocfilehash: 55432910455896434a1857d17016519bedb51caf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637132"
---
# <a name="scexe-config"></a>Sc.exe 設定

修改登錄和服務控制管理員資料庫中服務專案的值。

## <a name="syntax"></a>語法

```
sc.exe [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ServerName>|指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ \\ myserver) 。 若要在本機執行 SC.exe，請省略此參數。|
|\<ServiceName>|指定 **getkeyname** 作業所傳回的服務名稱。|
|type = {自有 \| share \| kernel \| filesys \| rec \| \| 請調整互動類型 = {自有 \| 共用}} | 指定服務類型。</br>**自有** -指定在其本身的進程中執行的服務。 它不會與其他服務共用可執行檔。 這是預設值。</br>**共用** -指定以共用進程的形式執行的服務。 它會與其他服務共用可執行檔。</br>**核心** -指定驅動程式。</br>**filesys** -指定檔案系統驅動程式。</br>**rec** -指定可識別電腦上使用之檔案系統的檔案系統辨識驅動程式。</br>[**調整**]-指定可識別硬體裝置的介面卡驅動程式，例如鍵盤、滑鼠和磁片磁碟機。</br>**互動** -指定可與桌面互動的服務，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。 此類型必須與**type = 自有**或**type = shared** (搭配使用，例如**type = 互動****類型 = 自有**) 。 使用 **type = 互動** 本身將會產生錯誤。|
|開始 = {開機 \| 系統 \| 自動 \| 要求 \| 已停用 \| 延遲-自動}|指定服務的啟動類型。</br>**開機** -指定開機載入器載入的設備磁碟機。</br>**系統** -指定在核心初始化期間啟動的設備磁碟機。</br>**自動** 指定每次電腦重新開機時自動啟動的服務，即使沒有任何人登入電腦也會執行。</br>**demand** -指定必須手動啟動的服務。 如果未指定 **start =** ，則這是預設值。</br>**disabled** -指定無法啟動的服務。 若要啟動已停用的服務，請將 [啟動類型] 變更為其他值。</br>**延遲-自動** 指定在啟動其他自動服務之後，會自動啟動的服務。|
|錯誤 = {正常的 \| 嚴重 \| 緊急 \| 忽略}|如果服務在開機時無法啟動，則指定錯誤的嚴重性。</br>[**正常**]-指定記錄錯誤並顯示訊息方塊，通知使用者服務無法啟動。 啟動將會繼續。 這是預設值。</br>**嚴重** -指定如果可能) ，就會將錯誤記錄 (。 電腦會嘗試以最後一個已知的正確設定重新開機。 這可能會導致電腦重新開機，但是服務仍無法執行。</br>**critical** -指定在可能的)  (記錄錯誤。 電腦會嘗試以最後一個已知的正確設定重新開機。 如果最後一個已知的良好設定失敗，則啟動也會失敗，而且開機程式會因停止錯誤而停止。</br>**ignore** -指定記錄錯誤並繼續進行。 除了將錯誤記錄在事件記錄檔中之外，使用者不會提供任何通知。|
|bin 路徑 = \<BinaryPathName>|指定服務二進位檔案的路徑。|
|群組 = \<LoadOrderGroup>|指定此服務為其成員之群組的名稱。 群組清單會儲存在登錄的 **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** 子機碼中。 預設值為 null。|
|tag = {yes \| 否}|指定是否從 CreateService 呼叫取得 TagID。 標記僅適用于開機啟動和系統啟動驅動程式。|
|相依于 = \<dependencies>|指定必須在此服務之前啟動之服務或群組的名稱。 名稱會以正斜線分隔 (/) 。|
|obj = { \<AccountName> \| \<ObjectName> }|指定服務將在其中執行的帳戶名稱，或指定驅動程式將在其中執行的 Windows 驅動程式物件名稱。 預設設定為 **LocalSystem**。|
|displayname = \<DisplayName>|指定在使用者介面程式中識別服務的描述性名稱。 例如，一個特定服務的子機碼名稱是 **wuauserv**，它的顯示名稱自動更新更好用。|
|密碼 = \<Password>|指定密碼。 如果使用 LocalSystem 帳戶以外的帳戶，則這是必要的。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   針對每個命令列選項 (參數) ，等號是選項名稱的一部分。
-   選項和其值之間必須有一個空格 (例如， **type =** required。 如果省略空格，作業將會失敗。

## <a name="examples"></a>範例

若要指定 NEWSERVICE 服務的二進位路徑，請輸入：
```
sc.exe config NewService binpath= ntsd -d c:\windows\system32\NewServ.exe
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
