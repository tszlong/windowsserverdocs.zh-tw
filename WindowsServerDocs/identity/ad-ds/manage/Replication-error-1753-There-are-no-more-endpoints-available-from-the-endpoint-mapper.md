---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 複寫錯誤 1753：端點對應表中無更多可用的端點
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e429c87a2194ecfaf02c3d6c579eda75293250d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827509"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>複寫錯誤 1753：端點對應表中無更多可用的端點

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>本主題說明徵狀、 原因和解決 Active Directory 複寫錯誤 8524 DSA 操作是無法繼續，因為 DNS 查閱失敗。</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">徵狀</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">原因</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">解決方式</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">詳細資訊</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>徵狀</title>
    <content>
      <para>這篇文章說明徵狀、 原因和解決方式步驟失敗，發生 Win32 錯誤 1753年的 Active Directory 作業："There are 沒有可用的終點。"</para>
      <list class="ordered">
        <listItem>
          <para>DCDIAG 報告連線測試、 Active Directory 複寫測試或 KnowsOfRoleHolders 測試失敗，錯誤 1753年:"There are 沒有可用的終點。"</para>
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
<listItem><para>REPADMIN。EXE 會報告該複寫嘗試失敗，狀態為 1753年。</para><para>REPADMIN 命令，通常會指出 1753年狀態包括但不限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>從 「 REPADMIN /SHOWREPS 」 描述從 CONTOSO-DC2 進行輸入的複寫至 CONTOSO-DC1"複寫存取被拒 」 錯誤而失敗的範例輸出如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
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

</code></listItem><listItem><para><ui>檢查複寫拓樸</ui>Active Directory 站台和服務中的命令會傳回"There are 沒有可用的終點。"</para><para>來源 DC 的連線物件上按一下滑鼠右鍵，然後選擇<ui>檢查複寫拓樸</ui>因"There are 沒有可用的終點。" 螢幕上會顯示錯誤訊息如下：</para><para>對話方塊標題的文字：檢查複寫拓樸</para><para>對話方塊訊息文字： </para><para>在嘗試連絡網域控制站期間，發生下列錯誤：沒有可用的終點的多個端點。</para></listItem><listItem><para><ui>立即複寫</ui>Active Directory 站台和服務中的命令會傳回"there are 沒有可用的終點。"</para><para>來源 DC 的連線物件上按一下滑鼠右鍵，然後選擇<ui>立即複寫</ui>因"There are 沒有可用的終點。" 螢幕上會顯示錯誤訊息如下：</para><para>對話方塊標題的文字：立即複寫</para><para>對話方塊訊息文字：在嘗試同步處理命名內容期間發生下列錯誤&lt;%目錄磁碟分割名稱 %&gt;從網域控制站&lt;來源 DC&gt;網域控制站&lt;目的地 DC&gt;:</para><para>

沒有可用的終點的多個端點。</para><para>將不會繼續作業</para></listItem><listItem><para>NTDS KCC、 NTDS 一般或 Microsoft-Windows-ActiveDirectory_DomainService-2146893022： 狀態的事件會記錄在事件檢視器中的 目錄服務記錄檔中。</para><para>Active Directory 事件通常會指出-2146893022： 狀態，包括但不是限於：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件識別碼</para></TD><TD><para>事件來源</para></TD><TD><para>事件字串</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS 一般</para></TD><TD><para>Active Directory 嘗試與下列通用類別目錄通訊，並嘗試不成功。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>嘗試建立下列的可寫入的目錄磁碟分割的複寫連結失敗。</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>知識一致性檢查程式 (KCC) 新增下列目錄磁碟分割及來源網域控制站複寫合約失敗。</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>原因</title>
    <content>
      <para>下圖顯示從 RPC 用戶端在步驟 7 中的用戶端應用程式開始進行註冊的伺服器應用程式與 RPC 端點對應程式 」 (EPM) 在步驟 1 中的資料傳送 RPC 工作流程。 </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>步驟 1 到 7 的對應到下列作業：</para>
      <list class="ordered">
        <listItem>
          <para>伺服器應用程式註冊其端點與 RPC 端點對應程式 (EPM) </para>
        </listItem>
        <listItem>
          <para>用戶端進行 RPC 呼叫 （代表使用者、 作業系統或應用程式啟動作業） </para>
        </listItem>
        <listItem>
          <para>用戶端 RPC 連絡目標電腦 EPM 和要求要完成在用戶端呼叫的端點 </para>
        </listItem>
        <listItem>
          <para>與端點的伺服器機器 EPM 回應 </para>
        </listItem>
        <listItem>
          <para>用戶端 RPC 連線的 server 應用程式 </para>
        </listItem>
        <listItem>
          <para>伺服器應用程式執行呼叫時，將結果傳回至用戶端 RPC </para>
        </listItem>
        <listItem>
          <para>用戶端 RPC 將結果傳遞給用戶端應用程式</para>
        </listItem>
      </list>
      <para>在步驟 #3 和 4 之間的失敗會產生失敗 1753年。 具體來說，錯誤 1753年表示 RPC 用戶端 （目的地 DC） 是能夠連線透過連接埠 135 的 RPC 伺服器 （來源 DC），但 EPM RPC 伺服器 （來源 DC） 上的找不到感興趣的 RPC 應用程式，而且傳回伺服器端錯誤 1753年。 1753 錯誤表示，RPC 用戶端 （目的地 DC） 的伺服器端錯誤回應從 RPC 伺服器 （AD 複寫來源 DC） 透過網路接收。 </para>
      <para>1753 錯誤的特定根本原因包括： </para>
      <list class="ordered">
        <listItem>
          <para>永遠不會啟動伺服器應用程式 （也就是永遠不會嘗試在位於上方的 「 詳細資訊 」 圖表中的步驟 #1）。</para>
        </listItem>
        <listItem>
          <para>伺服器應用程式啟動，但無法向 RPC 端點對應程式 （也就是步驟 #1 」 的詳細資訊 」 在上圖中已嘗試，但失敗） 的初始化期間發生一些錯誤。</para>
        </listItem>
        <listItem>
          <para>伺服器應用程式啟動，但之後終止。 （亦即步驟 #1 」 的詳細資訊 」 在上圖中已順利完成，但因為復原稍後伺服器終止）。</para>
        </listItem>
        <listItem>
          <para>伺服器應用程式以手動方式取消登錄其端點 （類似但刻意設計的 3。 不太可能，但為了完整性而加入。）</para>
        </listItem>
        <listItem>
          <para>RPC 用戶端 （目的地 DC） 連線與預期的多，因為 IP 對應錯誤，在 DNS、 WINS 或主機/Lmhosts 檔案的名稱不同的 RPC 伺服器。</para>
        </listItem>
      </list>
      <para>不被因錯誤 1753年: </para>
      <list class="bullet">
        <listItem>
          <para>沒有 RPC 用戶端 （目的地 DC） 和 RPC 伺服器 （來源 DC） 之間的網路連線，透過連接埠 135</para>
        </listItem>
        <listItem>
          <para>RPC 伺服器 （來源 DC） 之間的網路連線缺乏透過暫時連接埠的連接埠 135 和 RPC 用戶端 （目的地 DC）。 </para>
        </listItem>
        <listItem>
          <para>密碼不相符或來源 DC 無法解密 Kerberos 加密封包 </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>解決方式</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>確認登錄其服務端點對應程式服務已啟動</embeddedLabel>
          </para>
          <para>適用於 Windows 2000 和 Windows Server 2003 Dc： 請確定來源 DC，開機進入標準模式。 </para>
          <para>
Windows Server 2008 或 Windows Server 2008 R2： 從來源 DC 的主控台，啟動 服務管理員 (services.msc)，並確認<embeddedLabel>Active Directory 網域服務</embeddedLabel>服務正在執行。 </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>確定 RPC 用戶端 （目的地 DC） 連接到想要的 RPC 伺服器 （來源 DC）</embeddedLabel>
          </para>
          <para>常見的 Active Directory 樹系中的所有網域控制站在 _msdcs 註冊的網域控制站的 CNAME 記錄。&lt;樹系根網域&gt;不論它們位於樹系中網域的 DNS 區域。 DC CNAME 記錄衍生自<embeddedLabel>objectGUID</embeddedLabel>的每個網域控制站的 NTDS 設定物件的屬性。 </para>
          <para>執行複寫為基礎的作業時，目的地 DC 會查詢 DNS 來源網域控制站的 CNAME 記錄。 CNAME 記錄會包含來源 DC 完整的電腦名稱來衍生的來源網域控制站的 IP 位址透過 DNS 用戶端快取查閱，裝載 / LMHost 檔案查閱，主機 A / AAAA 記錄在 DNS 或 WINS。 </para>
          <para>過時的 NTDS 設定物件，並不正確的名稱到 IP 對應 DNS、 WINS、 主機和 LMHOST 檔案中可能會造成 RPC 用戶端 （目的地 DC），連接到錯誤的 RPC 伺服器 (來源 DC)。 此外，不正確的名稱到 IP 對應可能會造成 RPC 用戶端 （目的地 DC） 連線到的電腦，甚至不需要 RPC 伺服器應用程式感興趣 （Active Directory 角色在此情況下） 安裝。 (範例： DC2 的陳舊的主機記錄包含 DC3 或成員電腦的 IP 位址)。 </para>
          <para>請確認來源 DC 的 Active Directory 網域控制站複製目的地中存在的 objectGUID 符合儲存在 Active directory 網域控制站複製來源中來源 DC objectGUID。 如果沒有不一致的情形，使用 repadmin /showobjmeta 上 ntds 設定物件來查看哪一個對應至來源 DC 的最後一個升級 (提示： 比較日期戳記，NTDS 設定物件中的最後一個升級日期針對 /showobjmeta 從建立日期來源網域控制站的 dcpromo.log 檔案。 您可能必須使用上次修改 / DCPROMO 的建立日期。記錄檔本身）。 如果物件的 Guid 不完全相同，目的地 DC 可能會有過時的 NTDS 設定物件的來源 DC 的 CNAME 記錄與錯誤的名稱與 IP 之間的對應是指主機記錄。 </para>
          <para>在目的地 DC，執行 IPCONFIG /ALL 來判斷哪些 DNS 伺服器的 DC 會使用名稱解析所需的目的地：</para>
          <code>c:&gt;ipconfig /all</code>
          <para>在目的地 DC，執行 NSLOOKUP 針對來源網域控制站完整 DC CNAME 記錄：</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>確認 NSLOOKUP 所傳回的 IP 位址 「 擁有 」 主機名稱 / 來源 DC 的安全性識別：</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>或</para>
          <para>登入來源 DC 的主控台，從命令提示字元執行"IPCONFIG"並確認來源 DC 擁有上述 NSLOOKUP 命令所傳回的 IP 位址</para>
          <para>檢查有過時 / 重複的主機，以在 DNS 中的 IP 對應</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>如果主應用程式記錄中，有無效的 IP 位址，請調查是否啟用並正確設定 DNS 清除。 </para><para>如果上述測試或網路追蹤為例則不會顯示傳回無效的 IP 位址的名稱查詢，請考慮 LMHOSTS 檔案和 WINS 伺服器、 主機檔案中的過時項目。 請注意，DNS 伺服器也設定為執行 WINS 後援的名稱解析。</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>驗證伺服器應用程式 (Active Directory et al) 已向 RPC 伺服器 （來源 DC） 上的結束點對應程式</embeddedLabel>
          </para>
          <para>Active Directory 會使用各種已知和動態登錄連接埠。 下表列出已知連接埠和 Active Directory 網域控制站所使用的通訊協定。</para>
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
                  <para>通用類別目錄伺服器</para>
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
                  <para>通用類別目錄伺服器</para>
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
          <para>端點對應程式沒有登錄已知連接埠。 </para>
          <para>Active Directory 與其他應用程式也會註冊接收 RPC 暫時連接埠範圍動態指派連接埠的服務。 這類 RPC 伺服器應用程式以動態方式指派 1024年和 Windows 2000 和 Windows Server 2003 的電腦上的 5000 之間的 TCP 連接埠與 Windows Server 2008 和 Windows Server 2008 R2 電腦上的 49152 到 65535 的範圍之間的連接埠。 複寫所使用的 RPC 連接埠可以是硬式編碼在登錄中所述的步驟<externalLink><linkText>知識庫文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>。 Active Directory 會繼續向 EPM 設定時，若要使用硬式編碼的連接埠。 </para>
          <para>請確認 RPC 伺服器應用程式感興趣的已登錄自己本身的 RPC 伺服器 （來源 DC 在 AD 複寫的情況下） 上的 RPC 端點對應程式。 </para>
          <para>有數種方法來完成這項工作，但其中一個是安裝和執行 PORTQRY 從特殊權限的系統管理員命令提示字元，在主控台中的來源 DC 使用語法： </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>在 portqry 輸出中，請注意動態註冊的 「 MS NT 目錄 DRS 介面 」 的連接埠號碼 (UUID = 351...) 針對<embeddedLabel>ncacn_ip_tcp 通訊協定</embeddedLabel>。 下列程式碼片段會顯示範例 portquery 輸出從 Windows Server 2008 R2 DC 和 UUID / 通訊協定組專供 Active Directory 中反白顯示<embeddedLabel>粗體</embeddedLabel>: </para>
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
          <para>若要解決此錯誤的其他可能方式：</para>
          <list class="ordered">
            <listItem>
              <para>請確認來源 DC 以標準模式開機，且已完全啟動來源 DC 的 OS 和 DC 角色。</para>
            </listItem>
            <listItem>
              <para>確認 Active Directory 網域服務正在執行。 如果服務目前已停止，或未設定使用預設啟動的值，預設啟動值重設，將已修改的 DC 重新啟動然後重試此作業。</para>
            </listItem>
            <listItem>
              <para>請確認 RPC 服務和 RPC 定位程式的啟動值和服務狀態是正確的 RPC 用戶端 （目的地 DC） 和 RPC 伺服器 （來源 DC） 的 OS 版本。 如果服務目前已停止，或未設定使用預設啟動的值，預設啟動值重設，將已修改的 DC 重新啟動然後重試此作業。</para>
              <para>此外，請確定服務內容符合下表所列的預設設定。</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>服務</para>
                    </TD>
                    <TD>
                      <para>預設狀態 （啟動類型） 在 Windows Server 2003 和更新版本 </para>
                    </TD>
                    <TD>
                      <para>在 Windows Server 2000 中的預設狀態 （啟動類型）</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>遠端程序呼叫</para>
                    </TD>
                    <TD>
                      <para>已啟動 （自動）</para>
                    </TD>
                    <TD>
                      <para>已啟動 （自動）</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>遠端程序呼叫定位器</para>
                    </TD>
                    <TD>
                      <para>Null 或已停止 （手動）</para>
                    </TD>
                    <TD>
                      <para>已啟動 （自動）</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>請確認動態連接埠範圍的大小不受限制。 Windows Server 2008 和 Windows Server 2008 R2 NETSH 語法來列舉的 RPC 連接埠範圍如下所示：</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>請確認 KB 224196 中定義的硬式編碼的連接埠定義在動態連接埠範圍內的來源網域控制站作業系統版本。</para>
              <para>檢閱<externalLink><linkText>知識庫文章 224196</linkText> <linkUri> https://support.microsoft.com/kb/224196 </linkUri> </externalLink> ，並確定來源 DC 的作業系統版本週期的硬式編碼的連接埠會落在暫時的連接埠範圍內。</para>
            </listItem>
            <listItem>
              <para>確認 ClientProtocols 機碼下 HKLM\Software\Microsoft\Rpc 存在，並包含下列 5 個預設值：</para>
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
    <title>詳細資訊</title>
    <content>
      <para>
        <embeddedLabel>不正確名稱的對應而造成 RPC 錯誤 1753年-2146893022： 與 ip 的範例： 目標主體名稱格式不正確</embeddedLabel>
      </para>
      <para>Contoso.com 網域包含具有 IP 位址 x.x.1.1 與 x.x.1.2 的 DC1 和 DC2。 主機"A"/"AAAA"記錄，dc2 已正確地登錄所有的設定對 DC1 的 DNS 伺服器上。 此外，DC1 上的主機檔案包含將 DC2s 完整主機名稱對應至 IP 位址 x.x.1.2 的項目。 更新版本中，DC2 的 IP 位址會變更從 X.X.1.2 X.X.1.3 和新的成員電腦已加入使用 IP 位址 x.x.1.2 網域。 AD 複寫嘗試觸發<ui>立即複寫</ui>Active Directory 站台及服務嵌入式管理單元中的命令失敗，發生錯誤 1753年下列的追蹤中所示：</para>
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
      <para>在框架<embeddedLabel>10</embeddedLabel>，目的地 DC 查詢的來源網域控制站的結束點對應程式透過連接埠 135 的 Active Directory 複寫服務類別 UUID E351... </para>
      <para>在框架<embeddedLabel>11</embeddedLabel>，來源 DC，在此情況下還未裝載 DC 角色的成員電腦因此尚未註冊 E351...使用其本機 EPM UUID 複寫服務會以符號錯誤 EP_S_NOT_REGISTERED 對應至 decimal 錯誤 1753年、 十六進位錯誤 0x6d9 和易懂的錯誤回應"there are 沒有可用的終點 」。</para>
      <para>更新版本中，IP 位址 x.x.1.2 成員電腦不會提升為 contoso.com 網域中的"MayberryDC 」 複本。 同樣地，<ui>立即複寫</ui>命令用來觸發複寫，但是這次會失敗且螢幕上錯誤 「 目標主體名稱不正確。 」 電腦的網路介面卡指派 IP 位址 x.x.1.2<placeholder>是</placeholder>網域控制站目前開機進入標準模式和已註冊 E351...複寫服務 UUID，其本機 EPM，但它並未擁有 DC2 的名稱或安全性身分識別，並無法解密從 DC1 Kerberos 要求，因此要求現在便會發生錯誤 「 不正確的目標主體名稱 」。 錯誤會對應至 decimal 錯誤-2146893022： 十六進位錯誤 0x80090322 /。 </para>
      <para>在主控件中過時的項目造成這類無效的主機-IP 對應 / lmhost 檔案中，主機 A / AAAA DNS 或 WINS 中的註冊。 </para>
      <para>摘要：這個範例失敗，因為無效的主機-IP 對應 （在本例中的主機檔案） 會導致目的地 DC，以解析為 「 來源 」 沒有 Active Directory 網域服務的 DC 服務正在執行 （或甚至安裝該項目的） 因此複寫還未註冊 SPN，並傳回錯誤 1753年的來源 DC。 在第二個案例中，無效主機-IP 對應 （一次在主機檔案中） 會導致目的地 DC 連接到已註冊 E351 DC...複寫 SPN，但該來源有不同的主機名稱和安全性身分識別比預期的原始碼 DC 因此嘗試失敗，錯誤-2146893022::目標主體名稱不正確。</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>疑難排解 Active Directory 作業失敗，錯誤 1753年:沒有可用的終點的多個端點。</linkText> 
      <linkUri> https://support.microsoft.com/kb/2089874 </linkUri> 
    </externalLink> 
<externalLink><linkText>知識庫文件使用 Windows Server 2003 支援工具，從產品光碟839880疑難排解的RPC端點對應程式錯誤</linkText> <linkUri> https://support.microsoft.com/kb/839880 </linkUri> </externalLink> 
<externalLink><linkText>知識庫文件 832017 服務概觀和網路連接埠需求 Windows Server system</linkText> <linkUri>https://support.microsoft.com/kb/832017/ </linkUri> </externalLink> 
<externalLink><linkText>知識庫文件 224196 限制 Active Directory 複寫流量和特定的通訊埠的用戶端 RPC 流量</linkText><linkUri> https://support.microsoft.com/kb/224196/ </linkUri> </externalLink> 
<externalLink><linkText>知識庫文章 154596 如何設定 RPC 動態連接埠配置以使用防火牆</linkText><linkUri> https://support.microsoft.com/kb/154596 </linkUri> </externalLink> <externalLink> <linkText>RPC 的運作方式</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>伺服器如何連接會準備</linkText><linkUri> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </linkUri> </externalLink> 
<externalLink><linkText>如何在用戶端建立連接</linkText><linkUri> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </linkUri> </externalLink><externalLink><linkText>註冊介面</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>讓此伺服器在網路上可用</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>註冊端點</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx </linkUri> </externalLink> <externalLink><linkText>接聽的用戶端呼叫</linkText><linkUri> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </linkUri> </externalLink><externalLink><linkText>如何在用戶端建立連接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>限制Active Directory 複寫流量和用戶端 RPC 流量傳送到特定的連接埠</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>中的目標 DC 的 SPNAD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


