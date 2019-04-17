---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: "複製錯誤 1396 年登入失敗目標帳號不正確"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 84799de26e1260f914d9b959357d5eed6fef62f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>複製錯誤 1396 年登入失敗目標帳號不正確

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>此文章將描述，包括症狀原因，以及如何解析 Active Directory 複寫失敗 Win32 錯誤 1396 年:「登入失敗：目標帳號不正確。」</para><list class="bullet"><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">症狀</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">造成</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">解析度</link></para></listItem></list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症狀</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>的測試 Active Directory 複寫失敗，錯誤 1396 DCDIAG 報告：登入失敗：目標帳號不正確。」</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN。EXE 報告的最後一個︰ 複寫失敗 1396 年狀態。</para><para>REPADMIN 命令通常引用 1396 年狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN 命令</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN 進行</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>範例描述輸入的複寫失敗，以 CONTOSO DC1 CONTOSO-DC2 從「REPADMIN 進行」的輸出「登入失敗：目標帳號不正確。」錯誤如下所示：110::</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para><ui>複製現在</ui>命令 Active Directory 網站和服務會傳回」登入失敗：目標帳號不正確。」</para><para>連接物件來源俠上按一下滑鼠右鍵，然後選擇<ui>複製現在</ui>會失敗，且「登入失敗：目標帳號不正確。」螢幕上顯示如下的錯誤訊息：</para><para>對話方塊的標題文字：</para><para>立即複寫</para><para>對話的訊息文字：</para><para>下列錯誤同步命名操作時發生&lt;磁碟分割 DNS 路徑&gt;網域控制站的&lt;來源俠&gt;網域控制站&lt;目標 DC&gt;：登入失敗：目標帳號不正確。將不會繼續這項操作。</para></listItem><listItem><para>NTDS KCC、NTDS 一般或 Microsoft-Windows-ActiveDirectory_DomainService 事件 1396 年狀態的登入 Directory 服務登入事件檢視器中。</para><para>Active Directory 事件通常引用 1396 年狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件編號</para></TD><TD><para>事件來源</para></TD><TD><para>事件字串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services 安裝精靈（帶領）無法使用下列的網域控制站連接。</para></TD></tr><tr><TD><para>1645</para><para>此事件列出的三個部分 SPN。</para></TD><TD><para>NTDS 複寫</para></TD><TD><para>Active Directory 有其他網域控制站進行驗證遠端程序呼叫 (RPC)，因為解析 SPN 金鑰 Distribution 中心 (KDC) 網域控制站在不登記目的地網域控制站您想要的服務主體名稱 (SPN)。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services 嘗試使用下列的通用通訊，嘗試已失敗。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>認知一致性檢查位於複寫本機唯讀 directory 服務連接，嘗試從遠端下列 directory 服務執行個體上更新。 操作失敗。 它將會重試。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>建立下列寫入 directory 磁碟分割的連結︰ 複寫失敗。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試使用下列的參數，無法建立複寫唯讀 directory 磁碟分割的連結。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> 伺服器無法在 DNS 登記其名稱。</para></TD></tr></tbody></table></listItem><listItem><para>帶領失敗，錯誤螢幕小</para><para>對話的標題文字：</para><para>Active Directory 安裝失敗</para><para>對話方塊訊息文字：</para><para>操作失敗：Directory 服務無法建立 DATA-CN 伺服器物件 = NTDS 設定 DATA-CN = ServerBeingPromoted，DATA-CN = 伺服器，DATA-CN = 網站 DATA-CN = DATA-CN 的網站，= 設定，俠 = contoso 俠 = com ReplicationSourceDC.contoso.com 伺服器上。</para><para>請確保所提供的網路認證有不足存取新增複本。</para><para> [登入失敗：目標帳號不正確。」</para><para>在這種情形下，1645 年、1168，以及 1125 年事件 ID 登入正在升級的伺服器。</para></listItem><listItem><para>地圖磁碟機，使用<embeddedLabel>網路使用</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在此案例，也登入系統事件登入的事件編號 333 伺服器可並使用高空間不足的應用程式，例如 SQL Server。</para></listItem><listItem><para>DC 次不正確。</para></listItem><listItem><para>\ [KDC 將不開始 RODC krbtgt account 還原後的 RODC，必須被。例如，還原，錯誤 1396 年就會出現。</para><para>事件 ID 1645 登入 RODC。</para><para>也會 Dcdiag 報告，就無法更新 RODC krbtgt account 錯誤。</para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>造成</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>SPN 不存在於通用代表嘗試驗證使用 Kerberos client KDC 搜尋。</para>
          <para>複寫 Active Directory 處在，Kerberos client 是目的 DC，執行 SPN 查詢 KDC 可能目的本身 DC，但可能會遠端 DC。</para>
        </listItem>
        <listItem>
          <para>使用者或服務 account 應包含主體名稱所尋找的服務不存在於通用代表目的地俠想複製 KDC 搜尋。</para>
          <para>複寫 Active Directory 處在，來源俠電腦 account 不存在於通用 DC 代表目的地俠執行輸入複寫搜尋。</para>
        </listItem>
        <listItem>
          <para>目的 DC 缺少來源 Dc 網域 LSA 密碼。</para>
        </listItem>
        <listItem>
          <para>SPN 正在尋找存在於不同的電腦 account 比俠來源。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解析度</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>檢查 NTDS 複寫事件 1645 年目的 DC Directory 服務事件登入，並記下下列：</para>
          <para>的目標 DC 名稱</para>
          <para>SPN 正在尋找 (E3514235-4B06-11D1-AB04-00C04FC2DCD2 日&lt;物件 guid 來源網域控制站 NTDS 設定物件&gt;/&lt;目標網域&gt;。&lt;tld&gt;@&lt;目標網域&gt;。&lt;tld&gt;</para>
          <para>所使用的目的地俠 KDC</para>
        </listItem>
        <listItem>
          <para>從 KDC 步驟 1 中的「主控台中，輸入：</para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>執行緊接目的地 DC 1396 年錯誤而失敗複寫嘗試 NLTEST 定位器測試。</para>
          <para>這應該找出 KDC 執行 SPN 查詢針對該 GC。</para>
          <para>GC KDC 搜尋可能也會在 Microsoft 的 Windows-ActiveDirectory_DomainService 事件 1655 年擷取。</para>
        </listItem>
        <listItem>
          <para>SPN 發現通用中執行「步驟 2 發現在步驟 1 中搜尋。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或者</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>主機物件 spn 存在的驗證。</para>
          <para>確認主機物件，包括是否是 CNF / 受損衝突，或位於找回容器 DN 路徑。</para>
          <para>確認來源網域控制站 Active Directory 複寫 SPN 係只在 Dc 電腦 account 來源。</para>
          <para>如果複寫 SPN，判斷是否來源 DC 已經登記 SPN 其本身的以及是否 SPN 為使用 \ [KDC 因為簡單複寫延遲或︰ 複寫失敗 GC 遺失。</para>
        </listItem>
        <listItem>
          <para>檢查安全通道健康狀態，並信任狀態。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>疑難排解 Active Directory 操作失敗，錯誤 1396 年：登入失敗：目標帳號不正確。</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


