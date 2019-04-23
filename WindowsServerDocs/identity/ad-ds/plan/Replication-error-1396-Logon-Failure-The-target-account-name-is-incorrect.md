---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: '複寫錯誤 1396：登入失敗: 目標帳戶名稱不正確'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ef3da06dd348b804f538d37cafbfabdd8bf45beb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844499"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>複寫錯誤 1396：登入失敗: 目標帳戶名稱不正確

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>這篇文章說明徵狀、 原因和解決 Active Directory 複寫失敗，發生 Win32 錯誤 1396年:「 登入失敗：目標帳戶名稱不正確。 」 </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">徵狀</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">原因</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">解決方式</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>徵狀</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Active Directory 複寫測試失敗，錯誤 1396 DCDIAG 報表：登入失敗：目標帳戶名稱不正確。 」</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN。前次複寫嘗試失敗，狀態為 1396年的 EXE 報表。</para><para>REPADMIN 命令，通常會指出 1396年狀態包括但不限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>範例輸出從 「 REPADMIN /SHOWREPS 」 描述從 CONTOSO-DC2 進行輸入的複寫到 CONTOSO-DC1 失敗，發生 「 登入失敗：目標帳戶名稱不正確。 」 如下所示的錯誤::</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para><ui>立即複寫</ui>Active Directory 站台和服務中的命令會傳回 「 登入失敗：目標帳戶名稱不正確。 」</para><para>來源 DC 的連線物件上按一下滑鼠右鍵，然後選擇<ui>立即複寫</ui>因 「 登入失敗：目標帳戶名稱不正確。 」 螢幕上會顯示錯誤訊息如下：</para><para>對話方塊標題的文字：</para><para>立即複寫</para><para>對話方塊訊息文字： </para><para>在嘗試同步處理命名內容期間發生下列錯誤&lt;磁碟分割 DNS 路徑&gt;從網域控制站&lt;來源 DC&gt;網域控制站&lt;目的地 DC&gt;:登入失敗：目標帳戶名稱不正確。 這項作業將不會繼續。 </para></listItem><listItem><para>NTDS KCC、 NTDS 一般或 Microsoft-Windows-ActiveDirectory_DomainService 1396 狀態的事件會記錄在事件檢視器中的 目錄服務記錄檔中。</para><para>Active Directory 事件通常會指出 1396年狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件識別碼</para></TD><TD><para>事件來源</para></TD><TD><para>事件字串</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 網域服務安裝精靈 (Dcpromo) 無法與下列網域控制站建立連線。</para></TD></tr><tr><TD><para>1645</para><para>此事件會列出三個部分的 SPN。</para></TD><TD><para>NTDS 複寫</para></TD><TD><para>Active Directory 並未對另一個網域控制站執行已驗證的遠端程序呼叫 (RPC)，因為目的地網域控制站所需的服務主要名稱 (SPN) 並未登錄在解析 SPN 的金鑰發行中心 (KDC) 網域控制站上。</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory 網域服務嘗試與下列通用類別目錄通訊，並嘗試不成功。</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>知識一致性檢查程式位於本機的唯讀目錄服務複寫連線，並嘗試更新下列目錄服務執行個體上的遠端。 作業失敗。 它將會重試。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試建立下列的可寫入的目錄磁碟分割的複寫連結失敗。</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試與失敗的下列參數建立的唯讀目錄磁碟分割的複寫連結。</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> 伺服器無法在 DNS 中登錄它的名稱。</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO 會失敗並出現在螢幕上的錯誤</para><para>對話方塊標題的文字：</para><para>Active Directory 安裝失敗</para><para>對話方塊訊息文字：</para><para>作業失敗，因為：目錄服務無法建立伺服器物件的 CN = NTDS Settings，CN = ServerBeingPromoted，CN = Servers，CN = Site，CN = Sites，CN = Configuration，DC = contoso，DC = com 伺服器 ReplicationSourceDC.contoso.com 上。 </para><para>請確定提供的網路認證有足夠的存取權新增複本。 </para><para>
「 登入失敗：目標帳戶名稱不正確。 "</para><para>在此情況下，會記錄事件識別碼 1645年、 1168 和 1125年，正在升級的伺服器上。</para></listItem><listItem><para>對應磁碟機，使用<embeddedLabel>net 使用</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>在此情況下，伺服器可以也在系統事件記錄檔中記錄事件識別碼 333 並使用大量的虛擬記憶體的應用程式，例如 SQL Server。</para></listItem><listItem><para>DC 時間不正確。</para></listItem><listItem><para>KDC 將不會啟動在 RODC 上的 krbtgt 帳戶在還原後的 rodc，已刪除。 例如，還原之後，錯誤 1396年隨即出現。 </para><para>
在 RODC 上，會記錄事件識別碼為 1645年。 </para><para>
Dcdiag 也會報告錯誤，無法更新 RODC krbtgt 帳戶。 </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>原因</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>代表用戶端嘗試驗證使用 Kerberos kdc 搜尋通用類別目錄上沒有 SPN。</para>
          <para>在 Active Directory 複寫的內容中，Kerberos 用戶端是目的地 DC，執行 SPN 查閱 KDC 可能是目的地本身的 DC，但可能是遠端的 DC。</para>
        </listItem>
        <listItem>
          <para>代表目的地 DC 嘗試複寫 kdc 搜尋通用類別目錄上不存在的使用者或應該包含要進行查閱的服務主體名稱的服務帳戶。</para>
          <para>在 Active Directory 複寫的內容中，來源 DC 的電腦帳戶不存在通用類別目錄代表目的地 DC 執行輸入複寫搜尋由網域控制站上。</para>
        </listItem>
        <listItem>
          <para>目的地 DC 缺乏 LSA 密碼，來源網域控制站的網域。</para>
        </listItem>
        <listItem>
          <para>要進行查閱的 SPN 會存在於不同的電腦帳戶與來源 DC。</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解決方式</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>檢查 NTDS 複寫事件 1645年目的地 DC 的目錄服務事件記錄檔，並注意下列：</para>
          <para>目的地 DC 的名稱</para>
          <para>SPN 查閱 (e3514235-4b06-11d1-ab04-00c04fc2dcd2 /&lt;物件的來源網域控制站的 NTDS 設定物件的 guid&gt;/&lt;目標網域&gt;。&lt;tld&gt;@&lt;目標網域&gt;。&lt;tld&gt;</para>
          <para>正由目的地 DC 的 KDC</para>
        </listItem>
        <listItem>
          <para>從步驟 1 中識別的 KDC 主控台中，輸入： </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>執行 NLTEST 定位器測試緊接在目標 DC 上 1396年錯誤而失敗的複寫嘗試。 </para>
          <para>這應該識別 KDC 執行 SPN 查閱，針對該 GC。 </para>
          <para>GC 搜尋 kdc 可能也會擷取 Microsoft-Windows-ActiveDirectory_DomainService 事件 1655年中。</para>
        </listItem>
        <listItem>
          <para>搜尋在步驟 2 中找到通用類別目錄上的步驟 1 中探索到的 SPN。</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>或</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>請確認 SPN 的主機物件存在。</para>
          <para>請確認主機物件，包括是否物件是 CNF / 衝突受損或位於失物招領容器的 DN 路徑。</para>
          <para>確認來源網域控制站 Active Directory 複寫 SPN 註冊只能在來源網域控制站電腦帳戶上。</para>
          <para>如果複寫 SPN，判斷是否來源 DC 已向其 SPN 本身，以及是否在 GC 因為簡單的複寫延遲或複寫失敗 KDC 所使用的 SPN 遺漏。</para>
        </listItem>
        <listItem>
          <para>檢查健全狀況，安全通道，而信任健全狀況。</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>疑難排解 Active Directory 作業失敗，錯誤 1396年:登入失敗：目標帳戶名稱不正確。</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


