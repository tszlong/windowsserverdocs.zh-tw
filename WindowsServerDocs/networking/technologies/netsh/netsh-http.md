---
title: 超文字傳輸通訊協定 (HTTP) 的 netsh 命令
description: 使用 netsh http 查詢和設定 HTTP.sys 設定和參數。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401876"
---
# <a name="netsh-http-commands"></a>netsh http 命令


使用 **netsh http** 查詢和設定 HTTP.sys 設定和參數。  

>[!TIP]
>如果您在執行 Windows Server 2016 或 Windows 10 的電腦上使用 Windows PowerShell，請輸入 **netsh**，然後按 Enter 鍵。 在 netsh 提示字元中輸入 **http**，然後按 Enter 鍵，以取得 netsh http 提示字元。
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

可用的 netsh http 命令如下：

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [add urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [delete timeout](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [flush logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [show servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [show timeout](#show-timeout)
- [show urlacl](#show-urlacl)

## <a name="add-iplisten"></a>add iplisten

將新的 IP 位址新增至 IP 接聽清單 (不包括連接埠號碼)。

**語法**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | 要新增至 IP 接聽清單的 IPv4 或 IPv6 位址。 IP 接聽清單可用來界定 HTTP 服務所繫結的位址清單範圍。 "0.0.0.0" 表示任何 IPv4 位址，"::" 表示任何 IPv6 位址。 | 必要 |

---

**範例**

以下是 **add iplisten** 命令的四個範例。

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>add sslcert

為 IP 位址和連接埠新增新的 SSL 伺服器憑證繫結和對應的用戶端憑證原則。

**語法**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**參數**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       指定繫結的 IP 位址和連接埠。 冒號字元 (:) 可作為 IP 位址與連接埠號碼之間的分隔符號。                        | 必要 |
|                 **certhash**                 |                                     指定憑證的 SHA 雜湊。 此雜湊的長度為 20 個位元組，且會指定為十六進位字串。                                      | 必要 |
|                  **appid**                   |                                                                  指定用來識別主控應用程式的 GUID。                                                                  | 必要 |
|              **certstorename**               |                                  指定憑證的存放區名稱。 預設為 MY。 憑證必須儲存在本機電腦內容中。                                  | 選用 |
|        **verifyclientcertrevocation**        |                                                      指定要開啟/關閉用戶端憑證撤銷的驗證。                                                       | 選用 |
| **verifyrevocationwithcachedclientcertonly** |                                      指定要啟用還是停用僅使用快取的用戶端憑證進行撤銷檢查的功能。                                       | 選用 |
|                **usagecheck**                |                                                      指定要啟用還是停用使用檢查。 預設值為啟用。                                                       | 選用 |
|         **revocationfreshnesstime**          | 指定檢查是否有更新的憑證撤銷清單 (CRL) 的時間間隔 (以秒為單位)。 如果此值為零，則只有在前一個 CRL 過期時，才會更新新的 CRL。 | 選用 |
|           **urlretrievaltimeout**            |                            指定嘗試擷取遠端 URL 的憑證撤銷清單之後的逾時間隔 (以毫秒為單位)。                            | 選用 |
|             **sslctlidentifier**             |                指定可信任的憑證簽發者清單。 此清單可以是電腦信任之憑證簽發者的子集。                 | 選用 |
|             **sslctlstorename**              |                                                指定儲存 SslCtlIdentifier 儲存所在之 LOCAL_MACHINE 下的憑證存放區名稱。                                                | 選用 |
|              **dsmapperusage**               |                                                        指定要啟用還是停用 DS 對應工具。 預設值為停用。                                                         | 選用 |
|          **clientcertnegotiation**           |                                              指定要啟用還是停用憑證的交涉。 預設值為停用。                                               | 選用 |

---

**範例**

以下是 **add sslcert** 命令的範例。

add sslcert ipport=1.1.1.1:443 certhash=0102030405060708090A0B0C0D0E0F1011121314 appid={00112233-4455-6677-8899- AABBCCDDEEFF}

---

## <a name="add-timeout"></a>add timeout

將全域逾時新增至服務。

**語法** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**參數**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    設定的逾時類型。                                     |
|    **值**    | 逾時的值 (以秒為單位)。 若此值採用十六進位標記法，請新增前置詞 0x。 |

---

**範例**

以下是 **add timeout** 命令的兩個範例。

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>add urlacl

加入統一資源定位器 (URL) 保留項目。 此命令會保留非系統管理員使用者和帳戶的 URL。 您可以使用 NT 帳戶名稱搭配接聽和委派參數來指定 DACL，或使用 SDDL 字串來指定。

**語法**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**參數**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          指定完整的統一資源定位器 (URL)。                                           | 必要 |
|   **user**   |                                                      指定使用者或使用者群組名稱                                                       | 必要 |
|  **listen**  | 指定下列其中一個值：是：允許使用者註冊 URL。 這是預設值。 否：拒絕使用者註冊 URL。 | 選用 |
| **delegate** |  指定下列其中一個值：是：允許使用者委派 URL。否：拒絕使用者委派 URL。 這是預設值。  | 選用 |
|   **sddl**   |                                                指定說明 DACL 的 SDDL 字串。                                                 | 選用 |

---

**範例**

以下是 **add urlacl** 命令的四個範例。

- add urlacl url=https://+:80/MyUri user=DOMAIN\\ user
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user listen=yes
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user delegate=no
- add urlacl url=https://+:80/MyUri sddl=...

---

## <a name="delete-cache"></a>delete cache

從 HTTP 服務核心 URI 快取中刪除所有項目或指定的項目。

**語法**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**參數**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    指定您要刪除的完整統一資源定位器 (URL)。                     | 選用 |
| **recursive** | 指定是否要移除 URL 快取下的所有項目。 **是**：移除所有項目。**否**：不移除所有項目 | 選用 |

---

**範例**

以下是 **delete cache** 命令的兩個範例。

- delete cache url=<https://www.contoso.com:80/myresource/> recursive=yes
- delete cache

---

## <a name="delete-iplisten"></a>delete iplisten

從 IP 接聽清單中刪除 IP 位址。 IP 接聽清單可用來界定 HTTP 服務所繫結的位址清單範圍。

**語法**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | 要從 IP 接聽清單中刪除的 IPv4 或 IPv6 位址。 IP 接聽清單可用來界定 HTTP 服務所繫結的位址清單範圍。 "0.0.0.0" 表示任何 IPv4 位址，"::" 表示任何 IPv6 位址。 其中不包括連接埠號碼。 | 必要 |

---


**範例**

以下是 **delete iplisten** 命令的四個範例。

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>delete sslcert


刪除 IP 位址和連接埠的 SSL 伺服器憑證繫結和對應的用戶端憑證原則。

**語法**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定要刪除 SSL 憑證繫結的 IPv4 或 IPv6 位址和連接埠。 冒號字元 (:) 可作為 IP 位址與連接埠號碼之間的分隔符號。 | 必要 |

---


**範例**

以下是 **delete sslcert** 命令的三個範例。

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>delete timeout

刪除全域逾時，並且使服務還原為預設值。

**語法**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**參數**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | 指定逾時設定的類型。 | 必要 |

---


**範例**

以下是 **delete timeout** 命令的兩個範例。

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>delete urlacl

刪除 URL 保留。

**語法**

```powershell
delete urlacl [ url= ] URL
```

**參數**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | 指定您要刪除的完整統一資源定位器 (URL)。 | 必要 |

---


**範例**

以下是 **delete urlacl** 命令的兩個範例。

- delete urlacl url=https://+:80/MyUri
- delete urlacl url=<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>flush logbuffer

排清記錄檔的內部緩衝區。

**語法**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>show cachestate

列出快取的 URI 資源及其相關聯的屬性。 此命令會列出在 HTTP 回應快取中快取的所有資源及其相關聯的屬性，或顯示單一資源及其相關聯的屬性。

**語法**

```powershell
show cachestate [ [url= ] URL]
```

**參數**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | 指定您要顯示的完整 URL。 若未指定，則會顯示所有 URL。 URL 也可以是已註冊 URL 的前置詞。 | 選用 |

---


**範例**

以下是 **show cachestate** 命令的兩個範例：

- show cachestate url=<https://www.contoso.com:80/myresource>
- show cachestate

---

## <a name="show-iplisten"></a>show iplisten

顯示 IP 接聽清單中的所有 IP 位址。 IP 接聽清單可用來界定 HTTP 服務所繫結的位址清單範圍。 "0.0.0.0" 表示任何 IPv4 位址，"::" 表示任何 IPv6 位址。

**語法**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>show servicestate

顯示 HTTP 服務的快照集。

**語法**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**參數**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **檢視**   | 指定要根據伺服器工作階段還是要求佇列來檢視 HTTP 服務狀態的快照集。 | 選用 |
| **Verbose** |                指定是否要顯示也會顯示屬性資訊的詳細資訊。                | 選用 |

---

**範例**

以下是 **show servicestate** 命令的兩個範例。

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>show sslcert

顯示 IP 位址和連接埠的安全通訊端層 (SSL) 伺服器憑證繫結和對應的用戶端憑證原則。

**語法**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定要顯示 SSL 憑證繫結的 IPv4 或 IPv6 位址和連接埠。 冒號字元 (:) 可作為 IP 位址與連接埠號碼之間的分隔符號。 若未指定 ipport，則會顯示所有繫結。 | 必要 |

---


**範例**

以下是 **show sslcert** 命令的五個範例。

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
-   show sslcert ipport=[::]:443
-   show sslcert

---

## <a name="show-timeout"></a>show timeout

顯示 HTTP 服務的逾時值 (以秒為單位)。

**語法**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>show urlacl

針對指定的保留 URL 或所有保留的 URL 顯示判別存取控制清單 (DACL)。

**語法**

```powershell
show urlacl [ [url= ] URL]
```

**參數**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | 指定您要顯示的完整 URL。 若未指定，則會顯示所有 URL。 | 選用 |

---


**範例**

以下是 **show urlacl** 命令的三個範例。

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- show urlacl

---