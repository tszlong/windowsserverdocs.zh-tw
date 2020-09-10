---
title: evntcmd
description: Evntcmd 命令的參考文章，此命令會根據設定檔中的資訊，將事件的轉譯設定為陷阱、陷阱目的地或兩者。
ms.topic: reference
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 50347e8ef8c007fa89b1b226f705d4dcd6935e60
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636035"
---
# <a name="evntcmd"></a>evntcmd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

根據設定檔中的資訊，設定將事件轉譯為陷阱、陷阱目的地或兩者。

## <a name="syntax"></a>語法

```
evntcmd [/s <computername>] [/v <verbositylevel>] [/n] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /s `<computername>` | 依名稱指定要將事件轉譯為陷阱、陷阱目的地或兩者進行設定的電腦。 如果您未指定電腦，則會在本機電腦上進行設定。 |
| /v `<verbositylevel>` | 指定在設定陷阱和設陷目的地時，會顯示哪些類型的狀態訊息。 此參數必須是介於0到10之間的整數。 如果您指定10，則會顯示所有類型的訊息，包括追蹤訊息和是否成功設定陷阱的警告。 如果您指定0，則不會出現任何訊息。 |
| /n | 指定如果此電腦收到設陷設定變更，則不應重新開機 SNMP 服務。 |
| `<filename>` | 依名稱指定設定檔，該設定檔包含要設定之事件的轉譯相關資訊，以及要設定的陷阱目的地。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您想要設定陷阱但不設陷目的地，您可以使用 [事件] 來建立有效的設定檔，這是一個圖形化公用程式。 如果您已安裝 SNMP 服務，可以在命令提示字元中輸入 **evntwin** 來啟動事件，以將翻譯工具設為陷阱。 定義您想要的陷阱之後，請按一下 [ **匯出** ] 來建立適合與 **evntcmd**搭配使用的檔案。 您可以使用事件來設陷 Translator 來輕鬆建立設定檔，然後在命令提示字元中使用 **evntcmd** 設定檔，在多部電腦上快速設定陷阱。

- 設定陷阱的語法如下所示：

  ```
  #pragma add <eventlogfile> <eventsource> <eventID> [<count> [<period>]]
  ```

  以下文字是 true：

    - **#pragma** 必須出現在檔案中每個專案的開頭。

    - **Add**參數會指定您要新增事件來設定陷阱設定。

    - **Eventlogfile**、 **eventsource**和**eventID**都是必要參數，其中**eventlogfile**指定記錄事件的檔案， **Eventsource**指定產生事件的應用程式，而**eventID**則指定識別每個事件的唯一編號。

    若要判斷哪些值對應至每個事件，請在命令提示字元中輸入 **evntwin** ，以啟動要設陷轉譯程式的事件。 按一下 [ **自訂**]，然後按一下 [ **編輯**]。 在 [ **事件來源**] 下流覽資料夾，直到找出您要設定的事件、按一下它，然後按一下 [ **新增**]。 事件來源、事件記錄檔和事件識別碼的相關資訊會分別出現在 [ **來源]、[記錄**檔] 和 [設陷] **特定識別碼**下。

    - **Count**參數是選擇性的，它會指定在傳送陷阱訊息之前，事件必須發生多少次。 如果您未使用此參數，則會在事件發生一次之後傳送陷阱訊息。

    - **Period**參數是選擇性的，但它會要求您使用**count**參數。 Period 參數會以秒為單位來指定時間長度 () 在這 **段** 期間，事件必須發生在傳送陷阱訊息之前，以 **count** 參數指定的次數。 如果您未使用此參數，則會在事件發生的次數與 ***count*** 參數指定的次數之間傳送陷阱訊息，不論發生的時間有多少時間。

- 移除陷阱的語法如下所示：

  ```
  #pragma delete <eventlogfile> <eventsource> <eventID>
  ```

  以下文字是 true：

    - **#pragma** 必須出現在檔案中每個專案的開頭。

    - 參數 **delete** 指定您要移除事件以設定陷阱設定。

    - **Eventlogfile**、 **eventsource**和**eventID**都是必要參數，其中**eventlogfile**指定記錄事件的檔案， **Eventsource**指定產生事件的應用程式，而**eventID**則指定識別每個事件的唯一編號。

    若要判斷哪些值對應至每個事件，請在命令提示字元中輸入 **evntwin** ，以啟動要設陷轉譯程式的事件。 按一下 [ **自訂**]，然後按一下 [ **編輯**]。 在 [ **事件來源**] 下流覽資料夾，直到找出您要設定的事件、按一下它，然後按一下 [ **新增**]。 事件來源、事件記錄檔和事件識別碼的相關資訊會分別出現在 [ **來源]、[記錄**檔] 和 [設陷] **特定識別碼**下。

- 設定陷阱目的地的語法如下：

  ```
  #pragma add_TRAP_DEST <communityname> <hostID>
  ```

  以下文字是 true：

    - **#pragma** 必須出現在檔案中每個專案的開頭。

    - 參數 **add_TRAP_DEST** 會指定您想要將陷阱訊息傳送至某群組內的指定主機。

    - 參數 **communityname** 會依名稱指定傳送陷阱訊息的目標群體。

    - 參數 **hostID** 會依名稱或 IP 位址指定您要在其中傳送陷阱訊息的主機。

- 移除陷阱目的地的語法如下：

  ```
  #pragma delete_TRAP_DEST <communityname> <hostID>
  ```

  以下文字是 true：

    - **#pragma** 必須出現在檔案中每個專案的開頭。

    - 參數 **delete_TRAP_DEST** 指定您不想要將陷阱訊息傳送至某一個控制項內的指定主機。

    - 參數 **communityname** 會依名稱指定不要傳送陷阱訊息的目標。

    - 參數 **hostID** 會依名稱或 IP 位址指定您不想要傳送陷阱訊息的目標主機。

### <a name="examples"></a>範例

下列範例說明 **evntcmd** 命令的設定檔中的專案。 它們並非設計成在命令提示字元中輸入。

若要在事件記錄檔服務重新開機時傳送陷阱訊息，請輸入：

```
#pragma add System Eventlog 2147489653
```

若要在三分鐘內重新開機事件記錄服務兩次，以傳送陷阱訊息，請輸入：

```
#pragma add System Eventlog 2147489653 2 180
```

若要在事件記錄檔服務重新開機時停止傳送陷阱訊息，請輸入：

```
#pragma delete System Eventlog 2147489653
```

若要將名為 *Public* 的群組中的陷阱訊息傳送至 IP 位址為 *192.168.100.100*的主機，請輸入：

```
#pragma add_TRAP_DEST public 192.168.100.100
```

若要在名為 *Private* 的社區中將陷阱訊息傳送至名為 *Host1*的主機，請輸入：

```
#pragma add_TRAP_DEST private Host1
```

若要停止在名為 *Private* 的社區內將陷阱訊息傳送到您要設定陷阱目的地的同一部電腦，請輸入：

```
#pragma delete_TRAP_DEST private localhost
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
