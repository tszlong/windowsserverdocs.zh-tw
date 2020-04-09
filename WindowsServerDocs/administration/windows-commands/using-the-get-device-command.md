---
title: 取得裝置
description: 適用于「取得裝置」的 Windows 命令主題，它會抓取預先設置電腦的 Windows 部署服務資訊（也就是在 active directory 網域服務中已加入電腦帳戶的實體電腦）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b9554ed6236d02be0be3502f42552bafbbfe1cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831111"
---
# <a name="get-device"></a>取得裝置

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取預先設置電腦的 Windows 部署服務資訊（也就是在 active directory 網域服務中已加入電腦帳戶的實體電腦。

## <a name="syntax"></a>語法
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/Device：<Device name>|指定電腦的名稱（SAMAccountName）。|
|/ID：<MAC or UUID>|指定電腦的 MAC 位址或 UUID （GUID），如下列範例所示。 請注意，有效的 GUID 必須是兩種格式的其中一種：二進位字串或 GUID 字串<p>-   **二進位字串**：/ID： ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **MAC 位址**：00B056882FDC （無連字號）或 00-B0-56-88-2F-DC （含破折號）<br />-   **GUID 字串**：/ID： E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain：<Domain>]|指定要搜尋預先設置電腦的網域。 這個參數的預設值是本機網域。|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應搜尋整個樹系或本機網域。 預設值為 [**否**]，表示只會搜尋本機網域。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
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
- [命令列語法的索引鍵](command-line-syntax-key.md)
子命令：使用[AllDevices 命令](using-the-get-alldevices-command.md)
[使用 [新增裝置] 命令來](using-the-add-device-command.md)[設定裝置](subcommand-set-device.md)

