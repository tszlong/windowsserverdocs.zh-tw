---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 事件識別碼 2088-複寫成功時發生 DNS 查閱失敗
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9dbb7debbca8d1625ebe975a051ed8b607d1ddd0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943283"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件識別碼 2088：發生 DNS 查閱錯誤但複寫成功

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當執行 Windows Server 2003 Service Pack 1 (SP1 的目的地網域控制站) 在目錄服務事件記錄檔中收到事件識別碼2088時，會嘗試將別名 (CNAME) 資源記錄中的全域唯一識別碼 (GUID) ，解析為來源網域控制站的 IP 位址失敗。 不過，目的地網域控制站會嘗試使用完整功能變數名稱 (FQDN) 或來源網域控制站的 NetBIOS 名稱來解析名稱和成功。 雖然複寫成功，但應診斷並解決網域名稱系統 (DNS) 問題。

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

DNS 設定無效可能會影響成員電腦、網域控制站或此 Active Directory Domain Services 樹系中的應用程式伺服器上的其他必要作業，包括登入驗證或網路資源的存取。

您應該立即解決此 DNS 設定錯誤，讓此網域控制站可以使用 DNS 來解析來源網域控制站的 IP 位址。

替代伺服器名稱： DC1 失敗的 DNS 主機名稱： 4a8717eb-8e58-456c-995a-c92e4add7e8e _msdcs. contoso .com

注意：根據預設，即使發生超過10次失敗，任何指定的12小時內，最多隻會顯示10個 DNS 失敗。  若要記錄所有個別失敗事件，請將下列診斷登錄值設定為1：

登錄路徑： HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC 用戶端

使用者動作：

1) 如果來源網域控制站已不再運作，或其作業系統已經使用不同的電腦名稱稱或 NTDSDSA 物件 GUID 重新安裝，請使用 MSKB 文章216498中所述的步驟，移除來源網域控制站的中繼資料與 ntdsutil.exe。

2) 請確認來源網域控制站正在執行 Active Directory 而且可以在網路上透過輸入 "net view \\ <source DC name> " 或 "ping" 來存取 <source DC name> 。

3) 請確認來源網域控制站使用 DNS 服務的有效 DNS 伺服器，且已正確登錄來源網域控制站的主機記錄和 CNAME 記錄，並使用中可用的 DNS 增強版 DCDIAG.EXE<https://www.microsoft.com/dns>

dcdiag/test： dns

4) 藉由在目的地網域控制站的主控台上執行 DCDIAG.EXE 命令的 DNS 增強型版本，確認這個目的地網域控制站使用 DNS 服務的有效 DNS 伺服器，如下所示：

dcdiag/test： dns

5) 如需 DNS 錯誤失敗的進一步分析，請參閱 KB 824449：<https://support.microsoft.com/?kbid=824449>

其他資料錯誤值：11004要求的名稱有效，但找 </code> 不到要求類型的資料</introduction>
  <section>
    <title>診斷</title>
    <content>
      <para>若無法使用別名解析來源網域控制站名稱 (CNAME) 資源記錄，可能是因為 dns 的錯誤或 dns 資料傳播中的延遲所致。</para>
    </content>
  </section>
  <section>
    <title>解決方法</title>
    <content>
      <para>如 &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件識別碼2087： dns 查閱失敗</link>中所述，繼續進行 DNS 測試，導致複寫失敗。&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>
