---
title: 使用取得裝置命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6451e0a55a72fc88867a3f3be0e1317d881391aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363202"
---
# <a name="using-the-get-device-command"></a>使用取得裝置命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取預先設置電腦的 Windows 部署服務資訊（也就是在 active directory 網域服務中已加入電腦帳戶的實體電腦。
## <a name="syntax"></a>語法
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/Device： <Device name>|指定電腦的名稱（SAMAccountName）。|
|/ID： <MAC or UUID>|指定電腦的 MAC 位址或 UUID （GUID），如下列範例所示。 請注意，有效的 GUID 必須是兩種格式的其中一種：二進位字串或 GUID 字串<br /><br />-   **二進位字串**：/ID： ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **MAC 位址**：00B056882FDC （無連字號）或 00-B0-56-88-2F-DC （含破折號）<br />-   **GUID 字串**：/ID： E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain： <Domain>]|指定要搜尋預先設置電腦的網域。 這個參數的預設值是本機網域。|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應搜尋整個樹系或本機網域。 預設值為 [**否**]，表示只會搜尋本機網域。|
## <a name="BKMK_examples"></a>典型
若要使用電腦名稱稱取得資訊，請輸入：
```
wdsutil /Get-Device /Device:computer1
```
若要使用 MAC 位址取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
若要使用 GUID 字串取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[子命令：](subcommand-set-device.md)使用[AllDevices 命令](using-the-get-alldevices-command.md)
[，](using-the-add-device-command.md)將裝置 

