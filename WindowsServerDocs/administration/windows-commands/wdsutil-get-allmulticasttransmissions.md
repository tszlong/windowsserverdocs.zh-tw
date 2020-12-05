---
title: wdsutil get-allmulticasttransmissions
description: Wdsutil allmulticasttransmissions 的參考文章，會顯示伺服器上所有多播傳輸的相關資訊。
ms.topic: reference
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 39405952aac938a7d02ff0f4d1d0c3593df88326
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614850"
---
# <a name="wdsutil-get-allmulticasttransmissions"></a>wdsutil get-allmulticasttransmissions

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有多播傳輸的相關資訊。

## <a name="syntax"></a>語法
若為 Windows Server 2008：
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
若為 Windows Server 2008 R2：
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [/Server： <Server name> ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| /Show | **Windows Server 2008**<p>/Show：用戶端-顯示連接至多播傳輸的用戶端電腦的相關資訊。<p>**Windows Server 2008 R2**<p>顯示： {Boot &#124; Install &#124; All}-要傳回的映射類型。                                **開機** 只會傳回開機映射傳輸。                                  **Install** 只會傳回安裝映射傳輸。 **All** 會傳回這兩種影像類型。 |
|  |  |
| /details：用戶端 | 僅支援 Windows Server 2008 R2。 如果有，則會顯示連線到傳輸的用戶端。 |
| [/ExcludedeletePending] | 從清單中排除任何停用的傳輸。 |

## <a name="examples"></a>範例
若要查看所有傳輸的相關資訊，請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Show:All` 若要查看所有傳輸的相關資訊（停用的傳輸除外），請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-multicasttransmission 命令](wdsutil-get-multicasttransmission.md)
- [wdsutil new-multicasttransmission 命令](wdsutil-new-multicasttransmission.md)
- [wdsutil remove-multicasttransmission 命令](wdsutil-remove-multicasttransmission.md)
- [wdsutil 開始-multicasttransmission 命令](wdsutil-start-multicasttransmission.md)
