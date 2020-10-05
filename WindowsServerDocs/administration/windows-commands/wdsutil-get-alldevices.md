---
title: wdsutil get-alldevices
description: Wdsutil alldevices 的參考文章，會顯示所有預先設置電腦的 Windows 部署服務屬性。
ms.topic: reference
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57d565a0b4b79efd3a472505db5f57ecc0ba564f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729929"
---
# <a name="wdsutil-get-alldevices"></a>wdsutil get-alldevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示所有預先設置電腦的 Windows 部署服務屬性。 預先設置的電腦是指已連結至 active directory 網域服務中電腦帳戶的實體電腦。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服務是否應傳回整個樹系或本機網域中的電腦。 預設設定為 [ **否**]，表示只會傳回本機網域中的電腦。|
|[/ReferralServer： <Server name> ]|只傳回針對指定伺服器預先設置的電腦。|
## <a name="examples"></a>範例
若要查看所有電腦，請輸入下列其中一項：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 設定裝置命令](wdsutil-set-device.md)
- [wdsutil 新增裝置命令](wdsutil-add-device.md)
- [wdsutil 取得裝置命令](wdsutil-get-device.md)
