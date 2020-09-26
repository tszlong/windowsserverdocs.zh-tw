---
title: sc.exe 建立
description: sc.exe create 命令的參考文章，此命令會在登錄和服務控制管理員資料庫中建立服務的子機碼和專案。
ms.topic: reference
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2be59f0d91abdf91984985c536e45abb3cdc0d3f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388507"
---
# <a name="scexe-create"></a>sc.exe 建立

在登錄和服務控制管理員資料庫中建立服務的子機碼和專案。

## <a name="syntax"></a>語法

```
sc.exe [<servername>] create [<servicename>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <binarypathname>] [group= <loadordergroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<accountname> | <objectname>}] [displayname= <displayname>] [password= <password>]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
| `<servername>` | 指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ myserver) 。 若要在本機執行 SC.exe，請不要使用此參數。 |
| `<servicename>` | 指定 **getkeyname** 作業所傳回的服務名稱。 |
| `type= {own | share | kernel | filesys | rec | interact type= {own | share}}` | 指定服務類型。 選項包括：<ul><li>**自有** -指定在其本身的進程中執行的服務。 它不會與其他服務共用可執行檔。 這是預設值。</li><li>**共用** -指定以共用進程的形式執行的服務。 它會與其他服務共用可執行檔。</li><li>**核心** -指定驅動程式。</li><li>**filesys** -指定檔案系統驅動程式。</li><li>**rec** -指定可識別電腦上使用之檔案系統的檔案系統辨識驅動程式。</li><li>**互動** -指定可與桌面互動的服務，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。 此類型必須與**type = 自有**或**type = shared** (搭配使用，例如**type = 互動****類型 = 自有**) 。 使用 **type = 互動** 本身將會產生錯誤。</li></ul> |
| `start= {boot | system | auto | demand | disabled | delayed-auto}` | 指定服務的啟動類型。 選項包括：<ul><li>**開機** -指定開機載入器載入的設備磁碟機。</li><li>**系統** -指定在核心初始化期間啟動的設備磁碟機。</li><li>**自動** 指定每次電腦重新開機時自動啟動的服務，即使沒有任何人登入電腦也會執行。</li><li>**demand** -指定必須手動啟動的服務。 如果未指定 **start =** ，則這是預設值。</li><li>**disabled** -指定無法啟動的服務。 若要啟動已停用的服務，請將 [啟動類型] 變更為其他值。</li><li>**延遲-自動** 指定在啟動其他自動服務之後，會自動啟動的服務。</li></ul> |
| `error= {normal | severe | critical | ignore}` | 指定當電腦啟動時，服務無法啟動時的錯誤嚴重性。 選項包括：<ul><li>[**正常**]-指定記錄錯誤並顯示訊息方塊，通知使用者服務無法啟動。 啟動將會繼續。 這是預設值。</li><li>**嚴重** -指定如果可能) ，就會將錯誤記錄 (。 電腦會嘗試以最後一個已知的正確設定重新開機。 這可能會導致電腦重新開機，但是服務仍無法執行。</li><li>**critical** -指定在可能的)  (記錄錯誤。 電腦會嘗試以最後一個已知的正確設定重新開機。 如果最後一個已知的良好設定失敗，則啟動也會失敗，而且開機程式會因停止錯誤而停止。</li><li>**ignore** -指定記錄錯誤並繼續進行。 除了將錯誤記錄在事件記錄檔中之外，使用者不會提供任何通知。</li></ul> |
| `binpath= <binarypathname>` | 指定服務二進位檔案的路徑。 **Bin 路徑 =** 沒有預設值，而且必須提供這個字串。 |
| `group= <loadordergroup>` | 指定此服務為其成員之群組的名稱。 群組清單會儲存在登錄的 **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** 子機碼中。 預設值為 null。 |
| `tag= {yes | no}` | 指定是否從 CreateService 呼叫取得 TagID。 標記僅適用于開機啟動和系統啟動驅動程式。 |
| `depend= <dependencies>` | 指定必須在此服務之前啟動之服務或群組的名稱。 名稱會以正斜線分隔 (/) 。 |
| `obj= {<accountname> | <objectname>}` | 指定服務將在其中執行的帳戶名稱，或指定驅動程式將在其中執行的 Windows 驅動程式物件名稱。 預設設定為 **LocalSystem**。 |
| `displayname= <displayname>` | 指定在使用者介面程式中識別服務的易記名稱。 例如，一個特定服務的子機碼名稱是 **wuauserv**，它的顯示名稱自動更新更好用。 |
| `password= <password>` | 指定密碼。 如果使用 LocalSystem 帳戶以外的帳戶，則這是必要的。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 每個命令列選項 (參數) 都必須包含等號作為選項名稱的一部分。

- 選項和其值之間必須有一個空格 (例如， **type =** required。 如果省略空格，作業就會失敗。

## <a name="examples"></a>範例

若要為 *NewService* 服務建立及註冊新的二進位路徑，請輸入：

```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
```

```
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
