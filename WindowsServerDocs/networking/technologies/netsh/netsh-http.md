---
title: Netsh 命令都會使用超文字傳輸通訊協定 (HTTP)
description: 使用 netsh http 查詢，並設定 HTTP.sys 設定和參數。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c5f3927abf1a2394c2dd5b8ea664c7de8d5f614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446188"
---
# <a name="netsh-http-commands"></a>Netsh http 命令


使用**netsh http**來查詢及設定 HTTP.sys 設定與參數。  

>[!TIP]
>如果您使用 Windows PowerShell 在執行 Windows Server 2016 或 Windows 10 的電腦上，輸入**netsh**按 Enter 鍵。 在 netsh 提示字元中，輸入**http**按下 Enter，即可取得 netsh http 提示。
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

可以使用 netsh http 命令如下：

- [新增 iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [新增逾時](#add-timeout)
- [新增 urlacl](#add-urlacl)
- [刪除快取](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [刪除逾時](#delete-timeout)
- [刪除 urlacl](#delete-urlacl)
- [排清 logbuffer](#flush-logbuffer)
- [顯示 cachestate](#show-cachestate)
- [顯示 iplisten](#show-iplisten)
- [顯示 servicestate](#show-servicestate)
- [顯示 sslcert](#show-sslcert)
- [顯示逾時](#show-timeout)
- [顯示 urlacl](#show-urlacl)

## <a name="add-iplisten"></a>新增 iplisten

將新的 IP 位址新增至 IP 接聽清單中，但不包括連接埠號碼。

**語法**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | IPv4 或 IPv6 位址新增至 IP 接聽清單。 IP 接聽清單用來限定範圍的 HTTP 服務繫結的位址清單。 "0.0.0.0"表示任何 IPv4 位址和 「::"表示任何 IPv6 位址。 | 必要項 |

---

**範例**

以下是四個範例**新增 iplisten**命令。

-   新增 iplisten ipaddress = fe80::1
-   新增 iplisten ipaddress = 1.1.1.1
-   新增 iplisten ipaddress = 0.0.0.0
-   新增 iplisten ipaddress =::

---

## <a name="add-sslcert"></a>新增 sslcert

加入新的 SSL 伺服器憑證繫結和對應的 IP 位址和連接埠的用戶端憑證原則。

**語法**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**參數**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       指定的 IP 位址和繫結的連接埠。 冒號字元 （:）可做為分隔符號之間的 IP 位址和連接埠號碼。                        | 必要項 |
|                 **certhash**                 |                                     指定憑證的 SHA 雜湊。 此雜湊是 20 個位元組，並指定為十六進位字串。                                      | 必要項 |
|                  **appid**                   |                                                                  指定 GUID 來識別擁有端應用程式。                                                                  | 必要項 |
|              **certstorename**               |                                  指定憑證存放區名稱。 預設為 MY。 憑證必須儲存在本機電腦內容。                                  | 選擇性 |
|        **verifyclientcertrevocation**        |                                                      指定開啟/關閉驗證的用戶端憑證的撤銷。                                                       | 選擇性 |
| **verifyrevocationwithcachedclientcertonly** |                                      指定是否只快取的用戶端憑證的撤銷檢查的使用方式是啟用或停用。                                       | 選擇性 |
|                **usagecheck**                |                                                      指定使用方式檢查是否要啟用或停用。 預設為啟用。                                                       | 選擇性 |
|         **revocationfreshnesstime**          | 指定的時間間隔，以秒為單位，以檢查有更新的憑證撤銷清單 (CRL)。 如果此值為零，則被更新新的 CRL，只有當上一個到期。 | 選擇性 |
|           **urlretrievaltimeout**            |                            嘗試為遠端的 URL 擷取憑證撤銷清單之後，指定逾時間隔 （以毫秒為單位）。                            | 選擇性 |
|             **sslctlidentifier**             |                指定可信任的憑證簽發者的清單。 這份清單可以是電腦會信任的憑證簽發者的子集。                 | 選擇性 |
|             **sslctlstorename**              |                                                指定在 SslCtlIdentifier 儲存所在的本機電腦憑證存放區名稱。                                                | 選擇性 |
|              **dsmapperusage**               |                                                        指定是否啟用或停用 DS 對應程式。 預設為停用。                                                         | 選擇性 |
|          **clientcertnegotiation**           |                                              指定是否啟用或停用憑證的交涉。 預設為停用。                                               | 選擇性 |

---

**範例**

以下是範例**新增 sslcert**命令。

新增 sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>新增逾時

將服務的全域逾時。

**語法** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**參數**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    設定的逾時的類型。                                     |
|    **value**    | 逾時 （以秒為單位） 的值。 如果值為十六進位標記法，再加上前置詞 0 x。 |

---

**範例**

以下是兩個例子**新增逾時**命令。

-   add timeout timeouttype=idleconnectiontimeout value=120
-   新增逾時 timeouttype = headerwaittimeout value = 0x40

---

## <a name="add-urlacl"></a>新增 urlacl

將統一資源定位器 (URL) 保留項目。 此命令會保留非系統管理員使用者和帳戶的 URL。 使用 NT 帳戶名稱以接聽和委派的參數，或使用的 SDDL 字串，則可以指定 DACL。

**語法**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**參數**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          指定完整的統一資源定位器 (URL)。                                           | 必要項 |
|   **user**   |                                                      指定使用者群組名稱                                                       | 必要項 |
|  **listen**  | 指定下列值之一: [是]:允許使用者註冊 Url。 這是預設值。 否：拒絕的使用者無法註冊 Url。 | 選擇性 |
| **delegate** |  指定下列值之一: [是]:可讓使用者無法委派的 Url:拒絕使用者委派的 Url。 這是預設值。  | 選擇性 |
|   **sddl**   |                                                指定描述 DACL 的 SDDL 字串。                                                 | 選擇性 |

---

**範例**

以下是四個範例**新增 urlacl**命令。

- add urlacl =https://+:80/MyUri使用者 = DOMAIN\\使用者
- add urlacl =<https://www.contoso.com:80/MyUri>使用者 = DOMAIN\\使用者接聽 = yes
- add urlacl =<https://www.contoso.com:80/MyUri>使用者 = DOMAIN\\使用者委派 = 否
- add urlacl =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>刪除快取

從 HTTP 服務核心 URI 快取刪除所有項目或指定的項目。

**語法**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**參數**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    指定完整統一資源定位器 (URL)，您想要刪除。                     | 選擇性 |
| **recursive** | 指定是否移除 url 快取下的所有項目。 **[是]** ： 移除所有項目**沒有**： 請勿移除所有項目 | 選擇性 |

---

**範例**

以下是兩個例子**刪除快取**命令。

- 刪除快取 url =<https://www.contoso.com:80/myresource/>遞迴 = yes
- 刪除快取

---

## <a name="delete-iplisten"></a>刪除 iplisten

從 IP 接聽清單中刪除 IP 位址。 IP 接聽清單用來限定範圍的 HTTP 服務繫結的位址清單。

**語法**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | IPv4 或 IPv6 位址，刪除從 IP 接聽清單。 IP 接聽清單用來限定範圍的 HTTP 服務繫結的位址清單。 "0.0.0.0"表示任何 IPv4 位址和 「::"表示任何 IPv6 位址。 這不包括所使用的連接埠號碼。 | 必要項 |

---


**範例**

以下是四個範例**刪除 iplisten**命令。

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   刪除 iplisten ipaddress =::

---

## <a name="delete-sslcert"></a>刪除 sslcert


刪除 SSL 伺服器憑證繫結和對應的 IP 位址和連接埠的用戶端憑證原則。

**語法**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定 IPv4 或 IPv6 位址和連接埠的 SSL 憑證繫結將會刪除。 冒號字元 （:）可做為分隔符號之間的 IP 位址和連接埠號碼。 | 必要項 |

---


**範例**

以下是三個範例**刪除 sslcert**命令。

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>刪除逾時

刪除全域逾時，並可使服務還原為預設值。

**語法**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**參數**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | 指定的逾時設定的類型。 | 必要項 |

---


**範例**

以下是兩個例子**刪除逾時**命令。

-   刪除逾時 timeouttype = idleconnectiontimeout
-   刪除逾時 timeouttype = headerwaittimeout

---

## <a name="delete-urlacl"></a>刪除 urlacl

刪除 URL 保留項目。

**語法**

```powershell
delete urlacl [ url= ] URL
```

**參數**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | 指定完整統一資源定位器 (URL)，您想要刪除。 | 必要項 |

---


**範例**

以下是兩個例子**刪除 urlacl**命令。

- delete urlacl =https://+:80/MyUri
- delete urlacl =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>排清 logbuffer

排清記錄檔的內部緩衝區。

**語法**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>顯示 cachestate

URI 的資源和其相關聯的屬性，則快取清單。 此命令會列出所有資源和其相關聯的屬性，會在 HTTP 回應快取中快取，或顯示單一資源和其相關聯的屬性。

**語法**

```powershell
show cachestate [ [url= ] URL]
```

**參數**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | 指定您想要顯示的完整的 URL。 如果未指定，會顯示所有 Url。 URL 也可能是已註冊 url 前置詞。 | 選擇性 |

---


**範例**

以下是兩個例子**顯示 cachestate**命令：

- 顯示 cachestate url =<https://www.contoso.com:80/myresource>
- 顯示 cachestate

---

## <a name="show-iplisten"></a>顯示 iplisten

顯示 IP 接聽清單中的所有 IP 位址。 IP 接聽清單用來限定範圍的 HTTP 服務繫結的位址清單。 "0.0.0.0"表示任何 IPv4 位址和 「::"表示任何 IPv6 位址。

**語法**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>顯示 servicestate

顯示 HTTP 服務的快照集。

**語法**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**參數**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **檢視**   | 指定是否要檢視根據伺服器工作階段或在要求佇列上的 HTTP 服務狀態的快照集。 | 選擇性 |
| **Verbose** |                指定是否要顯示也會顯示屬性資訊的詳細資訊。                | 選擇性 |

---

**範例**

以下是兩個例子**顯示 servicestate**命令。

-   顯示 servicestate 檢視 = 「 工作階段 」
-   顯示 servicestate 檢視 ="requestq 」

---

## <a name="show-sslcert"></a>顯示 sslcert

顯示安全通訊端層 (SSL) 伺服器憑證繫結和對應的 IP 位址和連接埠的用戶端憑證原則。

**語法**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定的 IPv4 或 IPv6 位址和 SSL 憑證繫結顯示的連接埠。 冒號字元 （:）可做為分隔符號之間的 IP 位址和連接埠號碼。 如果您未指定 ipport，會顯示所有繫結。 | 必要項 |

---


**範例**

以下是五個範例**顯示 sslcert**命令。

-   show sslcert ipport=[fe80::1]:443
-   顯示 sslcert ipport = 1.1.1.1:443
-   顯示 sslcert ipport = 0.0.0.0: 443
-   show sslcert ipport=[::]:443
-   顯示 sslcert

---

## <a name="show-timeout"></a>顯示逾時

會顯示，以秒為單位，HTTP 服務的逾時值。

**語法**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>顯示 urlacl

顯示判別存取控制清單 (Dacl)，指定保留的 URL 或保留的所有 Url。

**語法**

```powershell
show urlacl [ [url= ] URL]
```

**參數**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | 指定您想要顯示的完整的 URL。 如果未指定，會顯示所有 Url。 | 選擇性 |

---


**範例**

以下是三個範例**顯示 urlacl**命令。

- 顯示 urlacl =https://+:80/MyUri
- 顯示 urlacl =<https://www.contoso.com:80/MyUri>
- 顯示 urlacl

---