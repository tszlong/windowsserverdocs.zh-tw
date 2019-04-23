---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 事件識別碼 2088-DNS 查閱失敗，發生複寫成功
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: cc090fa749a601e53b4347cce43245f22badc8ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840709"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件識別碼 2088年:DNS 查閱失敗，發生複寫成功

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

    
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

無效的 DNS 設定可能會影響其他成員電腦、 網域控制站或此 Active Directory 網域服務樹系，包括登入驗證或存取網路資源的應用程式伺服器上的基本作業。 

您應該立即解決此 DNS 設定錯誤，好讓此網域控制站可以解析使用 DNS 的來源網域控制站的 IP 位址。 

替代的伺服器名稱：DC1 容錯的 DNS 主機名稱：4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

注意：根據預設，只有最多 10 個 DNS 失敗會顯示任何指定 12 小時，即使發生超過 10 個失敗。  若要記錄所有的個別失敗事件，設定下列診斷登錄值設為 1: 

登錄路徑：HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

使用者動作： 

1) 如果來源網域控制站不再運作或其作業系統重新安裝使用不同的電腦名稱或 NTDSDSA 物件 GUID，移除來源網域控制站的中繼資料，使用 ntdsutil.exe，MSKB 文章中所述的步驟216498。 

2) 確認來源網域控制站正在執行 Active Directory，而且是可存取網路上，輸入"net 檢視\\&lt;來源 DC 名稱&gt;"或"ping&lt;來源 DC 名稱&gt;"。 

3) 確認來源網域控制站會使用有效的 DNS 伺服器，DNS 服務和來源網域控制站的主機記錄和 CNAME 記錄正確註冊，請使用 DCDIAG 的 DNS 增強型版本。上可用的 EXE https://www.microsoft.com/dns 

dcdiag /test:dns 

4) 確認此目的地網域控制站使用有效的 DNS 伺服器的 DNS 服務，執行 DCDIAG 的 DNS 增強型版本。在主控台中，如下所示的目的地網域控制站的 EXE 命令： 

dcdiag /test:dns 

5) 以便進一步分析 DNS 錯誤失敗，請參閱 KB 824449: https://support.microsoft.com/?kbid=824449 

其他資料錯誤值：11004 要求的名稱有效，但找不到要求類型的任何資料</code> </introduction>
  <section>
    <title>診斷</title>
    <content>
      <para>無法解析在 DNS 中使用的別名 (CNAME) 資源記錄的來源網域控制站名稱可能是因為 DNS 設定錯誤或 DNS 資料傳播延遲。</para>
    </content>
  </section>
  <section>
    <title>解決方式</title>
    <content>
      <para>繼續進行 DNS 測試中所述"<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件識別碼 2087年:DNS 查閱失敗造成複寫失敗</link>。 」</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


