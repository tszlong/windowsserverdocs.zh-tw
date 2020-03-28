---
title: Netsh 的 interface portproxy 命令
description: 使用 netsh interface portproxy 命令來作為 IPv4 與 IPv6 網路和應用程式之間的 Proxy。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.openlocfilehash: 645786484881af3a0f6d9503e1f3fcde32a2cdfe
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316732"
---
# <a name="netsh-interface-portproxy-commands"></a>Netsh interface portproxy 命令

使用 **netsh interface portproxy** 命令來作為 IPv4 與 IPv6 網路和應用程式之間的 Proxy。 您可以透過下列方式使用這些命令來建立 Proxy 服務：

-   已設定 IPv4 的電腦和應用程式訊息傳送到其他已設定 IPv4 的電腦和應用程式。

-   已設定 IPv4 的電腦和應用程式訊息傳送到已設定 IPv6 的電腦和應用程式。

-   已設定 IPv6 的電腦和應用程式訊息傳送到已設定 IPv4 的電腦和應用程式。

-   已設定 IPv6 的電腦和應用程式訊息傳送到其他已設定 IPv6 的電腦和應用程式。

使用這些命令來撰寫批次檔或指令碼時，每個命令都必須以 **netsh interface portproxy** 開頭。 例如，使用 **delete v4tov6** 命令來指定 portproxy 伺服器要從伺服器所接聽的 IPv4 位址清單中刪除 IPv4 連接埠和位址時，批次檔或指令碼必須使用下列語法：

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

可用的 netsh interface portproxy 命令如下：

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [show all](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

portproxy 伺服器會接聽傳送至特定連接埠和 IPv4 位址的訊息，並對應連接埠和 IPv4 位址以傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>參數


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv4 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv4 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v4tov6"></a>add v4tov6

portproxy 伺服器會接聽傳送至特定連接埠和 IPv4 位址的訊息，並對應連接埠和 IPv6 位址以傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv4 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv6 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v6tov4"></a>add v6tov4

portproxy 伺服器會接聽傳送至特定連接埠和 IPv6 位址的訊息，並對應連接埠和 IPv4 位址以對其傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv4 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v6tov6"></a>add v6tov6

portproxy 伺服器會接聽傳送至特定連接埠和 IPv6 位址的訊息，並對應連接埠和 IPv6 位址以對其傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv6 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="delete-v4tov4"></a>delete v4tov4

portproxy 伺服器會從伺服器所接聽的 IPv4 連接埠和位址清單中刪除 IPv4 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv4 連接埠。                                    |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定位址，則預設值是本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v4tov6"></a>delete v4tov6

portproxy 伺服器會從伺服器所接聽的 IPv4 位址清單中刪除 IPv4 連接埠和位址。

### <a name="syntax"></a>語法

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv4 連接埠。                                    |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定位址，則預設值是本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v6tov4"></a>delete v6tov4

portproxy 伺服器會從伺服器所接聽的 IPv6 位址清單中刪除 IPv6 連接埠和位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv6 連接埠。                                    |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定位址，則預設值是本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v6tov6"></a>delete v6tov6

portproxy 伺服器會從伺服器所接聽的 IPv6 位址清單中刪除 IPv6 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv6 連接埠。                                    |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定位址，則預設值是本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="reset"></a>reset

重設 IPv6 的設定狀態。

### <a name="syntax"></a>語法

`reset`

## <a name="set-v4tov4"></a>set v4tov4

針對使用 **add v4tov4** 命令所建立的 portproxy 伺服器，修改其上現有項目的參數值，或將新項目新增至對應了連接埠/位址配對的清單中。

### <a name="syntax"></a>語法

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv4 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv4 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

針對使用 **add v4tov6** 命令所建立的 portproxy 伺服器，修改其上現有項目的參數值，或將新項目新增至對應了連接埠/位址配對的清單中。

### <a name="syntax"></a>語法

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv4 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv6 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

針對使用 **add v6tov4** 命令所建立的 portproxy 伺服器，修改其上現有項目的參數值，或將新項目新增至對應了連接埠/位址配對的清單中。

### <a name="syntax"></a>語法

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv4 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。 |
|  **connectport**   |       指定要作為連線目標的 IPv4 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

針對使用 **add v6tov6** 命令所建立的 portproxy 伺服器，修改其上現有項目的參數值，或將新項目新增至對應了連接埠/位址配對的清單中。

### <a name="syntax"></a>語法

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            指定要接聽的 IPv6 連接埠 (依連接埠號碼或服務名稱)。                                                            |
| **connectaddress** | 指定要作為連線目標的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定位址，則預設值是本機電腦。  |
|  **connectport**   |        指定要作為連線目標的 IPv6 連接埠 (依連接埠號碼或服務名稱)。 如果未指定 **connectport**，則預設值是本機電腦上的 **listenport** 值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 IP 位址、電腦 NetBIOS 名稱或電腦 DNS 名稱。 如果您未指定位址，則預設值是本機電腦。 |
|    **protocol**    |                                                                                   指定要使用的通訊協定。                                                                                   |

## <a name="show-all"></a>顯示所有

顯示所有 portproxy 參數，包括 v4tov4、v4tov6、v6tov4 和 v6tov6 的連接埠/位址配對。

### <a name="syntax"></a>語法

`show all`


## <a name="show-v4tov4"></a>show v4tov4

顯示 v4tov4 portproxy 參數。

### <a name="syntax"></a>語法

`show v4tov4`

## <a name="show-v4tov6"></a>show v4tov6

顯示 v4tov6 portproxy 參數。

### <a name="syntax"></a>語法

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

顯示 v6tov4 portproxy 參數。

### <a name="syntax"></a>語法

`show v6tov4`


## <a name="show-v6tov6"></a>show v6tov6

顯示 v6tov6 portproxy 參數。

### <a name="syntax"></a>語法

`show v6tov6`

---
