---
title: AllMulticastTransmissions
description: AllMulticastTransmissions 的 Windows 命令主題，它會顯示伺服器上所有多播傳輸的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c81db5a5d5ebb9bcc5e2d00165d5c944c687fd97
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831325"
---
# <a name="get-allmulticasttransmissions"></a>AllMulticastTransmissions

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
| [/Server：<Server name>] |                                                                                                                                                                                 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<p>/Show： Clients-顯示連線到多播傳輸之用戶端電腦的相關資訊。<p>**Windows Server 2008 R2**<p>Show： {Boot &#124; Install &#124; All}-要傳回的映射類型。                                **開機**只會傳回開機映射傳輸。                                  **安裝**只會傳回安裝映射傳輸。 **All**會傳回這兩個影像類型。 |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /details：用戶端     |                                                                                                                                                                                              僅支援 Windows Server 2008 R2。 如果存在，則會顯示連線到傳輸的用戶端。                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              排除清單中任何已停用的傳輸。                                                                                                                                                                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要查看所有傳輸的相關資訊，請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Show:All` 若要查看停用的傳輸以外所有傳輸的相關資訊，請輸入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [使用 MulticastTransmission 命令](using-the-get-multicasttransmission-command.md)
  使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)，
  [使用 MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
  [子命令： start-MulticastTransmission](subcommand-start-multicasttransmission.md)
