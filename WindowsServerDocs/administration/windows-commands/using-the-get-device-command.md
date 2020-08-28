---
title: 取得裝置
description: 取得裝置的參考文章，以抓取預先設置電腦的 Windows 部署服務資訊， (也就是已在 active directory 網域服務中的電腦帳戶的實體電腦。
ms.topic: reference
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 118e14f738920527c61c8da25d19ab98126fc004
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029696"
---
# <a name="get-device"></a>取得裝置

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取有關預先設置電腦的 Windows 部署服務資訊， (也就是已在 active directory 網域服務中加入電腦帳戶的實體電腦。

## <a name="syntax"></a>語法
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|裝置<Device name>|指定 (SAMAccountName) 的電腦名稱稱。|
|識別碼<MAC or UUID>|指定電腦的 MAC 位址或 UUID (GUID) ，如下列範例所示。 請注意，有效的 GUID 必須是下列其中一種格式：二進位字串或 GUID 字串<p>-   **二進位字串**：/ID： ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **MAC 位址**： 00B056882FDC (沒有連字號) 或 00-B0-56-88-2F-DC (加上連字號) <br />-   **GUID 字串**：/ID： E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain： <Domain> ]|指定要搜尋預先設置電腦的網域。 這個參數的預設值是本機網域。|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應該搜尋整個樹系或本機網域。 預設值為 [ **否**]，表示只會搜尋本機網域。|
## <a name="examples"></a>範例
若要使用電腦名稱稱取得資訊，請輸入：
```
wdsutil /Get-Device /Device:computer1
```
若要使用 MAC 位址取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:00-B0-56-88-2F-DC /Domain:MyDomain
```
若要使用 GUID 字串取得資訊，請輸入：
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[子命令：設定裝置](subcommand-set-device.md) 
[使用 [新增裝置] 命令](using-the-add-device-command.md) 
[使用 AllDevices 命令](using-the-get-alldevices-command.md)
