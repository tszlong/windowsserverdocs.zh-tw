---
title: rpcping
description: Rpcping 命令的參考文章，可確認執行 Microsoft Exchange Server 的電腦與網路上任何支援的 Microsoft Exchange 用戶端工作站之間的 RPC 連線能力。
ms.topic: reference
ms.assetid: 7382aa0d-90fc-47c0-84b3-15f52dd656d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 380c52f9e567b5f185160e8b6ab068fc791d1029
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030516"
---
# <a name="rpcping"></a>rpcping

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

確認執行 Microsoft Exchange Server 的電腦與網路上任何支援的 Microsoft Exchange 用戶端工作站之間的 RPC 連線能力。 您可以使用此公用程式來檢查 Microsoft Exchange Server 服務是否正透過網路回應來自用戶端工作站的 RPC 要求。

## <a name="syntax"></a>語法

```
rpcping [/t <protseq>] [/s <server_addr>] [/e <endpoint>
        |/f <interface UUID>[,majorver]] [/O <interface object UUID]
        [/i <#_iterations>] [/u <security_package_id>] [/a <authn_level>]
        [/N <server_princ_name>] [/I <auth_identity>] [/C <capabilities>]
        [/T <identity_tracking>] [/M <impersonation_type>]
        [/S <server_sid>] [/P <proxy_auth_identity>] [/F <RPCHTTP_flags>]
        [/H <RPC/HTTP_authn_schemes>] [/o <binding_options>]
        [/B <server_certificate_subject>] [/b] [/E] [/q] [/c]
        [/A <http_proxy_auth_identity>] [/U <HTTP_proxy_authn_schemes>]
        [/r <report_results_interval>] [/v <verbose_level>] [/d]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 一起 `<protseq>` | 指定要使用的通訊協定順序。 可以是其中一個標準 RPC 通訊協定序列： ncacn_ip_tcp、ncacn_np 或 ncacn_HTTP。<p>如果未指定，預設值為 ncacn_ip_tcp。 |
| /s `<server_addr>` | 指定伺服器位址。 如果未指定，將會 ping 本機電腦。 |
| /e `<endpoint>` | 指定要 ping 的端點。 如果沒有指定，將會 ping 目的電腦上的端點對應程式。<p>此選項與介面 (**/f**) 選項互斥。 |
| /o `<binding_options>` | 指定 RPC ping 的系結選項。 |
| /f `<interface UUID>[,Majorver]` | 指定要 ping 的介面。 此選項與 endpoint 選項互斥。 介面已指定為 UUID。<p>如果未指定 *>majorver* ，則會搜尋第1版的介面。<p>當指定介面時， **rpcping** 會查詢目的電腦上的端點對應程式，以取得指定介面的端點。 將使用命令列中指定的選項來查詢端點對應程式。 |
| /O `<object UUID>` | 如果介面已登錄，則指定物件 UUID。 |
| /i `<#_iterations>` | 指定要進行的呼叫數目。 預設值是 1。 如果指定了多個反復專案，這個選項就很適合用來測量連接延遲。 |
| u `<security_package_id>` | 指定安全性封裝 (安全性提供者) RPC 將用來進行呼叫。 安全性封裝會識別為數字或名稱。 如果使用數位，則會與 RpcBindingSetAuthInfoEx API 中的數位相同。 如果您指定此選項，則必須指定 [ *無*] 以外的驗證層級。 此選項沒有預設值。 如果未指定，RPC 將不會使用 ping 的安全性。 下列清單顯示名稱和數位。 名稱不區分大小寫：<ul><li>Negotiate/9 或其中一個 nego、snego 或 negotiate</li><li>NTLM/10 或 NTLM</li><li>SChannel/14 或 SChannel</li><li>Kerberos/16 或 Kerberos</li><li>核心/20 或核心</li></ul> |
| /a `<authn_level>` | 指定要使用的驗證層級。 如果指定此選項，則也必須指定安全性封裝識別碼 (**/u**) 。 如果未指定此選項，RPC 將不會使用 ping 的安全性。 此選項沒有預設值。 可能的值包括：<ul><li>連線</li><li>call</li><li>pkt</li><li>完整性</li><li>privacy</li></ul> |
| N `<server_princ_name>` | 指定伺服器主體名稱。<p>只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| /I `<auth_identity>` | 可讓您指定用來連接到伺服器的替代身分識別。 身分識別的格式為 [使用者]、[網域]、[密碼]。 如果使用者名稱、網域或密碼具有可由 shell 解讀的特殊字元，請以雙引號括住身分識別。 您可以指定 `\*` 而非密碼，而 RPC 會提示您輸入密碼，而不需要在畫面上回應。 如果未指定此欄位，則會使用已登入之使用者的身分識別。<p>只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| /C `<capabilities>` | 指定旗標的十六進位位元遮罩。 只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| 一起 `<identity_tracking>` | 指定靜態或動態。 如果未指定，則會預設為動態。<p>只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| 一樣 `<impersonation_type>` | 指定 anonymous、識別、模擬或委派。 預設值為模擬。<p>只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| /S `<server_sid>` | 指定伺服器的預期 SID。<p>只有在選取驗證等級和安全性套件時，才能使用此欄位。 |
| /P `<proxy_auth_identity>` | 指定要向 RPC/HTTP proxy 驗證的身分識別。 的格式與 **/i** 選項的格式相同。 您必須指定安全性封裝 (**/u**) 、驗證層級 (**/a**) 以及驗證配置 (**/h**) ，才能使用此選項。 |
| /F `<RPCHTTP_flags>` | 指定要針對 RPC/HTTP 前端驗證傳遞的旗標。 旗標可指定為目前已辨識旗標的數位或名稱：<ul><li>使用 SSL/1 或 ssl 或 use_ssl</li><li>使用第一個驗證配置/2 或優先或 use_first</li></ul>您必須指定安全性封裝 (**/u**) 和驗證層級 (**/a**) 才能使用此選項。 |
| /H `<RPC/HTTP_authn_schemes>` | 指定用於 RPC/HTTP 前端驗證的驗證配置。 此選項是以逗號分隔的數值或名稱清單。 範例：基本、NTLM。 可辨識的值為 (名稱不區分大小寫) ：<ul><li>Basic/1 或 Basic</li><li>NTLM/2 或 NTLM</li><li>憑證/65536 或 Cert</li></ul><p>您必須指定安全性封裝 (**/u**) ，以及 (**/a**) 的驗證層級，才能使用此選項。 |
| 後退 `<server_certificate_subject>` | 指定伺服器憑證主體。 您必須使用 SSL，此選項才能運作。<p>您必須指定安全性封裝 (**/u**) ，以及 (**/a**) 的驗證層級，才能使用此選項。 |
| /b | 從伺服器傳送的憑證抓取伺服器憑證，並將它列印到螢幕或記錄檔。 只有在 Proxy echo 選項 (/E) 且已指定使用 SSL 選項時才有效。<p>您必須指定安全性封裝 (**/u**) ，以及 (**/a**) 的驗證層級，才能使用此選項。 |
| /R | 指定 HTTP proxy。 如果 *沒有*，則會使用 RPC proxy。 *預設*值表示在用戶端電腦上使用 IE 設定。 任何其他值都會被視為明確的 HTTP proxy。 如果您未指定此旗標，則會假設為預設值，也就是勾選 IE 設定。 只有在啟用 **/e** (echo) 旗標時，此旗標才有效。 |
| /E | 將 ping 限制為僅限 RPC/HTTP proxy。 Ping 未到達伺服器。 當嘗試建立是否可連接 RPC/HTTP proxy 時，會很有用。 若要指定 HTTP proxy，請使用/R 旗標。 如果在/o 旗標中指定 HTTP proxy，則會忽略此選項。<p>您必須指定安全性封裝 (**/u**) ，以及 (**/a**) 的驗證層級，才能使用此選項。 |
| /q | 指定安靜模式。 除了密碼以外，不會發出任何提示。 假設所有查詢都有 *Y* 回應。 請小心使用此選項。 |
| /C | 使用智慧卡憑證。 rpcping 會提示使用者選擇智慧卡。 |
| /A | 指定要向 HTTP proxy 驗證的身分識別。 的格式與/I 選項的格式相同。<p>您必須指定驗證配置 (/U) 、安全性封裝 (**/u**) 和驗證層級 (**/a**) 才能使用此選項。 |
| /U | 指定用於 HTTP proxy 驗證的驗證配置。 此選項是以逗號分隔的數值或名稱清單。 範例：基本、NTLM。 可辨識的值為 (名稱不區分大小寫) ：<ul><li>Basic/1 或 Basic</li><li>NTLM/2 或 NTLM</li></ul>您必須指定安全性封裝 (**/u**) ，以及 (**/a**) 的驗證層級，才能使用此選項。 |
| /r | 如果指定了多個反復專案，這個選項會讓 **rpcping** 在最後一次呼叫之後，定期顯示目前的執行統計資料。 報表間隔是以秒為單位。 預設值為 15。 |
| /v | 告訴 **rpcping** 輸出的詳細資訊。 預設值為 1。 2和3提供更多 **rpcping**的輸出。 |
| /d | 啟動 RPC 網路診斷 UI。 |
| /p | 指定在驗證失敗時提示輸入認證。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要瞭解您透過 RPC/HTTP 連接的 Exchange 伺服器是否可供存取，請輸入：

```
rpcping /t ncacn_http /s exchange_server /o RpcProxy=front_end_proxy /P username,domain,* /H Basic /u NTLM /a connect /F 3
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
