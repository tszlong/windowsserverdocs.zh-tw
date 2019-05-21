---
title: arp
description: 適用於 Windows 命令主題**arp** -顯示，並修改用來儲存 IP 位址和其已解析的實體位址的位址解析通訊協定 (arp) 快取中的項目。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5cd84269a5ac1a85d4b6cf359cc97f478a500c4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825979"
---
# <a name="arp"></a>arp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示及修改位址解析通訊協定 (ARP) 快取中的項目。 ARP 快取包含用來儲存 IP 位址和其解決的乙太網路或權杖環實體位址的一個或多個資料表。 沒有個別的資料表，每個乙太網路或權杖環網路介面卡安裝在您的電腦上。 不含參數， **arp**顯示說明資訊。
## <a name="syntax"></a>語法
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/a [<Inetaddr>] [/n <ifaceaddr>]|顯示目前的 arp 快取表格，為所有介面。 /N 參數會區分大小寫。<br /><br />若要顯示特定的 IP 位址的 arp 快取項目，請使用**arp/a**具有*Inetaddr*參數，其中*Inetaddr*是 IP 位址。 如果*Inetaddr*未指定，會使用第一個適當的介面。<br /><br />若要顯示特定介面的 arp 快取表格，請使用 **/n***ifaceaddr* 參數搭配 **/a**參數位置*ifaceaddr*是 IP 位址指派給介面。|
|/g [<Inetaddr>] [/n <ifaceaddr>]|與相同 **/a**。|
|[/d <Inetaddr> [<ifaceaddr>]|刪除項目特定的 IP 位址，其中*Inetaddr*是 IP 位址。<br /><br />若要刪除特定介面的資料表中的項目，請使用*ifaceaddr*參數所在*ifaceaddr*是指派給介面的 IP 位址。<br /><br />若要刪除所有項目，使用星號 (\*) 取代萬用字元*Inetaddr*。|
|/s <Inetaddr> <Etheraddr> [<ifaceaddr>]|將靜態項目加入至 arp 快取解析的 IP 位址*Inetaddr*的實體位址*Etheraddr*。<br /><br />若要新增靜態 arp 快取項目至特定介面的資料表，請使用*ifaceaddr*參數所在*ifaceaddr*是指派給介面的 IP 位址。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   IP 位址*Inetaddr*並*ifaceaddr*會以小數點十進位表示法。
-   實體位址*Etheraddr*十六進位表示法中表示和連字號 (例如 00-AA-00-4F-2A-9C) 分隔的六個位元組所組成。
-   使用新增的項目 **/s**參數是靜態的而且不執行時間超出 arp 快取。 如果停止並啟動 TCP/IP 通訊協定，則會移除的項目。 若要建立永久靜態 arp 快取項目，請將適當**arp**批次中的命令檔案，並在啟動時執行的批次檔使用排定的工作。
## <a name="BKMK_Examples"></a>範例
若要顯示所有介面的 arp 快取表格，請輸入：
```
arp /a
```
若要顯示的介面，指派的 IP 位址 10.0.0.99 arp 快取表格，請輸入：
```
arp /a /n 10.0.0.99
```
若要新增靜態 arp 快取項目解析的 IP 位址 10.0.0.80 00-AA-00-4F-2A-9C 的實體位址，請輸入：
```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
