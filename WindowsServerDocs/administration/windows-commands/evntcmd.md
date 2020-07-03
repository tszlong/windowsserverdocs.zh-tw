---
title: evntcmd
description: Evntcmd 命令的參考文章，其會根據設定檔中的資訊，將事件的轉譯設定為陷阱、陷阱目的地或兩者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 291b13163f5c5a13442ed6dc80b769d0170df42e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922789"
---
# <a name="evntcmd"></a>evntcmd

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會根據設定檔中的資訊，將事件的轉譯設定為陷阱、陷阱目的地或兩者。

## <a name="syntax"></a>語法

```
evntcmd [/s <computername>] [/v <verbositylevel>] [/n] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /s`<computername>` | 依名稱指定要將事件轉譯成陷阱、陷阱目的地或兩者的電腦。 如果您未指定電腦，則會在本機電腦上進行設定。 |
| 停`<verbositylevel>` | 指定哪些類型的狀態訊息會顯示為「陷阱」和「設陷目的地」。 這個參數必須是介於0到10之間的整數。 如果您指定10，則會顯示所有類型的訊息，包括追蹤訊息以及是否已成功設定 trap 的警告。 如果您指定0，則不會出現任何訊息。 |
| /n | 指定如果此電腦收到陷阱設定變更，則不應重新開機 SNMP 服務。 |
| `<filename>` | 依名稱指定設定檔案，其中包含將事件轉譯成陷阱和要設定之陷阱目的地的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您想要設定陷阱，但不要設陷目的地，您可以使用事件來建立有效的設定檔，這是一種圖形化公用程式。 如果您已安裝 SNMP 服務，您可以在命令提示字元中輸入**evntwin** ，以啟動事件以將轉譯程式設為陷阱。 定義您想要的陷阱之後，請按一下 [**匯出**] 來建立適合與**evntcmd**搭配使用的檔案。 您可以使用事件來設陷 Translator，以輕鬆建立設定檔，然後在命令提示字元中使用設定檔與**evntcmd** ，在多部電腦上快速設定陷阱。

- 設定陷阱的語法如下：

  ```
  #pragma add <eventlogfile> <eventsource> <eventID> [<count> [<period>]]
  ```

  其中的文字元合下列條件：

    - **#pragma**必須出現在檔案中每個專案的開頭。

    - [**新增**] 參數指定您想要將事件新增至 [設陷設定]。

    - **Eventlogfile**、 **eventsource**和**eventID**參數是必要的，其中**eventlogfile**指定記錄事件的檔案， **eventsource**會指定產生事件的應用程式，而**eventid**則指定識別每個事件的唯一編號。

    若要判斷哪些值對應至每個事件，請在命令提示字元中輸入**evntwin** ，以啟動要設陷 Translator 的事件。 按一下 [**自訂**]，然後按一下 [**編輯**]。 在 [**事件來源**] 底下流覽資料夾，直到您找到想要設定的事件、按一下它，然後按一下 [**新增**]。 事件來源、事件記錄檔和事件識別碼的相關資訊分別出現在 [**來源]、[記錄**] 和 [設陷**特定識別碼**] 底下。

    - **Count**參數是選擇性的，它會指定在傳送陷阱訊息之前，事件必須發生多少次。 如果您未使用此參數，則會在事件發生一次之後傳送陷阱訊息。

    - **Period**參數是選擇性的，但它需要您使用**count**參數。 **Period**參數會指定事件必須發生的時間長度（以秒為單位），在此期間內，在傳送 trap 訊息之前，使用**count**參數指定的次數。 如果您不使用此參數，則會在事件發生時，以***count***參數指定的次數來傳送陷阱訊息，不論發生的時間有多少。

- 移除陷阱的語法如下：

  ```
  #pragma delete <eventlogfile> <eventsource> <eventID>
  ```

  其中的文字元合下列條件：

    - **#pragma**必須出現在檔案中每個專案的開頭。

    - [**刪除**] 參數指定您想要移除要設陷設定的事件。

    - **Eventlogfile**、 **eventsource**和**eventID**參數是必要的，其中**eventlogfile**指定記錄事件的檔案， **eventsource**會指定產生事件的應用程式，而**eventid**則指定識別每個事件的唯一編號。

    若要判斷哪些值對應至每個事件，請在命令提示字元中輸入**evntwin** ，以啟動要設陷 Translator 的事件。 按一下 [**自訂**]，然後按一下 [**編輯**]。 在 [**事件來源**] 底下流覽資料夾，直到您找到想要設定的事件、按一下它，然後按一下 [**新增**]。 事件來源、事件記錄檔和事件識別碼的相關資訊分別出現在 [**來源]、[記錄**] 和 [設陷**特定識別碼**] 底下。

- 設定陷阱目的地的語法如下：

  ```
  #pragma add_TRAP_DEST <communityname> <hostID>
  ```

  其中的文字元合下列條件：

    - **#pragma**必須出現在檔案中每個專案的開頭。

    - 參數**add_TRAP_DEST**指定您想要將陷阱訊息傳送至社區內的指定主機。

    - 參數**communityname**會依名稱指定用來傳送陷阱訊息的團體。

    - 參數**hostID**會依名稱或 IP 位址指定您想要將訊息傳送到其中的主機。

- 移除陷阱目的地的語法如下：

  ```
  #pragma delete_TRAP_DEST <communityname> <hostID>
  ```

  其中的文字元合下列條件：

    - **#pragma**必須出現在檔案中每個專案的開頭。

    - 參數**delete_TRAP_DEST**指定您不想要將訊息傳送至社區內的指定主機。

    - 參數**communityname**會依名稱指定不應將陷阱訊息傳送至其中的團體。

    - 參數**hostID**會依名稱或 IP 位址指定您不想要將訊息傳送至其中的主機。

### <a name="examples"></a>範例

下列範例說明**evntcmd**命令的設定檔中的專案。 它們的設計不是在命令提示字元中輸入。

若要在事件記錄檔服務重新開機時傳送陷阱訊息，請輸入：

```
#pragma add System Eventlog 2147489653
```

若要在三分鐘內重新開機事件記錄服務兩次時傳送陷阱訊息，請輸入：

```
#pragma add System Eventlog 2147489653 2 180
```

若要在每次事件記錄檔服務重新開機時停止傳送陷阱訊息，請輸入：

```
#pragma delete System Eventlog 2147489653
```

若要將名為*Public*的社區內的陷阱訊息傳送到 IP 位址為*192.168.100.100*的主機，請輸入：

```
#pragma add_TRAP_DEST public 192.168.100.100
```

若要將名為*Private*的社區內的陷阱訊息傳送至名為*Host1*的主機，請輸入：

```
#pragma add_TRAP_DEST private Host1
```

若要停止將名為*私*用的社區中的陷阱訊息傳送到您設定設陷目的地的同一部電腦，請輸入：

```
#pragma delete_TRAP_DEST private localhost
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
