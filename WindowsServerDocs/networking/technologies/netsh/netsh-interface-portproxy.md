---
title: 介面 portproxy 的 Netsh 命令
description: 使用 netsh interface portproxy 命令，作為 IPv4 與 IPv6 網路和應用程式之間的 proxy。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 6244213ea689f07230ce53288e52959972112037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405509"
---
# <a name="netsh-interface-portproxy-commands"></a>Netsh 介面 portproxy 命令

使用**netsh interface portproxy**命令，作為 IPv4 與 IPv6 網路和應用程式之間的 proxy。 您可以透過下列方式使用這些命令來建立 proxy 服務：

-   IPv4 設定的電腦和應用程式訊息，會傳送到其他 IPv4 設定的電腦和應用程式。

-   IPv4 設定的電腦和應用程式訊息，會傳送至 IPv6 設定的電腦和應用程式。

-   IPv6 設定的電腦和應用程式訊息，會傳送到 IPv4 設定的電腦和應用程式。

-   已將 IPv6 設定的電腦和應用程式訊息傳送至其他 IPv6 設定的電腦和應用程式。

使用這些命令來撰寫批次檔或腳本時，每個命令的開頭都必須是**netsh interface portproxy**。 例如，當使用**delete v4tov6**命令來指定 portproxy 伺服器從伺服器接聽的 ipv4 地址清單中刪除 ipv4 埠和位址時，批次檔或腳本必須使用下列語法：

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

可用的 netsh interface portproxy 命令如下：

-   [新增 v4tov4](#add-v4tov4)

-   [新增 v4tov6](#add-v4tov6)

-   [新增 v6tov4](#add-v6tov4)

-   [新增 v6tov6](#add-v6tov6)

-   [刪除 v4tov4](#delete-v4tov4)

-   [刪除 v4tov6](#delete-v4tov6)

-   [刪除 v6tov6](#delete-v6tov6)

-   [啟動](#reset)

-   [設定 v4tov4](#set-v4tov4)

-   [設定 v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [設定 v6tov6](#set-v6tov6)

-   [全部顯示](#show-all)

-   [顯示 v4tov4](#show-v4tov4)

-   [顯示 v4tov6](#show-v4tov6)

-   [顯示 v6tov4](#show-v6tov4)

-   [顯示 v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>新增 v4tov4

Portproxy 伺服器會接聽傳送至特定埠和 IPv4 位址的訊息，並對應埠和 IPv4 位址，以傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>參數


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           依埠號碼或服務名稱指定要接聽的 IPv4 通訊埠。                                                            |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv4 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v4tov6"></a>新增 v4tov6

Portproxy 伺服器會接聽傳送至特定埠和 IPv4 位址的訊息，並對應埠和 IPv6 位址，以傳送在建立個別 TCP 連線之後所收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           依埠號碼或服務名稱指定要接聽的 IPv4 通訊埠。                                                            |
| **connectaddress** | 指定要連接的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv6 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v6tov4"></a>新增 v6tov4

Portproxy 伺服器會接聽傳送至特定埠和 IPv6 位址的訊息，並對應埠和 IPv4 位址，以便在建立個別的 TCP 連線之後傳送接收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 埠（依埠號碼或服務名稱）。                                                            |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv4 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="add-v6tov6"></a>新增 v6tov6

Portproxy 伺服器會接聽傳送至特定埠和 IPv6 位址的訊息，並對應埠和 IPv6 位址，以在建立個別的 TCP 連線之後傳送接收到的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 埠（依埠號碼或服務名稱）。                                                            |
| **connectaddress** | 指定要連接的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv6 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="delete-v4tov4"></a>刪除 v4tov4

Portproxy 伺服器會從伺服器接聽的 IPv4 埠和地址清單中刪除 IPv4 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv4 通訊埠。                                    |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定位址，預設值為本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v4tov6"></a>刪除 v4tov6

Portproxy 伺服器會從伺服器接聽的 IPv4 地址清單中刪除 IPv4 埠和位址。

### <a name="syntax"></a>語法

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv4 通訊埠。                                    |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定位址，預設值為本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v6tov4"></a>刪除 v6tov4

Portproxy 伺服器會從伺服器接聽的 IPv6 地址清單中，刪除 IPv6 埠和位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv6 埠。                                    |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定位址，預設值為本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="delete-v6tov6"></a>刪除 v6tov6

Portproxy 伺服器會從伺服器接聽的 IPv6 地址清單中刪除 IPv6 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    指定要刪除的 IPv6 埠。                                    |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定位址，預設值為本機電腦。 |
|   **protocol**    |                                      指定要使用的通訊協定。                                      |

## <a name="reset"></a>啟動

重設 IPv6 設定狀態。

### <a name="syntax"></a>語法

`reset`

## <a name="set-v4tov4"></a>設定 v4tov4

修改使用**add v4tov4**命令所建立之 portproxy 伺服器上現有專案的參數值，或將新專案新增至對應埠/位址配對的清單。

### <a name="syntax"></a>語法

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           依埠號碼或服務名稱指定要接聽的 IPv4 通訊埠。                                                            |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv4 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v4tov6"></a>設定 v4tov6

修改使用**add v4tov6**命令所建立之 portproxy 伺服器上現有專案的參數值，或將新專案新增至對應埠/位址配對的清單。

### <a name="syntax"></a>語法

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           依埠號碼或服務名稱指定要接聽的 IPv4 通訊埠。                                                            |
| **connectaddress** | 指定要連接的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv6 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v6tov4"></a>設定 v6tov4

修改使用**add v6tov4**命令所建立之 portproxy 伺服器上現有專案的參數值，或將新專案新增至對應埠/位址配對的清單。

### <a name="syntax"></a>語法

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           指定要接聽的 IPv6 埠（依埠號碼或服務名稱）。                                                            |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。 |
|  **connectport**   |       指定要連接的 IPv4 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|    **protocol**    |                                                                                  指定要使用的通訊協定。                                                                                   |

## <a name="set-v6tov6"></a>設定 v6tov6

修改使用**add v6tov6**命令所建立之 portproxy 伺服器上現有專案的參數值，或將新專案新增至對應埠/位址配對的清單。

### <a name="syntax"></a>語法

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            指定要接聽的 IPv6 埠（依埠號碼或服務名稱）。                                                            |
| **connectaddress** | 指定要連接的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果未指定位址，預設值為本機電腦。  |
|  **connectport**   |        指定要連接的 IPv6 埠（依埠號碼或服務名稱）。 如果未指定**connectport** ，則預設值為本機電腦上**listenport**的值。        |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值為 [IP 位址]、[電腦 NetBIOS 名稱] 或 [電腦 DNS 名稱]。 如果您未指定位址，預設值為本機電腦。 |
|    **protocol**    |                                                                                   指定要使用的通訊協定。                                                                                   |

## <a name="show-all"></a>顯示所有

顯示所有 portproxy 參數，包括 v4tov4、v4tov6、v6tov4 和 v6tov6 的埠/位址配對。

### <a name="syntax"></a>語法

`show all`


## <a name="show-v4tov4"></a>顯示 v4tov4

顯示 v4tov4 portproxy 參數。

### <a name="syntax"></a>語法

`show v4tov4`

## <a name="show-v4tov6"></a>顯示 v4tov6

顯示 v4tov6 portproxy 參數。

### <a name="syntax"></a>語法

`show v4tov6`

## <a name="show-v6tov4"></a>顯示 v6tov4

顯示 v6tov4 portproxy 參數。

### <a name="syntax"></a>語法

`show v6tov4`


## <a name="show-v6tov6"></a>顯示 v6tov6

顯示 v6tov6 portproxy 參數。

### <a name="syntax"></a>語法

`show v6tov6`

---
