---
title: arp
description: Arp 命令的參考文章，此命令會顯示並修改位址解析通訊協定中的專案， (arp) 快取，用來儲存 IP 位址及其已解析的實體位址。
ms.topic: reference
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 33b636bb27a6dcb9bc5d744e164fb16051cf4f05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633453"
---
# <a name="arp"></a>arp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示和修改位址解析通訊協定中的專案 (ARP) 快取。 ARP 快取包含一或多個資料表，可用來儲存 IP 位址及其解析的乙太網路或權杖通道實體位址。 電腦上安裝的每個乙太網路或權杖環網路介面卡都有個別的資料表。 使用時不含參數， **arp** 會顯示說明資訊。

## <a name="syntax"></a>語法

```
arp [/a [<inetaddr>] [/n <ifaceaddr>]] [/g [<inetaddr>] [-n <ifaceaddr>]] [/d <inetaddr> [<ifaceaddr>]] [/s <inetaddr> <etheraddr> [<ifaceaddr>]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[/a [<inetaddr>] [/n <ifaceaddr>]` | 顯示所有介面目前的 arp 快取資料表。 **/N**參數會區分大小寫。 若要顯示特定 IP 位址的 arp 快取專案，請使用 **arp/a** 搭配 **inetaddr** 參數，其中 **inetaddr** 是 IP 位址。 如果未指定 **inetaddr** ，則會使用第一個適用的介面。 若要顯示特定介面的 arp 快取表格，請使用 **/n ifaceaddr** 參數搭配 **/a** 參數，其中 **inetaddr** 是指派給介面的 IP 位址。 |
| `[/g [<inetaddr>] [/n <ifaceaddr>]` | 與 **/a**相同。 |
| `[/d <inetaddr> [<ifaceaddr>]` | 刪除具有特定 IP 位址的專案，其中 **inetaddr** 是 ip 位址。 若要在資料表中刪除特定介面的專案，請使用 **ifaceaddr** 參數，其中 **ifaceaddr** 是指派給介面的 IP 位址。 若要刪除所有專案，請使用星號 ( * ) 萬用字元來取代 **inetaddr**。 |
| `[/s <inetaddr> <etheraddr> [<ifaceaddr>]` | 將靜態專案新增至 arp 快取，以將 IP 位址 **inetaddr** 解析為實體位址 **etheraddr**。 若要為特定介面將靜態 arp 快取專案新增至資料表，請使用 **ifaceaddr** 參數，其中 **ifaceaddr** 是指派給介面的 IP 位址。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- **Inetaddr**和**ifaceaddr**的 IP 位址是以點分隔的十進位標記法來表示。

- **Etheraddr**的實體位址包含以十六進位標記法表示的六個位元組，並以連字號分隔 (例如 00-AA-00-4F-2A-9C) 。

- 使用 **/s** 參數新增的專案是靜態的，而且不會超出 arp 快取的時間。 如果 TCP/IP 通訊協定已停止並啟動，就會移除這些專案。 若要建立永久性靜態 arp 快取專案，請在批次檔中放置適當的 **arp** 命令，並使用排程的工作在啟動時執行批次檔。

## <a name="examples"></a>範例

若要顯示所有介面的 arp 快取表格，請輸入：

```
arp /a
```

若要針對指派給 IP 位址 *10.0.0.99*的介面顯示 arp 快取表格，請輸入：

```
arp /a /n 10.0.0.99
```

若要將解析 IP 位址 *10.0.0.80* 的靜態 arp 快取專案新增至實體位址 *00-AA-00-4F-2a-9C*，請輸入：

```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
