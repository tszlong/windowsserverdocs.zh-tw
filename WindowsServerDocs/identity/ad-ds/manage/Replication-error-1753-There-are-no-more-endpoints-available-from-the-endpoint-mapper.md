---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: "複製錯誤 1753 年有的端點對應程式提供更多端點"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e7412f5edc6c206888551fdc250883b5c0ced3e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>複製錯誤 1753 年有的端點對應程式提供更多端點

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>本主題解釋，包括症狀、原因，以及如何解析 Active Directory 複寫錯誤 8524 DSA 操作程式無法繼續因為 DNS 搜尋。</para>
    <list class="bullet">
      <listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">症狀</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">原因</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">解析度</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">更多資訊</link></para></listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症狀</title>
    <content>
      <para>此文章將描述包括症狀、原因和解析度步驟的 Active Directory 操作失敗 Win32 錯誤 1753 年:「有可用的更多端點端點對應程式。」</para>
      <list class="ordered">
        <listItem>
          <para>DCDIAG 報告可連接測試、Active Directory 複寫測試或 KnowsOfRoleHolders 測試失敗，錯誤 1753 年:「有可用的更多端點端點對應程式。」</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN。EXE 報告該︰ 複寫失敗 1753 年狀態。</para><para>REPADMIN 命令通常引用 1753 年狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN 進行</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>範例輸出從「REPADMIN 進行」描繪輸入的複寫 CONTOSO-DC2 從「複寫無此許可權」錯誤的 CONTOSO lax-dc1 失敗如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para><ui>個</ui>中 Active Directory 網站和服務的命令傳回」有可用的更多端點端點對應程式從。」</para><para>連接物件來源俠上按一下滑鼠右鍵，然後選擇<ui>個</ui>會失敗，且「有可用的更多端點端點對應程式。」下方螢幕上顯示錯誤訊息：</para><para>對話方塊的標題文字：個</para><para>對話方塊的訊息文字：</para><para>下列時發生嘗試連絡網域控制站：有更多端點端點對應程式可用。</para></listItem><listItem><para><ui>複製現在</ui>中 Active Directory 網站和服務的命令傳回」有可用的更多端點端點對應程式從。」</para><para>連接物件來源俠上按一下滑鼠右鍵，然後選擇<ui>複製現在</ui>會失敗，且「有可用的更多端點端點對應程式。」螢幕上的錯誤訊息如下所示：</para><para>對話方塊的標題文字：立即複寫</para><para>對話方塊的訊息文字：下列時發生嘗試同步命名操作&lt;directory 磁碟分割名稱 %&gt;網域控制站的&lt;來源俠&gt;網域控制站&lt;目的地俠&gt;:</para><para>

有更多端點端點對應程式可用。</para><para>將不會繼續操作</para></listItem><listItem><para>NTDS KCC、NTDS 一般或 Microsoft-Windows-ActiveDirectory_DomainService 事件-2146893022 狀態的登入 Directory 服務登入事件檢視器中。</para><para>Active Directory 事件通常引用-2146893022 狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件編號</para></TD><TD><para>事件來源</para></TD><TD><para>事件字串</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS 一般</para></TD><TD><para>Active Directory 嘗試使用下列的通用通訊，嘗試已失敗。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>建立下列寫入 directory 磁碟分割的連結︰ 複寫失敗。</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試知識一致性檢查程式 (KCC) 新增下列 directory 磁碟分割和來源網域控制站複寫合約失敗。</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>原因</title>
    <content>
      <para>下圖顯示 client 中的應用程式執行「步驟 7 RPC 的開始登記 server 應用程式與 RPC Endpoint 對應 (EPM) 步驟 1 中的資料傳送 RPC 工作流程。</para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>透過下列操作 7 對應步驟 1:</para>
      <list class="ordered">
        <listItem>
          <para>伺服器應用程式暫存的端點器 RPC Endpoint 對應 (EPM) 的</para>
        </listItem>
        <listItem>
          <para>Client 呼叫 RPC（代表使用者、作業系統或應用程式車載機起始作業）</para>
        </listItem>
        <listItem>
          <para>Client 側邊 RPC 連絡人的目標電腦 EPM，才能完成 client 通話端點要求</para>
        </listItem>
        <listItem>
          <para>伺服器電腦 EPM 回應端點</para>
        </listItem>
        <listItem>
          <para>Client 側邊 RPC 連絡人伺服器應用程式</para>
        </listItem>
        <listItem>
          <para>伺服器應用程式執行通話將結果傳回 client RPC </para>
        </listItem>
        <listItem>
          <para>Client 側邊 RPC 傳遞結果回 client 應用程式</para>
        </listItem>
      </list>
      <para>失敗 1753 年由兩個步驟 #3 和 #4 失敗。具體而言，錯誤 1753 年表示 RPC client（目的地俠）就能透過連接埠 135 連絡 RPC 伺服器（來源俠），但 EPM RPC 伺服器（來源俠）上的找不到感興趣的領域 RPC 應用程式，並傳回伺服器端錯誤 1753 年。卡 1753 年錯誤的指示，RPC client（目的地俠）收到伺服器端錯誤回應 RPC 伺服器（廣告複寫來源俠）在網路上。</para>
      <para>特定根本原因 1753 年錯誤，包括：</para>
      <list class="ordered">
        <listItem>
          <para>永遠不會開始伺服器應用程式（亦即永遠不會嘗試上方」的詳細資訊」圖表中的步驟 #1）。</para>
        </listItem>
        <listItem>
          <para>開始伺服器應用程式，但期間，所以它無法登記 RPC Endpoint 對應程式（亦即步驟 #1「詳細資訊」上圖中已嘗試執行，但無法）的初始設定是一些失敗。</para>
        </listItem>
        <listItem>
          <para>但後續死伺服器應用程式。（也就是步驟 #1「詳細資訊」上圖中已成功完成，但已復原稍後因為伺服器死）。</para>
        </listItem>
        <listItem>
          <para>[伺服器] app 以手動方式註冊其結束（但刻意 3 類似。但不是可能包含的完整性。）</para>
        </listItem>
        <listItem>
          <para>RPC client（目的地俠）連絡不同 RPC 伺服器比預期因為 IP 對應錯誤 DNS、WINS 或主機日 Lmhosts 檔案名稱。</para>
        </listItem>
      </list>
      <para>並不是藉由造成錯誤 1753 年: </para>
      <list class="bullet">
        <listItem>
          <para>RPC client（目的地俠）和 RPC 伺服器（來源俠）之間的網路中斷連接埠 135 缺少</para>
        </listItem>
        <listItem>
          <para>缺乏之間 RPC 伺服器（來源俠）網路連接到暫時的連接埠使用連接埠 135 和 RPC client（目的地俠）。</para>
        </listItem>
        <listItem>
          <para>密碼不符或是來源俠解密 Kerberos 加密封包</para>
        </listItem>
      </list>
      <para></para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解析度</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>開始進行登記其服務端點對應程式與服務的驗證</embeddedLabel>
          </para>
          <para>Windows 2000 的和 Windows Server 2003 Dc：確保的來源 DC 開機進入標準模式。</para>
          <para> Windows Server 2008 或 Windows Server 2008 R2：來源 DC 主機，從開始服務管理員 (services.msc) 並確認<embeddedLabel>Active Directory Domain Services</embeddedLabel>服務正在執行。</para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>驗證連接至目標 RPC 伺服器（來源俠）該 RPC client（目的地俠）</embeddedLabel>
          </para>
          <para>所有網域控制站常見的 Active Directory 森林登記網域控制站 CNAME 錄製 _msdcs 中。&lt;樹系根網域&gt;無論何種網域樹系的位於 DNS 區域。俠 CNAME 記錄從<embeddedLabel>objectguid 資訊</embeddedLabel>針對每個網域控制站 NTDS 設定物件的屬性。</para>
          <para>執行複寫為基礎的作業，目的地俠查詢 DNS 網域控制站 CNAME 記錄來源。CNAME 記錄包含用來衍生來源 Dc IP 位址來源 DC 完整的電腦名稱 DNS client 快取搜尋，透過裝載 / LMHost 檔案查詢主機 A / AAAA 錄製 DNS、WINS 中。</para>
          <para>過時 NTDS 設定物件和不良名稱-TO-IP 對應 DNS、WINS、主機和 LMHOST 檔案可能會造成連接到錯誤 RPC 伺服器 (來源 DC) RPC client（目的地俠）。此外，不良名稱 TO-IP 對應可能會感興趣的領域（在本案例中 Active Directory 角色）安裝會造成連接到電腦不能有 RPC 伺服器應用程式 RPC client（目的地俠）。(範例：DC2 的過時主機記錄包含 DC3 或成員電腦的 IP 位址)。</para>
          <para>確認符合 objectguid 來源 DC 在於目的地 Dc 複本 Active Directory 中的資訊儲存在 Active directory Dc 複製來源來源俠 objectguid 資訊。如果不一致，使用 repadmin /showobjmeta ntds 看到哪一個對應至最後一個促銷俠來源的設定物件 (提示：比較日期戳記 NTDS 設定物件建立的日期 /showobjmeta 針對來源 Dc dcpromo.log 檔案中的最後一個促銷日期。您可能必須使用上次修改 / 建立 DCPROMO.LOG 檔案本身。）物件 Guid 並不相同，如果目的 DC 可能會有一個過時的 NTDS 設定物件來源俠其 CNAME 記錄是指主機記錄不正確對應 IP 名稱。</para>
          <para>俠目的地執行 IPCONFIG /ALL 判斷的 DNS 伺服器目的 DC 所使用的名稱解析：</para>
          <code>c:&gt;ipconfig /all</code>
          <para>俠目的地執行 NSLOOKUP 針對網域控制站完整俠 CNAME 記錄來源：</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>驗證您的 IP 位址所 NSLOOKUP 傳回」擁有」主機名稱日的來源 DC 安全性身分：</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>或</para>
          <para>登入的來源 DC 主機、從命令提示字元中執行「IPCONFIG」，並確認來源 DC 擁有傳回上述 NSLOOKUP 命令的 IP 位址</para>
          <para>檢查過時 / 重複 dns IP 對應至主機</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>主機記錄存在於無效的 IP 位址，調查是否 DNS 清除支援並設定正確。</para><para>如果上述的測試或網路追蹤不會顯示名稱查詢退貨不正確的 IP 位址，請考慮將過時 LMHOSTS 檔案和 WINS 伺服器、主機檔案中的項目。請注意，也可以執行 WINS 回溯名稱解析設定 DNS 伺服器。</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>(Active Directory 等 al) 伺服器應用程式已經登記完畢端點對應程式 RPC（來源俠）伺服器上的確認</embeddedLabel>
          </para>
          <para>Active Directory 會使用多種已知和動態且已連接埠。下表列出熟知連接埠和使用 Active Directory 網域控制站的通訊協定。</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>RPC 伺服器應用程式</para>
                </TD>
                <TD>
                  <para>連接埠</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>DNS 伺服器</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP 伺服器</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP SSL</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>通用伺服器</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>通用伺服器</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>已知的連接埠不是使用端點對應程式登記完畢。</para>
          <para>Active Directory 和其他應用程式也登記收到動態指派連接埠 RPC 暫時連接埠有時候您附近的服務。這類 RPC 伺服器應用程式動態指派 1024 年和 Windows 2000 和 Windows Server 2003 電腦上的 5000 之間的 TCP 連接埠和 Windows Server 2008 和 Windows Server 2008 R2 上的 49152 與 65535 範圍之間的連接埠。使用複製的 RPC 連接埠可以固定的文件中的步驟<externalLink><linkText>知識庫文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>。Active Directory 持續登記與 EPM 設定時，使用硬連接埠。</para>
          <para>確認 RPC 伺服器感興趣的應用程式已經登記本身 RPC 端點對應 RPC 伺服器（來源俠在 AD 複寫）上使用。</para>
          <para>有許多方式完成這項工作，但其中一個是安裝及使用語法俠來源的主機上系統管理員權限命令提示字元執行 PORTQRY: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>在 portqry 輸出中，請注意，「MS NT Directory DRS 介面「動態登記連接埠號碼 (UUID = 351...) 的<embeddedLabel>ncacn_ip_tcp 通訊協定</embeddedLabel>。下方的程式碼片段與 Windows Server 2008 R2 俠 UUID 顯示範例 portquery 輸出 / 通訊協定組，專門用 Active Directory 中反白顯示<embeddedLabel>設定為粗體</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>其他解析這個錯誤可能方式：</para>
          <list class="ordered">
            <listItem>
              <para>確認標準模式中的開機來源 DC 來源 DC 的 OS 和俠角色完全已經開始進行。</para>
            </listItem>
            <listItem>
              <para>執行 Active Directory Domain 服務的驗證。如果目前已停止或未設定開機的預設值的服務，預設開機值重設、重新開機修改的俠然後再試一。</para>
            </listItem>
            <listItem>
              <para>確認開機值和服務狀態 RPC 服務和 RPC 定位是正確的 OS 版本的 RPC Client（目的地俠）和 RPC 伺服器（來源俠）。如果目前已停止或未設定開機的預設值的服務，預設開機值重設、重新開機修改的俠然後再試一。</para>
              <para>，確認服務操作符合預設設定，如下表所示。</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>服務</para>
                    </TD>
                    <TD>
                      <para>預設狀態（開機類型）在 Windows Server 2003 及更新版本 </para>
                    </TD>
                    <TD>
                      <para>在 Windows Server 2000 預設狀態（開機鍵入）</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>遠端程序呼叫</para>
                    </TD>
                    <TD>
                      <para>開始（自動）</para>
                    </TD>
                    <TD>
                      <para>開始（自動）</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>遠端程序呼叫定位器</para>
                    </TD>
                    <TD>
                      <para>空值或停止（手動）</para>
                    </TD>
                    <TD>
                      <para>開始（自動）</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>請確認連接埠動態範圍大小不受限制。Windows Server 2008 和 Windows Server 2008 R2 NETSH 語法列舉 RPC 連接埠範圍如下所示：</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>驗證，以 kb 為單位 224196 定義硬連接埠定義落連接埠動態範圍來源網域控制站 OS 版本。</para>
              <para>檢視<externalLink><linkText>知識庫文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>，並確保硬連接埠瀑布暫時連接埠範圍來源 DC 的作業系統版本。</para>
            </listItem>
            <listItem>
              <para>確認儲存的.reg 金鑰存在於 HKLM\Software\Microsoft\Rpc 在包含下列 5 預設值︰</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>更多的資訊</title>
    <content>
      <para>
        <embeddedLabel>IP 對應造成 RPC 錯誤 1753 年與-2146893022 名稱錯誤範例：不正確的目標主體名稱</embeddedLabel>
      </para>
      <para>contoso.com 網域組成 DC1 及 DC2 ip 位址 x.x.1.1 和 x.x.1.2。主機"A"/」AAAA「記錄 DC2 正確登記上所有的設定 DC1 DNS 伺服器。此外，主機上的檔案 DC1 包含對應 IP 位址 x.x.1.2 主機 DC2s 完整名稱的項目。之後，DC2 的 IP 位址變更 X.X.1.2 X.X.1.3 和新的成員電腦已經加入網域的 IP 位址 x.x.1.2。廣告複寫觸發嘗試<ui>現在複製</ui>命令 Active Directory 網站和服務] 嵌入式管理單元失敗的錯誤 1753 年下列追蹤中所示：</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>在畫面<embeddedLabel>10</embeddedLabel>，目的 DC 查詢來源網域控制站終點對應的 Active Directory 複寫服務課程 UUID E351 135 連接到...</para>
      <para>畫面中<embeddedLabel>11</embeddedLabel>，來源 DC，在這個案例不尚未主控俠的角色，因此不已經登記 E351 成員電腦...使用它本機 EPM 複寫服務 UUID 回應符號錯誤 EP_S_NOT_REGISTERED 地圖小數點錯誤 1753 年、十六進位錯誤 0x6d9 和易懂錯誤的「有可用的更多端點端點對應程式」。</para>
      <para>之後，成員電腦的 IP 位址 x.x.1.2 取得升級為「MayberryDC「contoso.com 網域中的複本。再試一次，<ui>現在複製</ui>命令可用來觸發複寫，但在此階段失敗，並螢幕錯誤」目標主體名稱不正確。」電腦的網路介面卡指定 IP 位址 x.x.1.2<placeholder>是</placeholder>網域控制站目前開機進入標準模式和已經登記完畢 E351...複寫服務 UUID 其本機 EPM，但它不擁有名稱或安全的身分 DC2 與無法解密 DC1 Kerberos 要求，因此要求現在失敗，錯誤」不正確的目標主體名稱]。錯誤地圖小數點錯誤-2146893022 / 十六進位 0x80090322 錯誤。</para>
      <para>這類無效主機-TO-IP 對應可能會造成過時在主機中的項目 lmhost 檔案裝載 A / AAAA 登錄 DNS、WINS 中的。</para>
      <para>摘要：此範例中，無法無效主機 TO-IP 對應（在本案例中的主機檔案）會造成目的地俠解析「來源「不含 Active Directory Domain Services 俠服務執行（或甚至該項目的安裝），不尚未登記 SPN 複寫和 DC 傳回錯誤 1753 年來源。在第二個案例中，不正確主機 TO-IP 對應（再試一次主機檔案）中的造成目的 DC 連接到...必須登記 E351 俠複寫 SPN，但該來源不同的主機名稱和安全性身分比預期的來源俠，嘗試失敗，錯誤-2146893022：目標主體名稱不正確。</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>疑難排解 Active Directory 操作失敗的錯誤 1753 年：有更多端點端點對應程式可用。</linkText>
      <linkUri>https://support.microsoft.com/kb/2089874</linkUri>
    </externalLink>
<externalLink><linkText>KB 文章 839880 疑難排解 RPC Endpoint 對應錯誤使用或許從 Windows Server 2003 的支援工具</linkText><linkUri>https://support.microsoft.com/kb/839880</linkUri></externalLink>
<externalLink><linkText>KB 文章 832017 服務概觀和網路上的 Windows Server 系統需求連接埠</linkText><linkUri>https://support.microsoft.com/kb/832017/</linkUri></externalLink>
<externalLink><linkText>KB 文章 224196 限制 Active Directory 複寫流量和 client RPC 傳輸到特定的連接埠</linkText><linkUri>https://support.microsoft.com/kb/224196/</linkUri></externalLink>
<externalLink><linkText>KB 文件 154596 如何設定以使用防火牆 RPC 動態連接埠配置</linkText><linkUri>https://support.microsoft.com/kb/154596</linkUri></externalLink><externalLink><linkText>RPC 的運作方式</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>如何伺服器準備連接</linkText><linkUri>https://msdn.microsoft.com/library/aa373938(VS.85).aspx</linkUri></externalLink>
<externalLink><linkText>Client 如何建立連接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>登記介面</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>推出伺服器網路上</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>登記端點</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx</linkUri></externalLink><externalLink><linkText>接聽電話 Client</linkText><linkUri>https://msdn.microsoft.com/library/aa373966(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Client 如何建立連接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>限制 Active Directory 複寫流量和 client RPC 特定的連接埠的流量</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>中 AD DS 目標 dc SPN</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


