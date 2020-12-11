---
description: 深入瞭解：如何處理 LDAP 伺服器 Cookie
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: 如何處理 LDAP 伺服器 Cookie
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5b4526bc195b2c4c17c1c2b39ece1a6064aa281e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049496"
---
# <a name="how-ldap-server-cookies-are-handled"></a>如何處理 LDAP 伺服器 Cookie

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 LDAP 中，某些查詢會產生大型結果集。 此類查詢會對 Windows Server 帶來一些挑戰。

收集和建置這些大型結果集是很重要的工作。 許多屬性必須從內部表示法轉換為 LDAP 有線表示法。 對於許多屬性，必須從通常為二進位的內部格式轉換為 LDAP 回應框架中的 UTF-8 文字格式。

另一個挑戰是擁有成千上萬個物件的結果集會變得很大，至少有數百 MB。 接著它們需要大量虛擬位址空間，而透過網路傳輸也存在問題，因為當 TCP 工作階段在傳輸過程中斷時，等於一切努力都白費了。

這些容量和羅吉斯問題促使 Microsoft LDAP 開發人員建立稱為「分頁查詢」的 LDAP 延伸模組。 此延伸模組會實作一個 LDAP 控制項，將一個大型查詢拆開成為較小型的結果集區塊。 它已成為 rfc [2696](http://www.ietf.org/rfc/rfc2696)的 rfc 標準。

## <a name="cookie-handling-on-client"></a>用戶端的 Cookie 處理
分頁查詢方法會使用用戶端設定的頁面大小，或透過 [LDAP 原則](https://support.microsoft.com/kb/315071/en-us) ( "MaxPageSize" ) 。 用户端一律需要透過傳送 LDAP 控制項才能啟用分頁。


在處理具有許多結果的查詢時，某些時候會達到允許的物件數目上限。 LDAP 伺服器會將回應訊息封裝起來，再加上一個 Cookie，其中包含稍後繼續搜尋所需的資訊。

用户端應用程式必須將 Cookie 視為不透明的 Blob。 它可以擷取回應中的物件計數，也能根據存在的 Cookie 繼續執行搜尋。用户端透過將具備相同參數 (例如基本物件和篩選器) 的查詢再次傳送給 LDAP 伺服器，並加入前次回應傳回的 Cookie 值，即可繼續搜尋。

如果物件數目未填滿頁面，LDAP 查詢就會完成，回應不會包含任何頁面 cookie。 如果伺服器未傳回任何 Cookie，用户端必須將分頁搜尋視為成功完成。

如果伺服器傳回錯誤，用户端必須將分頁搜尋視為不成功。 重試搜尋會導致從第一頁重新開始搜尋。

## <a name="server-side-cookie-handling"></a>伺服器端的 Cookie 處理
Windows Server 將 Cookie 傳回給用户端，有時候會在伺服器上儲存 Cookie 相關資訊。 此項資訊儲存在伺服器的快取中，受到某些限制。

在這種情況下，伺服器也會使用從伺服器傳送到用户端的 Cookie 來查閱來自伺服器快取中的資訊。 當用户端繼續進行分頁搜尋時，Windows Server 會使用用户端 Cookie 以及來自伺服器 Cookie 快取中的任何相關資訊，繼續進行搜尋。 如果伺服器因故找不到來自伺服器快取的相關 Cookie 資訊，則會停止搜尋，並傳回錯誤給用户端。

## <a name="how-the-cookie-pool-is-managed"></a>如何管理 Cookie 集區
很顯然，LDAP 伺服器一次不只服務一個用戶端，同一時間也會有多個用戶端啟動需要用到伺服器 Cookie 快取的查詢。因此 Windows Server 實作會追蹤 Cookie 集區使用情況，並加上限制條件，讓 Cookie 集區不致佔用過多資源。 系統管理員可以使用 LDAP 原則中的下列設定，來設定限制。 預設值和說明如下：

**MinResultSets：4**

如果伺服器 Cookie 快取中的項目數少於 MinResultSets，則 LDAP 伺服器不會查看下面討論的最大集區大小。

**MaxResultSetSize：262,144 位元組**

伺服器上的 Cookie 快取大小總計不得超過 MaxResultSetSize 的最大值 (以位元組為單位)。 如果超過，則會從最舊的 Cookie 開始刪除，直到集區小於 MaxResultSetSize 位元組或集區中少於 MinResultSets 個 Cookie。 這表示在預設設定下，LDAP 伺服器會認為只儲存了 3 個 Cookie 的 450KB 集區是適當的。

**MaxResultSetsPerConn：10**

LDAP 伺服器不允許集區中每個 LDAP 連線超過 MaxResultSetsPerConn 個 Cookie。

## <a name="handling-deleted-cookies"></a>處理删除的 Cookie
在所有情況下，從 LDAP 伺服器快取移除 Cookie 資訊都不會對應用程式造成立即錯誤。 應用程式可能會重頭開始進行分頁搜尋，並在再一次嘗試後完成。 某些應用程式具有這種重試機制，以增加穩定性。

某些應用程式可能會進行一次頁面搜尋，但永遠無法完成。 這可能會在 LDAP 伺服器 Cookie 快取中留下項目，這由第 4 節的機制來處理。 這對於釋放伺服器的記憶體以供作用中 LDAP 搜尋使用相當重要。

如果從伺服器刪除了此類 Cookie，而用戶端繼續使用此 Cookie 控制代碼搜尋時，會發生什麼事？LDAP 伺服器在伺服器 Cookie 快取中將找不到 Cookie，並會傳回查詢錯誤，此錯誤回應將類似於：

```
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1
```

> [!NOTE]
> "DSID" 後面的十六進位值會根據 LDAP 伺服器二進位檔的組建版本而有所不同。

## <a name="reporting-on-the-cookie-pool"></a>報告 Cookie 集區
LDAP 伺服器能夠透過 [NTDS 診斷機碼](https://support.microsoft.com/kb/314980/en-us)中的 "16 LDAP Interface" 類別來記錄事件。 如果您將此類別設定為 "2"，您可以取得下列事件：

```
Log Name:      Directory Service
Source:        Microsoft-Windows-ActiveDirectory_DomainService
Event ID:      2898
Task Category: LDAP Interface
Level:         Information
Description:
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.
Maximum number of Result Sets allowed per LDAP connection:
10
Current number of Result Sets for this LDAP connection:
11

User Action
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.

```

```
Log Name:      Directory Service
Source:        Microsoft-Windows-ActiveDirectory_DomainService
Event ID:      2899
Task Category: LDAP Interface
Level:         Information
Description:
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.

Number of result sets currently stored:
4
Current Result Set Size:
263504
Maximum Result Set Size:
262144
Size of single Result Set being discarded:
40876
User Action
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.

```

這些事件指出儲存的 Cookie 已删除。 它並不表示用戶端已經收到 LDAP 錯誤，只表示 LDAP 伺服器已達到快取的系統管理限制。  在某些情況下，LDAP 用戶端可能已放棄分頁搜尋，永遠看不到這個錯誤。

## <a name="monitoring-the-cookie-pool"></a>監視 Cookie 集區
如果您的網域中從未出現過 LDAP 搜尋錯誤，您可能不需要監視 LDAP 伺服器頁面搜尋 Cookie 集區。 如果您的環境中出現 LDAP 頁面搜尋相關錯誤，可能表示 Cookie 集區系統管理員限制有問題。

事件 2898 和 2899 是唯一可得知 LDAP 伺服器是否已達到系統管理員限制的方式。 如果您的 LDAP 查詢錯誤是由於上述控制項處理錯誤而發生，您應根據所取得的事件，查看第 4 節所述的「提高一或多個 LDAP 原則設定的限制」。

如果您的 DC/LDAP 伺服器上出現事件 2898，建議您將 MaxResultSetsPerConn 設為 25。 單一 LDAP 連線中超過 25 個並行分頁搜尋並不常見。 如果您持續看到事件 2898，請調查發生錯誤的 LDAP 用戶端應用程式。 有可能在擷取其他分頁結果時停滯住了，因而讓 Cookie 擱置，並重新啟動一個新查詢。 所以請查看應用程式在某個時候是否具有足夠的 Cookie 可供使用，您也可以將 MaxResultSetsPerConn 的值提高到 25 以上。如果是網域控制站上記錄了事件 2899，處理方法將有所不同。 如果您的 DC/LDAP 伺服器是在具有足夠記憶體的電腦上執行 (數 Gb 的可用記憶體) ，建議您將 LDAP 伺服器上的 Maxresultsetsize 262144 設定為 >= 250MB。 此上限已夠大，足以容納在極大目錄上進行的大量 LDAP 頁面搜尋。

如果在 250MB 以上的集區中仍然出現事件 2899，則您可能有許多用戶端以很頻繁的方式進行查詢，並傳回極大量物件。 您可以使用 [Active Directory 資料收集器集合](/archive/blogs/askds/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond) 人員收集的資料，可協助您找出可讓 LDAP 伺服器保持忙碌的重複分頁查詢。 這些查詢都會顯示符合所使用頁面大小的「傳回的專案數」。

可能的話，您應該檢查應用程式設計，並以較低的頻率、資料量及/或較少的用戶端實例來查詢此資料來實行不同的方法。如果您具有原始程式碼存取權的應用程式，本指南  [建立有效率的 AD-Enabled 應用程式](/previous-versions/ms808539(v=msdn.10)) ，可協助您瞭解應用程式存取 AD 的最佳方式。

如果無法變更查詢行為，其中一個方法也會新增所需命名內容的複寫實例，並重新發佈用戶端，最後減少個別 LDAP 伺服器上的負載。

