---
title: Evntcmd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a149e78170f2849512dcfc0a0a82f9eed979abe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885979"
---
# <a name="evntcmd"></a>Evntcmd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定翻譯的設陷、 設陷目的地，或兩者的組態檔中的資訊為基礎的事件。   
## <a name="syntax"></a>語法  
```  
evntcmd [/s <computerName>] [/v <verbosityLevel>] [/n] <FileName>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/s <computerName>|依名稱，指定您要設定翻譯的設陷、 設陷目的地，或兩者的事件的電腦。 如果您沒有指定的電腦，設定就會發生在本機電腦上。|  
|/v <verbosityLevel>|指定設定的狀態訊息會顯示為設陷和設陷目的地類型。 這個參數必須是介於 0 到 10 之間的整數。 如果您指定 10 時，會出現所有類型的訊息，包括追蹤訊息及警告設陷設定是否成功。 如果您指定 0 時，便會不顯示任何訊息。|  
|/n|指定是否此電腦收到設陷組態變更，不應該重新啟動 SNMP 服務。|  
|<FileName>|指定包含您想要設定翻譯的設陷和設陷目的地事件的相關資訊的組態檔的名稱。|  
|/?|在命令提示字元顯示說明。|  
## <a name="remarks"></a>備註  
-   如果您想要設定設陷，但不是設陷目的地，您可以使用事件設陷轉譯器，這圖形化公用程式來建立有效的組態檔。 如果您已安裝 SNMP 服務時，您可以輸入來啟動設陷轉譯器的事件**evntwin**在命令提示字元。 定義您想要的設陷之後，按一下 [匯出] 來建立檔案適用於**evntcmd**。 您可以使用設陷轉譯器的事件，來輕鬆地建立組態檔，然後使用 具有組態檔**evntcmd**在命令提示字元中，快速地設定多部電腦上的 設陷。  
-   設定的設陷的語法如下所示：  
    **#pragma add***<EventLogFile> <EventSource> <EventID> [<Count> [<Period>]]*  
    -   文字 **#pragma**必須出現在檔案中的每個項目開頭。  
    -   參數**新增**指定您想要新增事件來攔截組態。  
    -   參數*EventLogFile*， *EventSource*，並*EventID*所需。 參數*EventLogFile*指定記錄事件所在的檔案。 參數*EventSource*指定會產生事件的應用程式。 *EventID*參數會指定唯一識別每個事件的數目。 若要了解哪些值對應至特定的事件，請輸入，設陷轉譯器的事件啟動**evntwin**在命令提示字元。 按一下 **自訂**，然後按一下**編輯**。 底下**事件來源**，瀏覽的資料夾，直到您找出您想要設定，按一下它，然後按一下 的事件**新增**。 事件來源、 事件記錄檔和事件識別碼的相關資訊出現在**來源，請登**，並**攔截特定識別碼**分別。  
    -   *計數*參數是選擇性的而且它會指定事件的設陷訊息傳送之前，必須發生的次數。 如果您不要使用*計數*參數設陷訊息會傳送事件一次發生之後。  
    -   *期限*參數是選擇性的但它會要求您使用*計數*參數。 *期間*參數會指定一段期間事件必須發生的次數設陷訊息傳送之前，使用 Count 參數指定的時間 （以秒為單位）。 如果您不要使用*期限*參數設陷訊息會傳送事件發生指定次數之後*計數*參數，不論項目之間經過的時間。  
-   移除的設陷的語法如下所示：  
    **#pragma delete***<EventLogFile> <EventSource> <EventID>*  
    -   文字 **#pragma**必須出現在檔案中的每個項目開頭。  
    -   參數*刪除*指定您想要移除的事件來攔截組態。  
    -   參數*EventLogFile*， *EventSource*，並*EventID*所需。 參數*EventLogFile*指定記錄檔中就會記錄事件。 參數*EventSource*指定會產生事件的應用程式。 *EventID*參數會指定唯一識別每個事件的數目。  
-   設定的設陷目的地的語法如下所示：  
    **#pragma add_TRAP_DEST***<CommunityName> <HostID>*  
    -   文字 **#pragma**必須出現在檔案中的每個項目開頭。  
    -   參數**add_TRAP_DEST**指定您想要傳送至指定的主機，社群內的設陷訊息。  
    -   參數*CommunityName*依名稱指定的社群的設陷訊息傳送。  
    -   參數*HostID*依名稱或 IP 位址，指定您要設陷訊息傳送的主機。  
-   移除的設陷目的地的語法如下所示：  
    **#pragma delete_TRAP_DEST***<CommunityName> <HostID>*  
    -   文字 **#pragma**必須出現在檔案中的每個項目開頭。  
    -   參數*delete_TRAP_DEST*指定不想要傳送至指定的主機，社群內的設陷訊息。  
    -   參數*CommunityName*依名稱指定的社群的設陷訊息傳送。  
    -   參數*HostID*依名稱或 IP 位址，指定您不要設陷訊息傳送的主機。  
## <a name="BKMK_Examples"></a>範例  
下列範例說明的組態檔中的項目**evntcmd**命令。 它們並非設計來在 命令提示字元中輸入。  
若要傳送設陷訊息的事件記錄服務重新啟動時，輸入：  
```  
#pragma add System "Eventlog" 2147489653  
```  
若要傳送設陷訊息，如果事件日誌服務重新啟動兩次相同的三分鐘，輸入：  
```  
#pragma add System "Eventlog" 2147489653 2 180  
```  
若要停止傳送設陷訊息，只要事件記錄檔服務重新啟動，請輸入：  
```  
#pragma delete System "Eventlog" 2147489653  
```  
若要傳送至主機的 IP 位址 192.168.100.100 名為 Public 的社群內的設陷訊息，請輸入：  
```  
#pragma add_TRAP_DEST public 192.168.100.100  
```  
若要傳送要在名為 Host1 名為私人社群內的設陷訊息，請輸入：  
```  
#pragma add_TRAP_DEST private Host1  
```  
若要停止傳送至相同的電腦設定的設陷目的地，名為 Private 社群內的設陷訊息，請輸入：  
```  
#pragma delete_TRAP_DEST private localhost  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
