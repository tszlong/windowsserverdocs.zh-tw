---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "事件 ID 2088-DNS 查詢失敗複寫成功"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件 ID 2088: DNS 查詢時發生錯誤，而且複寫成功

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

    
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

無效的 DNS 設定可能會影響其他成員電腦、網域控制站或應用程式包括登入驗證或存取網路資源這個 Active Directory Domain Services 樹系的伺服器上的基本操作。 

您應該立即解析這個 DNS 設定錯誤，此網域控制站可以解析使用 DNS 來源網域控制站的 IP 位址。 

另一部名稱：DC1 失敗 DNS 主機名稱：4a8717eb-8e58-456 c-995a-c92e4add7e8e._msdcs.contoso.com 

注意：預設，只是最多 10 DNS 失敗會顯示為任何特定 12 小時，即使發生超過 10 失敗。  登入的所有個人失敗事件，下列診斷登錄將值設定為 1: 

登錄路徑：HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

使用者動作： 

1) 如果不能來源網域控制站運作或的作業系統已使用不同的電腦名稱重新安裝或 NTDSDSA 物件 GUID，移除來源網域控制站中繼資料的 ntdsutil.exe，使用 MSKB 文章 216498 中的步驟。 

2) 確認來源網域控制站執行 Active Directory 已存取網路上，輸入「net 檢視 \\&lt;來源俠名稱&gt;」或「ping&lt;來源俠名稱&gt;」。 

3) 確認來源網域控制站使用有效的 DNS 伺服器的 DNS 服務和來源網域控制站主機記錄和 CNAME 記錄正確登記，使用 DNS 增強 DCDIAG.EXE https://www.microsoft.com/dns 提供 

dcdiag//test: dns 

4) 請確認此目的地網域控制站正在使用 DNS 服務，有效的 DNS 伺服器執行 DNS 增強 DCDIAG.EXE 命令目的地網域控制站在主機上，如下所示： 

dcdiag//test: dns 

5) 適用於進一步的分析的 DNS 錯誤查看 KB 824449: https://support.microsoft.com/?kbid=824449 

其他資料錯誤值：11004 要求的名稱是有效的但是找不到要求類型的資料</code>
  </introduction>
  <section>
    <title>診斷</title>
    <content>
      <para>無法，使用 DNS (CNAME) 別名資源記錄解析來源網域控制站的名稱可能是因為 DNS 錯誤設定或 DNS 資料傳播的延遲。</para>
    </content>
  </section>
  <section>
    <title>解析度</title>
    <content>
      <para>繼續使用 DNS 測試中所述]<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">263 2087 年: DNS 查詢失敗造成︰ 複寫失敗</link>。」</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


