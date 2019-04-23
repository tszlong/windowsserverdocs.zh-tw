---
title: 使用 get AllDevices 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886369"
---
# <a name="using-the-get-alldevices-command"></a>使用 get AllDevices 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示所有預先設置電腦的 Windows 部署服務屬性。 預先設置的電腦是已連結至 active directory 網域服務中的電腦帳戶的實體電腦。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/forest:{Yes &#124; No}]|指定 Windows 部署服務是否應傳回在整個樹系或本機網域中的電腦。 預設值是**No**，只有在本機網域的電腦所傳回的意義。|
|[/ReferralServer:<Server name>]|傳回只會針對指定的伺服器預先設置的電腦。|
## <a name="BKMK_examples"></a>範例
若要檢視所有電腦，請輸入下列其中一項：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[子命令： 設定裝置](subcommand-set-device.md)
[使用 新增裝置命令](using-the-add-device-command.md)
[使用 get-裝置命令](using-the-get-device-command.md)
