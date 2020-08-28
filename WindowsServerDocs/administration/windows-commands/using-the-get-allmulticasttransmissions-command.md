---
title: AllMulticastTransmissions
description: AllMulticastTransmissions 的參考文章，可顯示伺服器上所有多播傳輸的相關資訊。
ms.topic: reference
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0c7c4e77c2ad23fab864839cde03ed4f4806c80
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035956"
---
# <a name="get-allmulticasttransmissions"></a>AllMulticastTransmissions

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

|        參數        |                                                                                                                                                                                                                                                                   說明                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： <Server name> ] |                                                                                                                                                                                 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<p>/Show：用戶端-顯示連接至多播傳輸的用戶端電腦的相關資訊。<p>**Windows Server 2008 R2**<p>顯示： {Boot &#124; Install &#124; All}-要傳回的映射類型。                                **開機** 只會傳回開機映射傳輸。                                  **Install** 只會傳回安裝映射傳輸。 **All** 會傳回這兩種影像類型。 |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /details：用戶端     |                                                                                                                                                                                              僅支援 Windows Server 2008 R2。 如果有，則會顯示連線到傳輸的用戶端。                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              從清單中排除任何停用的傳輸。                                                                                                                                                                                                                                               |

## <a name="examples"></a>範例
若要查看所有傳輸的相關資訊，請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Show:All` 若要查看所有傳輸的相關資訊（停用的傳輸除外），請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [使用 MulticastTransmission 命令](using-the-get-multicasttransmission-command.md) 
  [使用 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md) 
  [使用 MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md) 
  [子命令：啟動-MulticastTransmission](subcommand-start-multicasttransmission.md)
