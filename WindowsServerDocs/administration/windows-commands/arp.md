---
title: arp
description: '**Arp**的 Windows 命令主題-顯示及修改位址解析通訊協定（arp）快取中用來儲存 IP 位址及其已解析之實體位址的專案。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e6d34ceaa56ed40a1083b710e0db01b106f49e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382658"
---
# <a name="arp"></a>arp

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示和修改位址解析通訊協定（ARP）快取中的專案。 ARP 快取包含一或多個用來儲存 IP 位址及其已解析的乙太網路或權杖環形實體位址的資料表。 您的電腦上安裝的每個 Ethernet 或權杖環形網路介面卡都有個別的資料表。 使用時不含參數， **arp**會顯示說明資訊。
## <a name="syntax"></a>語法
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>參數

|                參數                |                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                               |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /a [<Inetaddr>] [/n <ifaceaddr>]     | 顯示所有介面的目前 arp 快取資料表。 /N 參數會區分大小寫。<br /><br />若要顯示特定 IP 位址的 arp 快取專案，請搭配*Inetaddr*參數使用**arp/A** ，其中*Inetaddr*是 IP 位址。 如果未指定*Inetaddr* ，則會使用第一個適用的介面。<br /><br />若要顯示特定介面的 arp 快取資料表，請使用 **/n**_ifaceaddr_參數搭配 **/a**參數，其中*ifaceaddr*是指派給介面的 IP 位址。 |
|    /g [<Inetaddr>] [/n <ifaceaddr>]     |                                                                                                                                                                                                                                                          與 **/a**相同。                                                                                                                                                                                                                                                           |
|      [/d <Inetaddr> [<ifaceaddr>]       |                                                                                           刪除具有特定 IP 位址的專案，其中*Inetaddr*是 ip 位址。<br /><br />若要刪除特定介面之資料表中的專案，請使用*ifaceaddr*參數，其中*ifaceaddr*是指派給介面的 IP 位址。<br /><br />若要刪除所有專案，請使用星號（\*）萬用字元來取代*Inetaddr*。                                                                                           |
| /s <Inetaddr> <Etheraddr> [<ifaceaddr>] |                                                                                                                     將靜態專案新增至 arp 快取，以將 IP 位址*Inetaddr*解析為實體位址*Etheraddr*。<br /><br />若要將靜態 arp 快取專案新增至特定介面的資料表，請使用*ifaceaddr*參數，其中*ifaceaddr*是指派給介面的 IP 位址。                                                                                                                     |
|                   /?                    |                                                                                                                                                                                                                                                  在命令提示字元顯示說明。                                                                                                                                                                                                                                                   |

## <a name="remarks"></a>備註
- *Inetaddr*和*ifaceaddr*的 IP 位址會以點十進位標記法表示。
- *Etheraddr*的實體位址包含以十六進位標記法表示的六個位元組，並以連字號分隔（例如，00-AA-00-4F-2A-9C）。
- 使用 **/s**參數新增的專案是靜態的，而且不會超出 arp 快取的時間。 如果 TCP/IP 通訊協定已停止並啟動，則會移除專案。 若要建立永久的靜態 arp 快取專案，請將適當的**arp**命令放在批次檔中，並在啟動時使用排定的工作來執行批次檔。
  ## <a name="BKMK_Examples"></a>典型
  若要顯示所有介面的 arp 快取資料表，請輸入：
  ```
  arp /a
  ```
  若要顯示已指派 IP 位址為10.0.0.99 之介面的 arp 快取表格，請輸入：
  ```
  arp /a /n 10.0.0.99
  ```
  若要新增可將 IP 位址10.0.0.80 解析為實體位址 00-AA-00-4F-2A-9C 的靜態 arp 快取專案，請輸入：
  ```
  arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
  ```
  ## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
