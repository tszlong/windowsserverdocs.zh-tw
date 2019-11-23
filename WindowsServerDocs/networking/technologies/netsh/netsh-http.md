---
title: 超文字傳輸通訊協定（HTTP）的 Netsh 命令
description: 使用 netsh HTTP 來查詢和設定 HTTP.SYS 設定和參數。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401876"
---
# <a name="netsh-http-commands"></a>Netsh http 命令


使用**netsh HTTP**來查詢和設定 HTTP.sys 設定和參數。  

>[!TIP]
>如果您在執行 Windows Server 2016 或 Windows 10 的電腦上使用 Windows PowerShell，請輸入**netsh** ，然後按 enter。 在 netsh 提示字元中輸入**HTTP** ，然後按 enter 鍵以取得 netsh HTTP 提示字元。
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh HTTP\>

可用的 netsh HTTP 命令如下：

- [新增 iplisten](#add-iplisten)
- [新增 sslcert](#add-sslcert)
- [新增超時](#add-timeout)
- [新增 urlacl](#add-urlacl)
- [刪除快取](#delete-cache)
- [刪除 iplisten](#delete-iplisten)
- [刪除 sslcert](#delete-sslcert)
- [刪除超時](#delete-timeout)
- [刪除 urlacl](#delete-urlacl)
- [清除 logbuffer](#flush-logbuffer)
- [顯示 cachestate](#show-cachestate)
- [顯示 iplisten](#show-iplisten)
- [顯示 servicestate](#show-servicestate)
- [顯示 sslcert](#show-sslcert)
- [顯示超時](#show-timeout)
- [顯示 urlacl](#show-urlacl)

## <a name="add-iplisten"></a>新增 iplisten

將新的 IP 位址新增至 IP 接聽清單，但不包括埠號碼。

**語法**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ip** | 要新增至 IP 接聽清單的 IPv4 或 IPv6 位址。 IP 接聽清單可用來界定 HTTP 服務所系結的地址清單範圍。 「0.0.0.0」表示任何 IPv4 位址和「：：」表示任何 IPv6 位址。 | 必要 |

---

**典型**

以下是**add iplisten**命令的四個範例。

-   add iplisten ipaddress = fe80：：1
-   新增 iplisten ipaddress = 1.1.1。1
-   新增 iplisten ipaddress = 0.0.0。0
-   新增 iplisten ipaddress =：：

---

## <a name="add-sslcert"></a>新增 sslcert

為 IP 位址和埠加入新的 SSL 伺服器憑證系結和對應的用戶端憑證原則。

**語法**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**參數**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       指定系結的 IP 位址和埠。 冒號字元（:)會用來做為 IP 位址與埠號碼之間的分隔符號。                        | 必要 |
|                 **certhash**                 |                                     指定憑證的 SHA 雜湊。 此雜湊長度為20個位元組，並指定為十六進位字串。                                      | 必要 |
|                  **標識**                   |                                                                  指定識別擁有應用程式的 GUID。                                                                  | 必要 |
|              **certstorename**               |                                  指定憑證的存放區名稱。 預設值為 MY。 憑證必須儲存在本機電腦內容中。                                  | 選擇性 |
|        **verifyclientcertrevocation**        |                                                      指定是否要開啟/關閉用戶端憑證撤銷的驗證。                                                       | 選擇性 |
| **verifyrevocationwithcachedclientcertonly** |                                      指定是否只針對撤銷檢查啟用或停用快取用戶端憑證的使用方式。                                       | 選擇性 |
|                **usagecheck**                |                                                      指定是否啟用或停用使用檢查。 預設為啟用。                                                       | 選擇性 |
|         **revocationfreshnesstime**          | 指定檢查已更新憑證撤銷清單（CRL）的時間間隔（以秒為單位）。 如果此值為零，則只有在前一個 CRL 過期時，才會更新新的 CRL。 | 選擇性 |
|           **urlretrievaltimeout**            |                            指定嘗試取得遠端 URL 的憑證撤銷清單後的逾時間隔（以毫秒為單位）。                            | 選擇性 |
|             **sslctlidentifier**             |                指定可信任的憑證簽發者清單。 此清單可以是電腦信任之憑證簽發者的子集。                 | 選擇性 |
|             **sslctlstorename**              |                                                指定儲存 SslCtlIdentifier 之 LOCAL_MACHINE 下的憑證存放區名稱。                                                | 選擇性 |
|              **dsmapperusage**               |                                                        指定是否啟用或停用 DS 對應程式。 預設為停用。                                                         | 選擇性 |
|          **clientcertnegotiation**           |                                              指定是否啟用或停用憑證的協商。 預設為停用。                                               | 選擇性 |

---

**典型**

以下是**add sslcert**命令的範例。

add sslcert ipport = 1.1.1.1： 443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>新增超時

將全域超時加入至服務。

**語法** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**參數**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    設定的超時類型。                                     |
|    **值**    | 超時時間的值（以秒為單位）。 如果值是十六進位標記法，則新增前置詞0x。 |

---

**典型**

以下是**add timeout**命令的兩個範例。

-   add timeout timeouttype = idleconnectiontimeout value = 120
-   add timeout timeouttype = headerwaittimeout value = 0x40

---

## <a name="add-urlacl"></a>新增 urlacl

加入統一資源定位器（URL）保留專案。 此命令會保留非系統管理員使用者和帳戶的 URL。 您可以使用 NT 帳戶名稱搭配接聽和委派參數，或使用 SDDL 字串來指定 DACL。

**語法**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**參數**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **連結**    |                                          指定完整的統一資源定位器（URL）。                                           | 必要 |
|   **使用者**   |                                                      指定使用者或使用者組名                                                       | 必要 |
|  **接聽**  | 指定下列其中一個值：是：允許使用者註冊 Url。 這是預設值。 否：拒絕使用者註冊 Url。 | 選擇性 |
| **委託人** |  指定下列其中一個值： [是]： [允許使用者委派 Url]： [拒絕使用者委派 Url]。 這是預設值。  | 選擇性 |
|   **sddl**   |                                                指定描述 DACL 的 SDDL 字串。                                                 | 選擇性 |

---

**典型**

以下是**add urlacl**命令的四個範例。

- 新增 urlacl url =https://+:80/MyUri 使用者 = 網域\\使用者
- 新增 urlacl url =<https://www.contoso.com:80/MyUri> 使用者 = 網域\\使用者接聽 = 是
- 新增 urlacl url =<https://www.contoso.com:80/MyUri> 使用者 = 網域\\使用者委派 = 否
- 新增 urlacl url =https://+:80/MyUri sddl = 。

---

## <a name="delete-cache"></a>刪除快取

從 HTTP 服務核心 URI 快取中刪除所有專案，或指定的專案。

**語法**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**參數**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **連結**    |                    指定您想要刪除的完整統一資源定位器（URL）。                     | 選擇性 |
| **式** | 指定是否要移除 url 快取底下的所有專案。 **是**：移除所有專案**否**：不要移除所有專案 | 選擇性 |

---

**典型**

以下是**delete cache**命令的兩個範例。

- 刪除快取 url =<https://www.contoso.com:80/myresource/> 遞迴 = 是
- 刪除快取

---

## <a name="delete-iplisten"></a>刪除 iplisten

從 IP 接聽清單中刪除 IP 位址。 IP 接聽清單可用來界定 HTTP 服務所系結的地址清單範圍。

**語法**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**參數**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ip** | 要從 IP 接聽清單中刪除的 IPv4 或 IPv6 位址。 IP 接聽清單可用來界定 HTTP 服務所系結的地址清單範圍。 「0.0.0.0」表示任何 IPv4 位址和「：：」表示任何 IPv6 位址。 這不包含通訊埠編號。 | 必要 |

---


**典型**

以下是**delete iplisten**命令的四個範例。

-   delete iplisten ipaddress = fe80：：1
-   delete iplisten ipaddress = 1.1.1。1
-   delete iplisten ipaddress = 0.0.0。0
-   delete iplisten ipaddress =：：

---

## <a name="delete-sslcert"></a>刪除 sslcert


刪除 IP 位址和埠的 SSL 伺服器憑證系結和對應的用戶端憑證原則。

**語法**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定要刪除 SSL 憑證系結的 IPv4 或 IPv6 位址和埠。 冒號字元（:)會用來做為 IP 位址與埠號碼之間的分隔符號。 | 必要 |

---


**典型**

以下是**delete sslcert**命令的三個範例。

- delete sslcert ipport = 1.1.1.1：443
- delete sslcert ipport = 0.0.0.0：443
- delete sslcert ipport = [：：]：443

---

## <a name="delete-timeout"></a>刪除超時

刪除全域超時，並讓服務還原為預設值。

**語法**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**參數**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | 指定超時設定的類型。 | 必要 |

---


**典型**

以下是**delete timeout**命令的兩個範例。

-   delete timeout timeouttype = idleconnectiontimeout
-   delete timeout timeouttype = headerwaittimeout

---

## <a name="delete-urlacl"></a>刪除 urlacl

刪除 URL 保留專案。

**語法**

```powershell
delete urlacl [ url= ] URL
```

**參數**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **連結** | 指定您想要刪除的完整統一資源定位器（URL）。 | 必要 |

---


**典型**

以下是**delete urlacl**命令的兩個範例。

- 刪除 urlacl url =https://+:80/MyUri
- 刪除 urlacl url =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>清除 logbuffer

排清記錄檔的內部緩衝區。

**語法**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>顯示 cachestate

列出快取的 URI 資源及其相關聯的屬性。 此命令會列出在 HTTP 回應快取中快取的所有資源及其相關聯的屬性，或顯示單一資源及其相關聯的屬性。

**語法**

```powershell
show cachestate [ [url= ] URL]
```

**參數**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **連結** | 指定您想要顯示的完整 URL。 如果未指定，則會顯示所有 Url。 URL 也可以是已註冊 Url 的前置詞。 | 選擇性 |

---


**典型**

以下是**show cachestate**命令的兩個範例：

- 顯示 cachestate url =<https://www.contoso.com:80/myresource>
- 顯示 cachestate

---

## <a name="show-iplisten"></a>顯示 iplisten

顯示 IP 接聽清單中的所有 IP 位址。 IP 接聽清單可用來界定 HTTP 服務所系結的地址清單範圍。 「0.0.0.0」表示任何 IPv4 位址和「：：」表示任何 IPv6 位址。

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
|  **檢視**   | 指定是否要根據伺服器會話或要求佇列，來查看 HTTP 服務狀態的快照集。 | 選擇性 |
| **冗余** |                指定是否要顯示也會顯示內容資訊的詳細資訊。                | 選擇性 |

---

**典型**

以下是**show servicestate**命令的兩個範例。

-   顯示 servicestate 視圖 = "會話"
-   顯示 servicestate view = "requestq"

---

## <a name="show-sslcert"></a>顯示 sslcert

顯示 IP 位址和埠的安全通訊端層（SSL）伺服器憑證系結和對應的用戶端憑證原則。

**語法**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**參數**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定 SSL 憑證系結所顯示的 IPv4 或 IPv6 位址和埠。 冒號字元（:)會用來做為 IP 位址與埠號碼之間的分隔符號。 如果您未指定 ipport，則會顯示所有系結。 | 必要 |

---


**典型**

以下是**show sslcert**命令的五個範例。

-   show sslcert ipport = [fe80：： 1]：443
-   顯示 sslcert ipport = 1.1.1.1：443
-   顯示 sslcert ipport = 0.0.0.0：443
-   show sslcert ipport = [：：]：443
-   顯示 sslcert

---

## <a name="show-timeout"></a>顯示超時

顯示 HTTP 服務的超時值（以秒為單位）。

**語法**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>顯示 urlacl

針對指定的保留 URL 或所有保留的 Url，顯示任意存取控制清單（Dacl）。

**語法**

```powershell
show urlacl [ [url= ] URL]
```

**參數**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **連結** | 指定您想要顯示的完整 URL。 如果未將其顯示，則顯示所有 Url。 | 選擇性 |

---


**典型**

以下是**顯示 urlacl**命令的三個範例。

- 顯示 urlacl url =https://+:80/MyUri
- 顯示 urlacl url =<https://www.contoso.com:80/MyUri>
- 顯示 urlacl

---