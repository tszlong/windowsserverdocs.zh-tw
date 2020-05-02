---
title: AllDevices
description: AllDevices 的參考主題，它會顯示所有預先設置電腦的 Windows 部署服務屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26e114be7ecf104687da237636b54b79e4114591
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720896"
---
# <a name="get-alldevices"></a>AllDevices

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="examples"></a>範例
若要查看所有電腦，請輸入下列其中一項：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法按鍵](command-line-syntax-key.md)
子命令：[使用 [取得裝置] 命令](using-the-get-device-command.md)[，透過 [新增裝置] 命令](using-the-add-device-command.md)
[設定裝置](subcommand-set-device.md)

