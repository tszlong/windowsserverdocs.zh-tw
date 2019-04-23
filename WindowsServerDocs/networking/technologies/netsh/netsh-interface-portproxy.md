---
title: 介面 portproxy 的 Netsh 命令
description: 使用 netsh 介面 portproxy 命令做為 IPv4 與 IPv6 網路和應用程式之間的 proxy。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 194a418fe6b33e312a3f2529e82d50d76cd15f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842479"
---
# <a name="netsh-interface-portproxy-commands"></a>Netsh 介面 portproxy 命令

使用**netsh 介面 portproxy**命令，以做為 IPv4 與 IPv6 網路和應用程式之間的 proxy。 以下列方式建立 proxy 服務，您可以使用下列命令：

-   IPv4 設定的電腦和應用程式傳送至其他 IPv4 設定的電腦和應用程式的訊息。

-   IPv4 設定的電腦和應用程式訊息傳送至 IPv6 設定的電腦和應用程式。

-   IPv6 設定的電腦和應用程式訊息傳送至 IPv4 設定的電腦和應用程式。

-   IPv6 設定的電腦和應用程式傳送至其他 IPv6 設定的電腦和應用程式的訊息。

在撰寫批次檔或指令碼使用這些命令時，每個命令必須以開頭**netsh 介面 portproxy**。 例如，當使用**刪除 v4tov6**命令，以指定 portproxy 伺服器的 IPv4 連接埠和位址從清單中刪除的 IPv4 位址，伺服器會接聽，批次檔或指令碼必須使用下列語法：

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

可以使用 netsh 介面 portproxy 命令如下：

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

-   [全部顯示](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>新增 v4tov4

Portproxy 伺服器會接聽訊息傳送至特定連接埠和 IPv4 位址和對應的連接埠和 IPv4 位址傳送所建立的個別 TCP 連線後接收的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>參數


| | |
|-----|--------|----------|
| **listenport**     | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，用於接聽。                                                                                                                      | 必要項 |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="add-v4tov6"></a>新增 v4tov6

Portproxy 伺服器會接聽訊息傳送至特定連接埠和 IPv4 位址，而對應的連接埠與 IPv6 位址來傳送訊息接收到之後建立的個別 TCP 連線。

### <a name="syntax"></a>語法

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-----------|-------------|----------|
| **listenport**     | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，用於接聽。       | 必要項 |
| **connectaddress** | 指定要連線的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="add-v6tov4"></a>新增 v6tov4

Portproxy 伺服器會接聽訊息傳送至特定連接埠和 IPv6 位址和連接埠和 IPv4 位址傳送所建立的個別 TCP 連線後接收的訊息目的地的對應。

### <a name="syntax"></a>語法

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|------------|-------------|----------|
| **listenport**     | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，用於接聽。              | 必要項 |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **protocol**       | 指定要使用的通訊協定。      |          |

## <a name="add-v6tov6"></a>新增 v6tov6

Portproxy 伺服器會接聽特定通訊埠和 IPv6 位址，以及對應連接埠和 IPv6 位址來傳送所建立的個別 TCP 連線後接收的訊息傳送的訊息。

### <a name="syntax"></a>語法

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-------------|------------------|----------|
| **listenport**     | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，用於接聽。       | 必要項 |
| **connectaddress** | 指定要連線的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="delete-v4tov4"></a>刪除 v4tov4

Portproxy 伺服器會刪除從清單中的 IPv4 連接埠和位址，伺服器會接聽 IPv4 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要刪除的 IPv4 連接埠。                                                                       | 必要項 |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**      | 指定要使用的通訊協定。                                                                           |          |

## <a name="delete-v4tov6"></a>delete v4tov6

Portproxy 伺服器刪除 IPv4 連接埠和位址，從清單中，伺服器會接聽 IPv4 位址。

### <a name="syntax"></a>語法

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要刪除的 IPv4 連接埠。                                                                       | 必要項 |
| **listenaddress** | 指定要刪除的 IPv4 位址。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**      | 指定要使用的通訊協定。                                                                           |          |

## <a name="delete-v6tov4"></a>刪除 v6tov4

Portproxy 伺服器會刪除從清單中的 IPv6 位址，伺服器會接聽 IPv6 通訊埠和位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要刪除的 IPv6 連接埠。                                                                       | 必要項 |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**      | 指定要使用的通訊協定。                                                                           |          |

## <a name="delete-v6tov6"></a>刪除 v6tov6

Portproxy 伺服器會刪除從清單中的 IPv6 位址，伺服器會接聽 IPv6 位址。

### <a name="syntax"></a>語法

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要刪除的 IPv6 連接埠。                                                                       | 必要項 |
| **listenaddress** | 指定要刪除的 IPv6 位址。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**      | 指定要使用的通訊協定。                                                                           |          |

## <a name="reset"></a>重設

重設的 IPv6 設定的狀態。

### <a name="syntax"></a>語法

`reset`

## <a name="set-v4tov4"></a>設定 v4tov4

修改現有的項目，以建立 portproxy 伺服器上的參數值**新增 v4tov4**命令，或將新的項目加入至清單的對應連接埠/位址配對。

### <a name="syntax"></a>語法

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|--------------------|---------------------------|----------|
| **listenport**     | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，用於接聽。     | 必要項 |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="set-v4tov6"></a>set v4tov6

修改現有的項目，以建立 portproxy 伺服器上的參數值**新增 v4tov6**命令，或將新的項目加入至清單的對應連接埠/位址配對。

### <a name="syntax"></a>語法

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|--------------------|---------------------|----------|
| **listenport**     | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，用於接聽。     | 必要項 |
| **connectaddress** | 指定要連線的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="set-v6tov4"></a>set v6tov4

修改現有的項目，以建立 portproxy 伺服器上的參數值**新增 v6tov4**命令，或將新的項目加入至清單的對應連接埠/位址配對。

### <a name="syntax"></a>語法

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|--------------------|----------------------|----------|
| **listenport**     | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，用於接聽。      | 必要項 |
| **connectaddress** | 指定要連接的 IPv4 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。 |          |
| **connectport**    | 指定的 IPv4 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。              |          |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                    |          |

## <a name="set-v6tov6"></a>set v6tov6

修改現有的項目，以建立 portproxy 伺服器上的參數值**新增 v6tov6**命令，或將新的項目加入至清單的對應連接埠/位址配對。

### <a name="syntax"></a>語法

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>參數

|   |   |
|--------------------|-------------------------|----------|
| **listenport**     | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，用於接聽。   | 必要項 |
| **connectaddress** | 指定要連線的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果未指定的位址，則預設為本機電腦。  |          |
| **connectport**    | 指定的 IPv6 連接埠，連接埠號碼或服務名稱，以用來連接。 如果**connectport**未指定，預設值是值**listenport**本機電腦上。               |          |
| **listenaddress**  | 指定要接聽的 IPv6 位址。 可接受的值是 IP 位址、 電腦的 NetBIOS 名稱或電腦 DNS 名稱。 如果您未指定的位址，則預設為本機電腦。 |          |
| **protocol**       | 指定要使用的通訊協定。                                                                                                                                                                     |          |

## <a name="show-all"></a>顯示所有

顯示所有 portproxy 參數，包括連接埠/位址組 v4tov4、 v4tov6、 v6tov4 和 v6tov6。

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
