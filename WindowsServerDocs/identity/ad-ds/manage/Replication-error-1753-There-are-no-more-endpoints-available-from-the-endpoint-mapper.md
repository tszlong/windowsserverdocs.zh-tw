---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 複寫錯誤 1753：端點對應表中無更多可用的端點
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 94e63d439217c21c1634e7f1b685267ad98178b4
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067670"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>複寫錯誤 1753：端點對應表中無更多可用的端點

>適用於：Windows Server

本文說明因 Win32 錯誤1753而失敗之 Active Directory 作業的徵兆、原因和解決步驟：「端點對應程式中沒有其他可用的端點」。

DCDIAG 報告連接測試、Active Directory 複寫測試或 KnowsOfRoleHolders 測試失敗，並出現錯誤1753：「端點對應程式中沒有其他可用的端點」。

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN.EXE 報告複寫嘗試失敗，狀態為1753。
通常會引用1753狀態的 REPADMIN 命令包含但不限於：

* REPADMIN/REPLSUM
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN/SYNCALL

從 CONTOSO-DC2 到 CONTOSO-DC1 之間輸入複寫的範例輸出，會顯示「複寫存取被拒」錯誤，如下所示：

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

Active Directory 網站和服務中的 [ **檢查複寫拓撲** ] 命令會傳回「端點對應程式中沒有其他可用的端點」。

以滑鼠右鍵按一下來源 DC 中的連線物件，並選擇 [ **檢查複寫拓撲** ] 失敗，並出現「端點對應程式中沒有其他可用的端點」。 畫面上的錯誤訊息如下所示：

對話方塊標題文字：檢查複寫拓撲對話方塊郵件內文：嘗試聯繫網域控制站時發生下列錯誤：端點對應程式中沒有其他可用的端點。

Active Directory 網站和服務中的 [ **立即** 複寫] 命令會傳回「端點對應程式中沒有其他可用的端點」。
以滑鼠右鍵按一下來源 DC 中的連線物件，然後選擇 [ **立即** 複寫] 會失敗，並出現「端點對應程式中沒有其他可用的端點」。
畫面上的錯誤訊息如下所示：

對話方塊標題文字：立即複寫對話方塊郵件內文：嘗試將命名內容 \<%directory partition name%> 從網域控制站同步處理 \<Source DC> 至網域控制站時發生下列錯誤 \<Destination DC> ：

端點對應程式中沒有其他可用的端點。
作業將不會繼續

[NTDS KCC]、[NTDS 一般] 或 [Microsoft-Windows-ActiveDirectory_DomainService 事件（具有-2146893022 狀態）都會記錄在 [目錄服務] 記錄檔中事件檢視器。

通常提及-2146893022 狀態的 Active Directory 事件，包括但不限於：

| 事件識別碼 | 事件來源 | 事件字串|
| --- | --- | --- |
| 1655 | NTDS 一般 | Active Directory 嘗試與下列通用類別目錄通訊，但嘗試失敗。 |
| 1925 | NTDS KCC | 嘗試建立下列可寫入目錄分割的複寫連結失敗。 |
| 1265 | NTDS KCC | 知識一致性檢查程式 (KCC) 新增下列目錄分割和來源網域控制站的複製協定失敗。 |

## <a name="cause"></a>原因

下列步驟顯示從使用 RPC 端點對應程式登錄伺服器應用程式時的 RPC 工作流程 (EPM) 步驟1中的步驟1，將 RPC 用戶端的資料傳遞至步驟7中的用戶端應用程式。

### <a name="adds-rpc-workflow"></a>新增 RPC 工作流程

1. 伺服器應用程式會使用 (EPM 的 RPC 端點對應程式來註冊其端點) 
1. 用戶端會代表使用者、OS 或應用程式起始的作業，進行 RPC 呼叫 () 
1. 用戶端 RPC 會與目的電腦 EPM 聯繫，並要求端點完成用戶端呼叫
1. 伺服器電腦的 EPM 以端點回應
1. 用戶端 RPC 與伺服器應用程式聯絡
1. 伺服器應用程式執行呼叫，將結果傳回至用戶端 RPC
1. 用戶端 RPC 將結果傳回到用戶端應用程式

失敗1753是由步驟 #3 和 #4 之間的失敗所產生。 具體而言，錯誤1753表示 RPC 用戶端 (目的地 DC) 能夠透過埠135連接 RPC 伺服器 (來源 DC) ，但 RPC 伺服器 (來源 DC 上的 EPM) 找不到感興趣的 RPC 應用程式，並傳回伺服器端錯誤1753。 如果出現1753錯誤，表示 RPC 用戶端 (目的地 DC) 透過網路從 RPC 伺服器 (AD 複寫來源 DC) 收到伺服器端錯誤回應。

1753錯誤的特定根本原因包括：

* 伺服器應用程式永遠不會啟動 (也就是不會嘗試) 的 [詳細資訊] 圖表中的步驟 #1。
* 伺服器應用程式已啟動，但在初始化期間發生一些失敗，導致無法向 RPC 端點對應程式註冊 (例如，已嘗試在上方的 [詳細資訊] 圖表中進行步驟 #1，但) 失敗。
* 伺服器應用程式已啟動，但後續壞掉。  (亦即，上述「詳細資訊」圖表中的步驟 #1 已成功完成，但稍後因為伺服器壞掉) 而復原。
* 伺服器應用程式會手動取消註冊其端點 (類似于3但刻意進行。 不可能但為了完整性而納入。 ) 
* 由於 DNS、WINS 或主機/Lmhosts 檔案中的 IP 對應錯誤名稱，RPC 用戶端 (目的地 DC) 與所需的 RPC 伺服器連接的 RPC 伺服器不同。

錯誤1753不是由下列原因所造成：

* RPC 用戶端 (目的地 DC) 和 RPC 伺服器之間的網路連線不足， (來源 DC) 透過埠135
* RPC 伺服器 (來源 DC 之間的網路連線不足) 使用埠135和 RPC 用戶端 (目的地 DC) 透過暫時埠。
* 密碼不符或來源 DC 無法將 Kerberos 加密封包解密

## <a name="resolutions"></a>解決方式

確認使用端點對應程式註冊服務的服務已啟動

* 針對 Windows 2000 和 Windows Server 2003 Dc：確定來源 DC 已開機進入正常模式。
* 若為 Windows Server 2008 或 Windows Server 2008 R2：從來源 DC 的主控台中，啟動服務管理員 (services.msc) 並確認 Active Directory Domain Services 服務是否正在執行。

確認 RPC 用戶端 (目的地 DC) 連線到預期的 RPC 伺服器 (來源 DC) 

Common Active Directory 樹系中的所有網域控制站會在 _msdcs 中註冊網域控制站 CNAME 記錄。 \<forest root domain> DNS 區域，不論它們位於樹系內的網域為何。 DC CNAME 記錄衍生自每個網域控制站之 NTDS 設定物件的 objectGUID 屬性。

執行以複寫為基礎的作業時，目的地 DC 會查詢 DNS 中的來源 Dc CNAME 記錄。 CNAME 記錄包含來源 DC 的完整電腦名稱稱，此名稱是用來透過 DNS 用戶端快取查閱、主機/LMHost 檔查閱、主機 A/AAAA 記錄在 DNS 中或 WINS 來衍生來源 Dc IP 位址。

在 DNS、WINS、主機和 LMHOST 檔案中，過時的 NTDS 設定物件和錯誤的名稱對 IP 對應，可能會導致 RPC 用戶端 (目的地 DC) 連接到錯誤的 RPC 伺服器 (來源 DC) 。 此外，錯誤的名稱對 IP 對應可能會導致 RPC 用戶端 (目的地 DC) 連接到甚至沒有相關 RPC 伺服器應用程式的電腦， (此案例中) 安裝的 Active Directory 角色。  (範例： DC2 的過時主機記錄包含 DC3 的 IP 位址或) 的成員電腦。

確認 Active Directory 的來源 DC 的 objectGUID 符合儲存在 Active Directory 之來源 Dc 複本中的來源 DC objectGUID。 如果有不一致的情況，請在 [ntds 設定] 物件上使用 repadmin/showobjmeta，以查看哪一個對應到來源 DC 的最後一個升級 (提示：比較 NTDS 設定物件的日期戳記和來源 Dc dcpromo .log 檔案中的最後一個升級日期。 您可能必須使用 DCPROMO 的上次修改/建立日期。記錄檔本身) 。 如果物件 Guid 不相同，則目的地 DC 可能具有來源 DC 的過時 NTDS 設定物件，其 CNAME 記錄指的是對 IP 對應具有錯誤名稱的主機記錄。

在目的地 DC 上，執行 IPCONFIG/ALL 來判斷目的地 DC 使用的 DNS 伺服器進行名稱解析：

```
c:>ipconfig /all
```

在目的地 DC 上，對來源 Dc 的完全合格 DC CNAME 記錄執行 NSLOOKUP：

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

確認 NSLOOKUP 「擁有」來源 DC 的主機名稱/安全性識別所傳回的 IP 位址：

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

登入來源 DC 的主控台，從命令提示字元執行 "IPCONFIG"，然後確認來源 DC 擁有上面的 NSLOOKUP 命令所傳回的 IP 位址

檢查 DNS 中是否有過時/重複的主機至 IP 對應

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

如果主機記錄中有不正確 IP 位址，請調查是否已啟用並正確設定 DNS 清除。

如果上述測試或網路追蹤未顯示傳回無效 IP 位址的名稱查詢，請考慮主機檔案、LMHOSTS 檔案和 WINS 伺服器中過時的專案。 請注意，DNS 伺服器也可以設定為執行 WINS 回溯名稱解析。

* 確認伺服器應用程式 (Active Directory et) 已向 RPC 伺服器 (來源 DC 註冊端點對應程式) 
* Active Directory 混合使用已知和動態註冊的埠。 下表列出 Active Directory 網域控制站所使用的知名埠和通訊協定。

| RPC 伺服器應用程式 | 連接埠 | TCP | UDP |
| --- | --- | --- | --- |
| DNS 伺服器 | 53 | X | X |
| Kerberos | 88 | X | X |
| LDAP 伺服器 | 389 | X | X |
| Microsoft DS | 445 | X | X |
| LDAP SSL | 636 | X | X |
| 通用類別目錄伺服器 | 3268 | X |   |
| 通用類別目錄伺服器 | 3269 | X |   |

已知的埠未向端點對應程式登錄。

Active Directory 和其他應用程式也會註冊可接收 RPC 暫時埠範圍中動態指派埠的服務。 這類 RPC 伺服器應用程式是在 windows 2000 和 Windows server 2003 電腦上的1024與5000，以及 windows server 49152 與 Windows Server 65535 R2 電腦上的2008與2008範圍之間，動態指派的 TCP 埠。 複寫所使用的 RPC 埠可以使用 [知識庫文章 224196](https://support.microsoft.com/kb/224196)中記載的步驟，在登錄中進行硬式編碼。 當設定為使用硬式編碼埠時，Active Directory 會繼續向 EPM 註冊。

在 AD 複寫) 的情況下，確認相關的 RPC 伺服器應用程式已向 RPC 伺服器上的 RPC 端點對應程式註冊自己的 rpc 端點對應程式 (來源 DC。

有幾種方式可以完成這項工作，但其中一個方法是使用語法，在來源 DC 的主控台上使用系統管理員許可權的 CMD 提示字元來安裝和執行 PORTQRY：

```
portquery -n <source DC> -e 135 > file.txt
```

在 portqry 輸出中，請注意 ncacn_ip_tcp 通訊協定的 "MS NT Directory DRS Interface" (UUID = 351 ... ) 動態註冊的埠號碼。 下列程式碼片段顯示來自 Windows Server 2008 R2 DC 的範例 portquery 輸出：

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157]
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

解決此錯誤的其他可能方法：

* 確認來源 DC 已在正常模式下開機，而且來源 DC 上的 OS 和 DC 角色已完全啟動。
* 確認 Active Directory 網域服務正在執行。 如果服務目前已停止或未以預設啟動值設定，請重設預設啟動值、重新開機已修改的 DC，然後重試此作業。
* 確認 rpc 用戶端的啟動值和服務狀態，以及 rpc 用戶端的 OS 版本是否正確（ (目的地 DC) 和 RPC 伺服器 (來源 DC) 。 如果服務目前已停止或未以預設啟動值設定，請重設預設啟動值、重新開機已修改的 DC，然後重試此作業。
   * 此外，請確定服務內容符合下表所列的預設設定。

      | 服務 | Windows Server 2003 和更新版本中的預設狀態 (啟動類型)  | Windows Server 2000 中的預設狀態 (啟動類型)  |
      | --- | --- | --- |
      | 遠端程序呼叫 | 已開始 (自動)  | 已開始 (自動)  |
      | 遠端程序呼叫定位器 | Null 或已停止 (手動)  | 已開始 (自動)  |

* 確認動態埠範圍的大小未受限制。 列舉 RPC 埠範圍的 Windows Server 2008 和 Windows Server 2008 R2 NETSH 語法如下所示：

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* 確認 KB 224196 中定義的硬式編碼埠定義落在來源 Dc 作業系統版本的動態埠範圍內。 請參閱 [知識庫文章 224196](https://support.microsoft.com/kb/224196) ，並確定硬式編碼埠落在來源 DC 的作業系統版本的暫時埠範圍內。

* 確認 ClientProtocols 索引鍵存在於 HKLM\Software\Microsoft\Rpc 下，而且包含下列5個預設值：

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>詳細資訊

IP 對應的錯誤名稱範例，導致 RPC 錯誤1753與-2146893022：目標主體名稱不正確

Contoso.com 網域是由 DC1 和 DC2 所組成，其 IP 位址為 x. x. 1.1 和 x. 1.2。 DC2 的主機 "A"/"AAAA" 記錄已在為 DC1 設定的所有 DNS 伺服器上正確註冊。 此外，DC1 上的 HOSTS 檔案包含一個專案對應 DC2s 完整的主機名稱至 IP 位址 2.x. 1.2。 之後，DC2's IP 位址會從 X. x. X. X. X. X. X. X. X. X. X. X. X. X. X. X. x. x。 Active Directory 網站和服務嵌入式管理單元中的 [ **立即** 複寫] 命令所觸發的 AD 複寫嘗試失敗，錯誤為1753，如下列追蹤所示：

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

在畫面格 **10** ，目的地 dc 會針對 Active Directory replication SERVICE Class UUID E351，在埠135上查詢來源 dc 端點對應程式。

在畫面格 **11** 中，來源 DC （在此案例中為尚未裝載 DC 角色的成員電腦，因此尚未註冊 E351 .。。具有本機 EPM 的複寫服務的 UUID 會回應符號錯誤 EP_S_NOT_REGISTERED 其對應至十進位錯誤1753、十六進位錯誤0x6d9 和易記錯誤「端點對應程式中沒有可用的端點」。

之後，在 contoso.com 網域中，會將 IP 位址為 x. 1.2 的成員電腦升級為 "MayberryDC" 複本。 同樣地， **現在** 會使用 [立即複寫] 命令來觸發複寫，但這次會失敗，並出現畫面錯誤「目標主體名稱不正確」。 指派了網路介面卡的電腦 IP 位址（x. 1.2 是網域控制站）目前已開機進入正常模式，且已註冊 E351 .。。複寫服務 UUID 與其本機 EPM，但不擁有 DC2 的名稱或安全性識別，因此無法將來自 DC1 的 Kerberos 要求解密，因此要求現在會失敗，並出現錯誤「目標主體名稱不正確」。 錯誤會對應到 decimal error-2146893022/hex error 0x80090322。

這類不正確主機對 IP 對應可能是因為主機/lmhost 檔中有過時的專案，請在 DNS 中裝載 A/AAAA 註冊或 WINS。

摘要：此範例失敗，因為在此情況下，主機檔案中的主機對 IP 對應 (無效) 導致目的地 DC 解析為沒有執行 Active Directory Domain Services (服務的「來源」 DC，或甚至是針對) 該情況進行安裝，因此複寫 SPN 尚未註冊，且來源 DC 傳回錯誤1753。 在第二個案例中，主機檔案中的主機對 IP 對應 (重複，) 導致目的地 DC 連接到已註冊 E351 的 DC .。。複寫 SPN，但該來源的主機名稱和安全性識別與預期的來源 DC 不同，因此嘗試失敗併發生錯誤-2146893022：目標主體名稱不正確。

## <a name="related-topics"></a>相關主題

* [針對因錯誤1753而失敗的 Active Directory 作業進行疑難排解：端點對應程式中沒有其他可用的端點。](https://support.microsoft.com/kb/2089874)
* [知識庫文章839880使用產品 CD 中的 Windows Server 2003 支援工具針對 RPC 端點對應程式錯誤進行疑難排解](https://support.microsoft.com/kb/839880)
* [知識庫文章832017服務總覽和 Windows Server 系統的網路埠需求](https://support.microsoft.com/kb/832017/)
* [知識庫文章224196限制將 Active Directory 複寫流量和用戶端 RPC 流量傳送至特定埠](https://support.microsoft.com/kb/224196/)
* [知識庫文章154596如何設定 RPC 動態埠配置以使用防火牆](https://support.microsoft.com/kb/154596)
* [RPC 的運作方式](/windows/win32/rpc/how-rpc-works)
* [伺服器如何準備連接](/windows/win32/rpc/how-the-server-prepares-for-a-connection)
* [用戶端建立連接的方式](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [註冊介面](/windows/win32/rpc/registering-the-interface)
* [讓伺服器可在網路上使用](/windows/win32/rpc/making-the-server-available-on-the-network)
* [註冊端點](/windows/win32/rpc/registering-endpoints)
* [接聽用戶端呼叫](/windows/win32/rpc/listening-for-client-calls)
* [用戶端建立連接的方式](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [限制對特定埠的 Active Directory 複寫流量和用戶端 RPC 流量](https://support.microsoft.com/kb/224196)
* [AD DS 中目標 DC 的 SPN](/openspecs/windows_protocols/ms-drsr/41efc56e-0007-4e88-bafe-d7af61efd91f)
