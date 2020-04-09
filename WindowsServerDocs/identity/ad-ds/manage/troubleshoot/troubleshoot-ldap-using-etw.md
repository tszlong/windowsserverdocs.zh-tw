---
title: 使用 ETW 針對 LDAP 連接進行疑難排解
description: 如何開啟和使用 ETW 來追蹤 AD DS 網域控制站之間的 LDAP 連接。
author: Teresa-Motiv
manager: dcscontentpm
ms.prod: windows-server-dev
ms.technology: active-directory-lightweight-directory-services
audience: Admin
ms.author: v-tea
ms.topic: article
ms.date: 11/22/2019
ms.openlocfilehash: f7b7df714dbd02b15555fa20c70c1e995e121a48
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822931"
---
# <a name="using-etw-to-troubleshoot-ldap-connections"></a>使用 ETW 針對 LDAP 連接進行疑難排解

[Windows 事件追蹤（ETW）](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)對於 Active Directory Domain Services （AD DS）而言，是一種有價值的疑難排解工具。 您可以使用 ETW 來追蹤 Windows 用戶端與 LDAP 伺服器之間的輕量型目錄存取協定（[ldap](https://docs.microsoft.com/previous-versions/windows/desktop/ldap/lightweight-directory-access-protocol-ldap-api)）通訊，包括 AD DS 網域控制站。

## <a name="how-to-turn-on-etw-and-start-a-trace"></a>如何開啟 ETW 並啟動追蹤

**開啟 ETW**

1. 開啟 [登錄編輯程式]，然後建立下列登錄子機碼：

   **HKEY\_本機\_機\\系統\\CurrentControlSet\\Services\\ldap\\追蹤\\* ProcessName***

   在此子機碼中， *ProcessName*是您想要追蹤之進程的完整名稱，包括其延伸模組（例如，"svchost.exe"）。

1. （**選擇性**）在此子機碼底下，建立名為**PID**的新專案。 若要使用此專案，請將處理序識別碼指派為 DWORD 值。  

   如果您指定處理序識別碼，ETW 只會追蹤具有此處理序識別碼的應用程式實例。

**啟動追蹤會話**

- 開啟 [命令提示字元] 視窗，然後執行下列命令：

   ```cmd
   tracelog.exe -start <SessionName> -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f <FileName> -flag <TraceFlags>
   ```

   此命令中的預留位置代表下列值。

  - \<*SessionName*> 是用來標示追蹤會話的任意識別碼。  
  > [!NOTE]  
  > 稍後當您停止追蹤會話時，您將必須參考此會話名稱。
  - \<*FileName*> 指定將寫入事件的記錄檔。
  - \<*追蹤旗標*> 應該是[追蹤旗標資料表](#values-for-trace-flags)中所列的一個或多個值。

## <a name="how-to-end-a-tracing-session-and-turn-off-event-tracing"></a>如何結束追蹤會話並關閉事件追蹤

**若要停止追蹤**

- 在命令提示字元執行下列命令：

   ```cmd
   tracelog.exe -stop <SessionName>
   ```

   在此命令中，\<*SessionName*> 與您在**tracelog.exe-start**命令中使用的名稱相同。

**關閉 ETW**

- 在 [登錄編輯程式] 中，刪除**HKEY\_本機\_機\\系統\\CurrentControlSet\\Services\\ldap\\追蹤\\* ProcessName*** 子機碼。

## <a name="values-for-trace-flags"></a>追蹤旗標的值

若要使用旗標，請在**tracelog.exe**的引數中，以 <*追蹤旗標*> 預留位置取代旗標值。

> [!NOTE]  
> 您可以使用適當旗標值的總和來指定多個旗標。 例如，若要指定**debug\_SEARCH** （0x00000001）和**debug\_CACHE** （0x00000010）旗標，適當的 \<*追蹤旗標*> 值為**0x00000011**。

|旗標名稱 |旗標值 |旗標描述 |
| --- | --- | --- |
|**DEBUG_SEARCH** |0x00000001 |記錄搜尋要求，以及傳遞給這些要求的參數。 這裡不會記錄回應。 只會記錄搜尋要求。 （使用**DEBUG_SPEWSEARCH**來記錄搜尋要求的回應）。 |
|**DEBUG_WRITE** |0x00000002 |記錄寫入要求和傳遞至這些要求的參數。 寫入要求包括 [新增]、[刪除]、[修改] 和 [擴充] 作業。 |
|**DEBUG_REFCNT** |0x00000004 |記錄會參考連接和要求的計數資料和作業。 |
|**DEBUG_HEAP** |0x00000008 |記錄所有記憶體配置和記憶體釋放。 |
|**DEBUG_CACHE** |0x00000010 |記錄快取活動。 此活動包括新增、移除、點擊、遺漏等等。 |
|**DEBUG_SSL** |0x00000020 |記錄 SSL 資訊和錯誤。 |
|**DEBUG_SPEWSEARCH** |0x00000040 |記錄所有伺服器對搜尋要求的回應。 這些回應包括所要求的屬性，以及收到的所有資料。 |
|**DEBUG_SERVERDOWN** |0x00000080 |記錄伺服器關閉和連接錯誤。 |
|**DEBUG_CONNECT** |0x00000100 |記錄與建立連接相關的資料。<br />使用**DEBUG_CONNECTION**來記錄與連接相關的其他資料。 |
|**DEBUG_RECONNECT** |0x00000200 |記錄自動重新連接活動。 此活動包括重新連線嘗試、失敗和相關錯誤。 |
|**DEBUG_RECEIVEDATA** |0x00000400 |記錄與從伺服器接收訊息相關的活動。 此活動包括「等待伺服器回應」和從伺服器接收的回應等事件。 |
|**DEBUG_BYTES_SENT** |0x00000800 |記錄 LDAP 用戶端傳送到伺服器的所有資料。 此函式基本上是封包記錄，但它一律會記錄未加密的資料。 （如果封包是透過 SSL 傳送的，此函式會記錄未加密的封包）。這種記錄可以是詳細資訊。 此旗標可能最適合單獨使用，或與**DEBUG_BYTES_RECEIVED**結合。 |
|**DEBUG_EOM** |0x00001000 |記錄與到達訊息清單結尾相關的事件。 這些事件包括「已清除的訊息清單」等資訊。 |
|**DEBUG_BER** |0x00002000 |記錄與基本編碼規則（BER）相關的作業和錯誤。 這些作業和錯誤包含編碼、緩衝區大小問題等等的問題。 |
|**DEBUG_OUTMEMORY** |0x00004000 |記錄失敗以配置記憶體。 也會記錄任何無法計算所需記憶體的錯誤（例如，在計算所需的緩衝區大小時發生溢位）。 |
|**DEBUG_CONTROLS** |0x00008000 |記錄與控制項相關的資料。 此資料包括插入的控制項、影響控制項的問題、連接上的強制控制項等等。 |
|**DEBUG_BYTES_RECEIVED** |0x00010000 |記錄 LDAP 用戶端接收的所有資料。 此行為基本上是封包記錄，但它一律會記錄未加密的資料。 （如果封包是透過 SSL 傳送，此選項會記錄未加密的封包）。這種類型的記錄可以是詳細資訊。 此旗標可能最適合單獨使用，或與**DEBUG_BYTES_SENT**結合。 |
|**DEBUG_CLDAP** |0x00020000 |記錄 UDP 和無連接 LDAP 的特定事件。 |
|**DEBUG_FILTER** |0x00040000 |記錄當您建立搜尋篩選時所遇到的事件和錯誤。<br/>**注意**此選項只會在篩選器結構期間記錄用戶端事件。 它不會記錄有關篩選器之伺服器的任何回應。 |
|**DEBUG_BIND** |0x00080000 |記錄系結事件和錯誤。 此資料包含協調資訊、系結成功、系結失敗等等。 |
|**DEBUG_NETWORK_ERRORS** |0x00100000 |記錄一般網路錯誤。 此資料包括「傳送」和「接收」錯誤。<br/>**注意**如果連接遺失或無法連線到伺服器， **DEBUG_SERVERDOWN**是慣用的標記。 |
|**DEBUG_VERBOSE** |0x00200000 |記錄一般訊息。 針對通常會產生大量輸出的任何訊息，請使用此選項。 例如，它會記錄訊息，例如「已到達訊息結尾」、「伺服器尚未回應」等等。 此選項也適用于一般訊息。 |
|**DEBUG_PARSE** |0x00400000 |記錄一般訊息事件和錯誤，以及封包剖析和編碼事件和錯誤。 |
|**DEBUG_REFERRALS** |0x00800000 |記錄有關參考和追蹤參考的資料。 |
|**DEBUG_REQUEST** |0x01000000 |記錄追蹤要求。 |
|**DEBUG_CONNECTION** |0x02000000 |記錄一般連接資料和錯誤。 |
|**DEBUG_INIT_TERM** |0x04000000 |記錄模組初始化和清除（DLL Main 等等）。 |
|**DEBUG_API_ERRORS** |0x08000000 |支援記錄 API 的不正確使用。 例如，如果在相同的連接上呼叫系結作業兩次，此選項會記錄資料。 |
|**DEBUG_ERRORS** |0x10000000 |記錄一般錯誤。 這些錯誤大多可分類為模組初始化錯誤、SSL 錯誤，或溢位或下溢錯誤。 |
|**DEBUG_PERFORMANCE** |0x20000000 |在收到 LDAP 要求的伺服器回應之後，記錄關於進程的全域 LDAP 活動統計資料。 |

## <a name="example"></a>範例

假設有一個應用程式 App1，它會設定使用者帳戶的密碼。 假設 App1 產生未預期的錯誤。 若要使用 ETW 來協助診斷此問題，請遵循下列步驟：

1. 在 [登錄編輯程式] 中，建立下列登錄專案：

   **HKEY\_本機\_機\\系統\\CurrentControlSet\\Services\\ldap\\追蹤\\App1**

1. 若要啟動追蹤會話，請開啟 [命令提示字元] 視窗，然後執行下列命令：

   ```cmd
   tracelog.exe -start ldaptrace -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f .\ldap.etl -flag 0x80000
   ```

   在此命令啟動之後， **DEBUG\_BIND**可確保 ETW 會將追蹤訊息寫入至。\\的 ldap .etl。

1. 啟動 App1，並重現未預期的錯誤。

1. 若要停止追蹤會話，請在命令提示字元中執行下列命令：

   ```cmd
    tracelog.exe -stop ldaptrace
   ```

1. 若要防止其他使用者追蹤應用程式，請刪除**HKEY\_本機\_機**\\**系統**\\**CurrentControlSet**\\**Services**\\**ldap**\\**追蹤**\\**App1 .exe**登錄專案。

1. 若要檢查追蹤記錄檔中的資訊，請在命令提示字元中執行下列命令：

   ```cmd
    tracerpt.exe .\ldap.etl -o -report
    ```

   > [!NOTE]  
   > 在此命令中， **tracerpt**是追蹤取用[者](https://go.microsoft.com/fwlink/p/?linkid=83876)工具。
