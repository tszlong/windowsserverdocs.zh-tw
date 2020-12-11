---
description: 深入瞭解：複寫錯誤1396登入失敗。目標帳戶名稱不正確
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: '複寫錯誤 1396：登入失敗: 目標帳戶名稱不正確'
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a21d4f2a5e9772393d3e84a2adb497406c693f63
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042586"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>複寫錯誤 1396：登入失敗: 目標帳戶名稱不正確

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>本文描述的徵兆、原因和解決方法 Active Directory 複寫失敗，Win32 錯誤1396：登入 &quot; 失敗：目標帳戶名稱不正確。&quot; </para>
    <list class="bullet"> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">症狀</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">原因</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">決議</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>徵兆</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG 報告 Active Directory 複製測試失敗，錯誤1396：登入失敗：目標帳戶名稱不正確。&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN.EXE 報告上次複寫嘗試失敗，狀態為1396。</para><para>通常會引用1396狀態的 REPADMIN 命令包含但不限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/ADD</para></listItem><listItem><para>REPADMIN/REPLSUM</para></listItem><listItem><para>REPADMIN/REHOST</para></listItem><listItem><para>REPADMIN/SHOWVECTOR/LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN/SHOWREPS</para></listItem><listItem><para>REPADMIN/SHOWREPL</para></listItem><listItem><para>REPADMIN/SYNCALL</para></listItem></list></TD></tr></tbody></table><para>從 &quot; &quot; CONTOSO-DC2 到 CONTOSO-DC1 的輸入複寫範例輸出的範例輸出/SHOWREPS 登入 &quot; 失敗：目標帳戶名稱不正確。 &quot; 錯誤如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>Active Directory 網站和服務中的 [ <ui>立即</ui> 複寫] 命令傳回 &quot; 登入失敗：目標帳戶名稱不正確。&quot;</para><para>以滑鼠右鍵按一下來源 DC 中的連線物件，然後選擇 [ <ui>立即</ui> 複寫] 失敗並出現 &quot; 登入失敗：目標帳戶名稱不正確。 &quot; 畫面上的錯誤訊息如下所示：</para><para>對話方塊標題文字：</para><para>立即複寫</para><para>對話方塊郵件內文： </para><para>嘗試將命名內容 &lt; 磁碟分割 DNS 路徑 &gt; 從網域控制站 &lt; 來源 dc 同步 &gt; 至網域控制站目的地 dc 時發生下列錯誤： &lt; 登入 &gt; 失敗：目標帳戶名稱不正確。 這種作業將不會繼續。 </para></listItem><listItem><para>[NTDS KCC]、[NTDS 一般] 或 [Microsoft-Windows-ActiveDirectory_DomainService 事件（具有1396狀態）都會記錄在 [目錄服務] 記錄檔中事件檢視器。</para><para>通常提及1396狀態的 Active Directory 事件，包括但不限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件識別碼</para></TD><TD><para>事件來源</para></TD><TD><para>事件字串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services 安裝精靈 (Dcpromo) 無法與下列網域控制站建立連線。</para></TD></tr><tr><TD><para>1645</para><para>此事件會列出三部分的 SPN。</para></TD><TD><para>NTDS 複寫</para></TD><TD><para>Active Directory 並未對另一個網域控制站執行已驗證的遠端程序呼叫 (RPC)，因為目的地網域控制站所需的服務主要名稱 (SPN) 並未登錄在解析 SPN 的金鑰發行中心 (KDC) 網域控制站上。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services 嘗試與下列通用類別目錄通訊，但嘗試失敗。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>知識一致性檢查程式找到本機唯讀目錄服務的複寫連線，並嘗試從遠端在下列目錄服務實例上進行更新。 作業失敗。 將會重試。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試建立下列可寫入目錄分割的複寫連結失敗。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試以下列參數建立唯讀目錄分割的複寫連結失敗。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> 伺服器無法在 DNS 中註冊其名稱。</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO 失敗，並出現螢幕錯誤</para><para>對話方塊標題文字：</para><para>Active Directory 安裝失敗</para><para>對話方塊郵件內文：</para><para>作業失敗，因為：目錄服務無法在伺服器 ReplicationSourceDC.contoso.com 上建立 CN = NTDS Settings，CN = ServerBeingPromoted，CN = Servers，CN = Site，cn = Sites，CN = Configuration，DC = contoso，DC = com 的伺服器物件。 </para><para>請確定提供的網路認證具有足夠的存取權，才能新增複本。 </para><para>
&quot;登入失敗：目標帳戶名稱不正確。 &quot;</para><para>在此情況下，系統會將事件識別碼1645、1168和1125記錄在正在升級的伺服器上。</para></listItem><listItem><para>使用 <embeddedLabel>net use 來</embeddedLabel>對應磁片磁碟機：</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在此情況下，伺服器也可以將事件識別碼333記錄在系統事件記錄檔中，並針對應用程式（例如 SQL Server）使用大量的虛擬記憶體。</para></listItem><listItem><para>DC 時間不正確。</para></listItem><listItem><para>當 RODC 的 krbtgt 帳戶還原已被刪除後，KDC 將不會在 RODC 上啟動。 例如，在還原之後，會出現錯誤1396。 </para><para>
事件識別碼1645會記錄在 RODC 上。 </para><para>
Dcdiag 也會報告無法更新 RODC krbtgt 帳戶的錯誤。 </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>原因</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>當用戶端嘗試使用 Kerberos 進行驗證時，不會在 KDC 所搜尋的通用類別目錄上存在 SPN。</para>
          <para>在 Active Directory 複寫的內容中，Kerberos 用戶端是目的地 DC，執行 SPN 查閱的 KDC 可能是目的地 DC 本身，但可能是遠端 DC。</para>
        </listItem>
        <listItem>
          <para>用戶端或服務帳戶若應包含所查閱的服務主體名稱，則代表嘗試複寫的目的地 DC 所搜尋的通用類別目錄不存在。</para>
          <para>在 Active Directory 複寫的內容中，來源 DC 電腦帳戶不存在於 DC 代表執行輸入複寫的目的地 DC 所搜尋的通用類別目錄上。</para>
        </listItem>
        <listItem>
          <para>目的地 DC 缺少來源 Dc 網域的 LSA 秘密。</para>
        </listItem>
        <listItem>
          <para>要查閱的 SPN 存在於與來源 DC 不同的電腦帳戶上。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解決方式</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>檢查目的地 DC 上的目錄服務事件記錄檔中是否有 NTDS 複寫事件1645，並注意下列事項：</para>
          <para>目的地 DC 的名稱</para>
          <para>要查閱的 SPN (&lt; 來源 dc NTDS 設定物件目標網域的 E3514235-4B06-11D1-AB04-00C04FC2DCD2/物件 guid （ &gt; / &lt; 目標網域 &amp; amp; gt;）。 &amp;amp; lt; tld &amp; amp; gt; @ &lt; 目標網域 &gt; 。 &lt;Tld&gt;</para>
          <para>目的地 DC 所使用的 KDC</para>
        </listItem>
        <listItem>
          <para>從步驟1中識別的 KDC 主控台，輸入： </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>緊接在目的地 DC 上發生1396錯誤的複寫嘗試之後，立即執行 NLTEST 定位器測試。 </para>
          <para>這應該會找出 KDC 執行 SPN 查閱的 GC。 </para>
          <para>KDC 所搜尋的 GC 也可以在 Microsoft-Windows-ActiveDirectory_DomainService 事件1655中捕捉。</para>
        </listItem>
        <listItem>
          <para>在步驟2中找到的通用類別目錄上搜尋步驟1中找到的 SPN。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或者</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>確認 SPN 的主機物件存在。</para>
          <para>驗證主機物件的 DN 路徑，包括物件是否 MY.CNF/衝突，或位於遺失和找到的容器中。</para>
          <para>確認 Active Directory 複寫 SPN 的來源 Dc 只在來源 Dc 電腦帳戶上註冊。</para>
          <para>如果複寫 SPN 遺失，請判斷來源 DC 是否已向本身登錄其 SPN，以及因為有簡單的複寫延遲或複寫失敗，而導致 KDC 使用的 GC 上缺少 SPN。</para>
        </listItem>
        <listItem>
          <para>檢查安全通道健康情況和信任健全狀況。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>疑難排解 Active Directory 作業失敗，錯誤1396：登入失敗：目標帳戶名稱不正確。</linkText>
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


