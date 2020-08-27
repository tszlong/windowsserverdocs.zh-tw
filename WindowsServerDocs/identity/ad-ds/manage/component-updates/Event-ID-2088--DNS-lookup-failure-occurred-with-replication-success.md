---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 事件識別碼 2088-複寫成功時發生 DNS 查閱失敗
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a6e78c458f92e477ddeaf156370f5e254caf4679
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941338"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件識別碼 2088：發生 DNS 查閱錯誤但複寫成功

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當執行 Windows Server 2003 （含 Service Pack 1）的目的地網域控制站 (SP1) 接收到目錄服務事件記錄檔中的事件識別碼2088時，會嘗試將別名 (CNAME) a d a d a d a d a a d) 中的全域唯一 (識別碼，解析為來源網域控制站的 IP 位址失敗。 不過，目的地網域控制站已嘗試使用完整功能變數名稱 (FQDN) 或來源網域控制站的 NetBIOS 名稱，來解析該名稱並成功的方法。 複寫成功時，應診斷並解決網域名稱系統 (DNS) 問題。

以下是事件文字的範例：

```
Log Name: Directory Service
Source: Microsoft-Windows-ActiveDirectory_DomainService
Date: 3/15/2008  9:20:11 AM
Event ID: 2088
Task Category: DS RPC Client
Level: Warning
Keywords: Classic
User: ANONYMOUS LOGON
Computer: DC3.contoso.com
Description:
Active Directory could not use DNS to resolve the IP address of the source domain controller listed below. To maintain the consistency of Security groups, group policy, users and computers and their passwords, Active Directory Domain Services successfully replicated using the NetBIOS or fully qualified computer name of the source domain controller.
```

DNS 設定無效可能會影響此 Active Directory Domain Services 樹系中成員電腦、網域控制站或應用程式伺服器上的其他重要作業，包括登入驗證或存取網路資源。

您應該立即解決此 DNS 設定錯誤，讓此網域控制站可以使用 DNS 解析來源網域控制站的 IP 位址。

替代伺服器名稱： DC1 失敗的 DNS 主機名稱：4a8717eb-8e58-456c-995a-c92e4add7e8e。 _msdcs .com

注意：根據預設，任何指定的12小時期間內只會顯示最多10個 DNS 失敗，即使發生10次以上的失敗也是如此。  若要記錄所有個別的失敗事件，請將下列診斷登錄值設定為1：

登錄路徑： HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC 用戶端

使用者動作：

1) 如果來源網域控制站無法正常運作，或已使用不同的電腦名稱稱或 NTDSDSA 物件 GUID 重新安裝其作業系統，請使用 MSKB 文章216498中所述的步驟，將來源網域控制站的中繼資料與 ntdsutil.exe 移除。

2) 確認來源網域控制站正在執行 Active Directory 並且可在網路上輸入 "net view \\ <source DC name> " 或 "ping" 來存取 <source DC name> 。

3) 請確認來源網域控制站是否使用 DNS 服務的有效 DNS 伺服器，以及是否已正確登錄來源網域控制站的主機記錄和 CNAME 記錄，並使用提供的 DNS 增強版 DCDIAG.EXE <https://www.microsoft.com/dns>

dcdiag/test： dns

4) 藉由在目的地網域控制站的主控台上執行 DNS 增強版的 DCDIAG.EXE 命令，確認此目的地網域控制站使用 DNS 服務的有效 DNS 伺服器，如下所示：

dcdiag/test： dns

5) 如需 DNS 錯誤失敗的進一步分析，請參閱 KB 824449： <https://support.microsoft.com/?kbid=824449>

其他資料錯誤值：11004要求的名稱有效，但是找 </code> 不到要求類型的資料 </introduction>
  <section>
    <title>診斷</title>
    <content>
      <para>在 dns 中使用別名 (CNAME) 資源記錄時，無法解析來源網域控制站名稱，可能是 dns 資料傳播的 DNS 錯誤或延遲所造成。</para>
    </content>
  </section>
  <section>
    <title>解決方法</title>
    <content>
      <para>繼續進行 DNS 測試，如 &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件識別碼2087中所述： dns 查閱失敗導致複寫失敗</link>。&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>
