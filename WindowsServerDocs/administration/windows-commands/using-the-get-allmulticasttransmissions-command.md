---
title: 使用 get AllMulticastTransmissions 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf4c3449a5c3194ec27efc2ee4adaccb54f9f7e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889149"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>使用 get AllMulticastTransmissions 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示伺服器上的所有多點傳送傳輸的資訊。
## <a name="syntax"></a>語法
適用於 Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
適用於 Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|[/Show]|**Windows Server 2008**<br /><br />/Show: clients-會顯示用戶端電腦連線到多點傳送傳輸的相關資訊。<br /><br />**Windows Server 2008 R2**<br /><br />顯示: {開機&#124;安裝&#124;所有}-要傳回的影像類型。                                **開機**傳回只有開機映像傳輸。                                  **安裝**傳回只安裝映像傳輸。 **所有**傳回兩者映像類型。|
|||
|/details:clients|僅支援 Windows Server 2008 R2。 如果有的話，會顯示連線到傳輸的用戶端。|
|[/ExcludedeletePending]|排除清單中的任何已停用的傳輸。|
## <a name="BKMK_examples"></a>範例
若要檢視所有傳輸的資訊，請輸入：
-   Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
-   Windows Server 2008 R2：`wdsutil /Get-AllMulticastTransmissions /Show:All` 若要檢視已停用傳輸除外的所有傳輸的資訊，請輸入：
-   Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
-   Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用 新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
