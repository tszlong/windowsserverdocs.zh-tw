---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 事件識別碼 2088-複寫成功時發生 DNS 查閱失敗
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d51cbcc93a8decbcb72a1e91854a09345507511d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368906"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件識別碼 2088：發生 DNS 查閱錯誤但複寫成功

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

DNS 設定無效可能會影響成員電腦、網域控制站或此 Active Directory Domain Services 樹系中的應用程式伺服器上的其他必要作業，包括登入驗證或網路資源的存取。 

您應該立即解決此 DNS 設定錯誤，讓此網域控制站可以使用 DNS 來解析來源網域控制站的 IP 位址。 

替代伺服器名稱： DC1 失敗的 DNS 主機名稱： 4a8717eb-8e58-456c-995a-c92e4add7e8e _msdcs. contoso .com 

注意：根據預設，即使發生超過10次失敗，任何指定的12小時內，最多隻會顯示10個 DNS 失敗。  若要記錄所有個別失敗事件，請將下列診斷登錄值設定為1： 

登錄路徑： HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC 用戶端 

使用者動作： 

1) 如果來源網域控制站已不再正常運作，或其作業系統已使用不同的電腦名稱稱或 NTDSDSA 物件 GUID 重新安裝，請使用 MSKB 文章中所述的步驟，移除來源網域控制站的中繼資料與 ntdsutil.exe216498。 

2) 確認來源網域控制站正在執行 Active Directory 且可在網路上存取，方法是輸入 "net view \\&lt;來源 DC 名稱&gt;" 或 [ping &lt;來源 DC 名稱&gt;]。 

3) 請確認來源網域控制站使用 DNS 服務的有效 DNS 伺服器，而且來源網域控制站的主機記錄和 CNAME 記錄已正確登錄，並使用 DNS 增強版的 DCDIAG。<https://www.microsoft.com/dns> 上可用的 EXE 

dcdiag/test： dns 

4) 藉由執行 DNS 增強版的 DCDIAG，確認這個目的地網域控制站使用 DNS 服務的有效 DNS 伺服器。在目的地網域控制站的主控台上執行 EXE 命令，如下所示： 

dcdiag/test： dns 

5) 如需 DNS 錯誤失敗的進一步分析，請參閱 KB 824449： <https://support.microsoft.com/?kbid=824449> 

其他資料錯誤值：11004要求的名稱有效，但找不到所要求類型的資料</code> </introduction>
  <section>
    <title>診斷</title>
    <content>
      <para>若無法使用 DNS 中的別名（CNAME）資源記錄來解析來源網域控制站名稱，可能是因為 dns 的錯誤或 DNS 資料傳播中的延遲所致。</para>
    </content>
  </section>
  <section>
    <title>解決</title>方式
    <content>
      <para>如 &quot;<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件識別碼2087： dns 查閱失敗</link>中所述，繼續進行 DNS 測試，導致複寫失敗。&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


