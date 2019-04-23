---
title: 使用 get 裝置命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871029"
---
# <a name="using-the-get-device-command"></a>使用 get 裝置命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取 Windows 部署服務資訊的預先設置的電腦 （也就是實體電腦具有已內嵌至 active directory 網域服務中的電腦帳戶。
## <a name="syntax"></a>語法
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ 裝置：<Device name>|指定電腦 (SAMAccountName) 的名稱。|
|/ID:<MAC or UUID>|指定的 MAC 位址或電腦的 UUID (GUID)，如下列範例所示。 請注意，在兩種格式的二進位字串或 GUID 字串，其中之一必須是有效的 GUID<br /><br />-   **二進位字串**: /ID:ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **MAC 位址**:00B056882FDC （沒有連字號） 或 00-B0-56-88-2F-DC （含連接號）<br />-   **GUID 字串**: /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ 網域：<Domain>]|指定要搜尋預先設置的電腦的網域。 此參數的預設值是本機網域。|
|[/forest:{Yes &#124; No}]|指定 Windows 部署服務是否應該搜尋整個樹系或本機網域。 預設值是**No**，這表示將會搜尋本機網域。|
## <a name="BKMK_examples"></a>範例
若要使用的電腦名稱，以取得資訊，請輸入：
```
wdsutil /Get-Device /Device:computer1
```
若要使用的 MAC 位址，以取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
若要使用的 GUID 字串，以取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[子命令： 設定裝置](subcommand-set-device.md)
[使用 新增裝置命令](using-the-add-device-command.md)
[使用取得 AllDevices 命令](using-the-get-alldevices-command.md)
