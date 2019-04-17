---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: "LDAP 伺服器 Cookie 的處理方式"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>LDAP 伺服器 Cookie 的處理方式

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 LDAP，某些大型結果查詢會導致設定。 這類查詢對 Windows Server 造成一些問題。  
  
收集建置這些結果大集，請重要的工作。 許多屬性必須從內部表示轉換成 LDAP 花朵代表。 許多屬性，需要從內部、 通常二進位，格式轉換文字型 utf-8 格式 LDAP 回應框架中執行。  
  
另一個挑戰是結果該設定的數以萬計的物件成為龐大，輕鬆地數個數百大-位元組。 這些然後需要多很多的 virtual 地址空間及傳輸中斷的 TCP 工作階段時不會遺失整個努力在網路上的傳送也有問題。  
  
這些容量和後勤問題有 led Microsoft LDAP 開發人員來建立稱為 「 分頁查詢 「 LDAP 擴充功能。 其實 LDAP 控制龐大查詢分成的較小的結果集區塊。 變得更 RFC 標準為[RFC 2696](http://www.ietf.org/rfc/rfc2696)。  
  
## <a name="cookie-handling-on-client"></a>Cookie Client 上處理  
分頁查詢方法使用頁面大小任一設 client，或透過[LDAP 原則](https://support.microsoft.com/kb/315071/en-us)(」 MaxPageSize 」)。 隨時 client 需要藉由傳送 LDAP 控制項讓分頁。  

  
使用許多結果查詢時，有些時候允許的物件已達上限。 LDAP 伺服器上回應訊息封裝，並將 cookie 包含之後繼續搜尋所需的資訊。  
  
Client 應用程式必須視為透明 blob cookie。 它可以擷取物件計數回應，可以繼續基於 cookie 的搜尋]。Client 繼續搜尋查詢傳送 LDAP 伺服器再試一次使用相同的基本物件及篩選器，例如參數，包含在上一個回應傳回 cookie 值。  
  
如果物件數量不會填滿頁面，請 LDAP 查詢已完成，而且回應包含不頁面上的 cookie。 如果不 cookie 伺服器傳回，client 必須考慮成功完成分頁的搜尋。  
  
伺服器傳回錯誤，如果 client 必須請考慮將會失敗分頁的搜尋。 重試一次搜尋] 會造成從第一頁搜尋。  
  
## <a name="server-side-cookie-handling"></a>伺服器端 Cookie 處理  
Windows Server 回到 client cookie 和有時會儲存在伺服器上的 cookie 相關的資訊。 此資訊會儲存在伺服器上的快取中，皆受特定限制。  
  
此時，請傳送到 client 的伺服器 cookie 也會使用伺服器以查詢快取的伺服器上的資訊。 當 client 持續分頁的搜尋時，Windows Server 會使用 client cookie，以及任何相關的資訊伺服器 cookie 快取的繼續搜尋。 如果伺服器找不到任何原因伺服器快取 cookie 相關的資訊，已不再提供搜尋和錯誤到 client。  
  
## <a name="how-the-cookie-pool-is-managed"></a>如何管理 cookie 集區  
當然，LDAP 伺服器服務一次以上 client，也更多個 client 一次可以舉辦查詢需要伺服器 cookie 快取的使用。因此的 Windows Server 實作 cookie 集區使用量追蹤且 cookie 集區不花太多資源，限制放入定位。 限制可以設定的系統管理員使用下列設定 LDAP 原則。 解釋與預設值︰  
  
**MinResultSets: 4**  
  
如果有小於 MinResultSets 伺服器 cookie 快取中的項目，如下所示大集區大小不會看到 LDAP 伺服器。  
  
**MaxResultSetSize: 262,144 位元組**  
  
在伺服器上的 cookie 總快取大小不得超過 MaxResultSetSize 位元組最大值。 若是如此，請從舊的 cookie 刪除集區小於 MaxResultSetSize 位元組或小於 MinResultSets cookie 的集區中。 這表示過使用預設設定，LDAP 伺服器視為 450 KB 為確定如果只 3 cookie 儲存集區。  
  
**MaxResultSetsPerConn: 10**  
  
LDAP 伺服器可不超過 MaxResultSetsPerConn cookie 每 389 集區中。  
  
## <a name="handling-deleted-cookies"></a>處理刪除 Cookie  
移除 cookie 從 LDAP Server 快取的資訊不會導致立即所有案例中的應用程式的錯誤。 應用程式可能會重新分頁的搜尋從 [開始] 畫面和上嘗試另一個將它完成。 某些應用程式有此類型重試機制來新增穩定性。  
  
某些應用程式可能瀏覽網頁搜尋並不會將它完成。 這可能會保留 LDAP 伺服器中的項目 cookie 快取，透過 4 一節中的機制。 這是必要釋出的作用中 LDAP 搜尋伺服器上的記憶體。  
  
這類 cookie 刪除伺服器上 client 繼續使用此 cookie 控點搜尋時的行為？LDAP 伺服器會不找到伺服器 cookie 快取 cookie 並傳回查詢錯誤，會類似錯誤回應：  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> 「 DSID 「 背後的十六進位值會根據組建 LDAP 伺服器二進位版本而有所不同。  
  
## <a name="reporting-on-the-cookie-pool"></a>Cookie 集區報告  
LDAP 伺服器已登入事件通過分類 「 16 Ldap 介面 」 功能[NTDS 診斷鍵](https://support.microsoft.com/kb/314980/en-us)。 如果您設定的這個分類 」 2 」，您可以取得下列事件：  
  
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
  
事件訊號移除了儲存的 cookie。 並不代表 client 已經看過 LDAP 錯誤，但僅限 LDAP 伺服器人數已達快取的管理限制。  有時候，LDAP client 可能會有放棄分頁的搜尋，可能不會看到此錯誤。  
  
## <a name="monitoring-the-cookie-pool"></a>Cookie 集區的監視  
如果您不會在您的網域體驗 LDAP 搜尋錯誤，您不需要監視 LDAP 伺服器頁面搜尋 cookie 集區。 如果您看到您的環境中搜尋相關的錯誤 LDAP 頁面，您可能會有 cookie 集區的系統管理員限制的問題。  
  
事件 2898年和 2899年是知道您已經到達 LDAP 伺服器管理員限制的唯一方式。 您體驗時出該 LDAP 查詢錯誤而處理錯誤上述控制項，您應該查看增加限制一或多個 4，您收到的事件根據一節中所提到的 LDAP 原則設定。  
  
如果您看到事件 2898年俠日 LDAP 伺服器上，我們建議您 25 MaxResultSetsPerConn 設定。 在單一 389 平行分頁的搜尋 25 個以上不是平常。 如果您看到事件 2898年繼續，請考慮調查 LDAP client 應用程式，發生錯誤。 懷疑就是，它日子卡住擷取額外分頁的結果、 離開擱置中的 cookie，新的查詢重新開機。 請查看是否應用程式會有些時候有不足 cookie 其為了，您也可以增加 MaxResultSetsPerConn 25.以外的值，當您看到的身分登入您的網域控制站事件 2899年、 計劃會不同。 如果您俠日 LDAP 伺服器記憶體不足 (數個 Gb 的可用記憶體) 的電腦上執行，建議您設定 MaxResultsetSize LDAP 伺服器上 > = 有 250 MB。 這項限制足以容納大量 LDAP 網頁搜尋非常大型目錄即使是在。  
  
如果您仍然看到事件 2899年有 250 MB 或更多的集區，您可能會有許多戶端傳回的物件更高的數字，，查詢非常常用的方式。 您可以使用收集的資料[Active Directory 資料收集設定](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx)可協助您尋找可讓您 LDAP 伺服器重複分頁的查詢忙碌。 這些查詢將會有數字的 「 退貨項目 」 符合使用的頁面的大小顯示。  
  
如果可能的話，您應該檢視應用程式的設計，並實作不同的頻率較低、 資料音量和/或較少 client 執行個體查詢此資料的方法。在您擁有的來源的程式碼存取此指南的應用程式[建立有效率 AD-Enabled 應用程式](https://msdn.microsoft.com/en-us/library/ms808539.aspx)可協助您了解應用程式可以存取廣告的最佳方式。  
  
如果您無法變更查詢行為，其中一種方法新增所需的命名內容並可以轉散發戶端及最後減少個人 LDAP 伺服器上的載入更多複寫執行個體。  
  


