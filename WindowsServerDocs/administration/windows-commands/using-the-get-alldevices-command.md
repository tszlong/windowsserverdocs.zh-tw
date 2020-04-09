---
title: AllDevices
description: AllDevices 的 Windows 命令主題，它會顯示所有預先設置電腦的 Windows 部署服務屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929a74b6cccaed6e85015648538c1ca875b62208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831411"
---
# <a name="get-alldevices"></a>AllDevices

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示所有預先設置電腦的 Windows 部署服務屬性。 預先設置的電腦是指已連結至 active directory 網域服務中電腦帳戶的實體電腦。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應傳回整個樹系或本機網域中的電腦。 預設設定為 [**否**]，表示只會傳回本機網域中的電腦。|
|[/ReferralServer：<Server name>]|只傳回針對指定伺服器預先設置的電腦。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要查看所有電腦，請輸入下列其中一項：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[子命令：](subcommand-set-device.md)使用 [[取得裝置] 命令](using-the-get-device-command.md)
[的 [](using-the-add-device-command.md)裝置
] 設定
