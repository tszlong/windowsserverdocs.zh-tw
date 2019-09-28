---
title: 使用 AllDevices 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b7d2ce709c7e3fbaf7ab4f0e49be14c98ba1cd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401979"
---
# <a name="using-the-get-alldevices-command"></a>使用 AllDevices 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示所有預先設置電腦的 Windows 部署服務屬性。 預先設置的電腦是指已連結至 active directory 網域服務中電腦帳戶的實體電腦。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應傳回整個樹系或本機網域中的電腦。 預設設定為 [**否**]，表示只會傳回本機網域中的電腦。|
|[/ReferralServer： <Server name>]|只傳回針對指定伺服器預先設置的電腦。|
## <a name="BKMK_examples"></a>典型
若要查看所有電腦，請輸入下列其中一項：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[子命令：](subcommand-set-device.md)使用 [[取得裝置] 命令](using-the-get-device-command.md)
[，](using-the-add-device-command.md)設定裝置 

