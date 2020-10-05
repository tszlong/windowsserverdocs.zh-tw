---
title: wdsutil get-autoadddevices
description: Wdsutil autoadddevices 的參考文章，它會顯示在 Windows 部署服務伺服器上自動新增資料庫中的所有電腦。
ms.topic: reference
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c7d5be85ce29a41714d70d8bdfa11e3afa1661d6
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729909"
---
# <a name="wdsutil-get-autoadddevices"></a>wdsutil get-autoadddevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示 Windows 部署服務伺服器上自動新增資料庫中的所有電腦。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Devicetype： {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|指定要傳回的電腦類型。<p>-   **PendingDevices** 會傳回資料庫中狀態為 [擱置] 的所有電腦。<br />-   **RejectedDevices** 會傳回資料庫中狀態為 [已拒絕] 的所有電腦。<br />-   **ApprovedDevices** 會傳回資料庫中狀態為 [已核准] 的所有電腦。|
## <a name="examples"></a>範例
若要查看所有核准的電腦，請輸入：
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
若要查看所有已拒絕的電腦，請輸入：
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil delete-autoadddevices 命令](wdsutil-delete-autoadddevices.md)
- [wdsutil 核准-autoadddevices 命令](wdsutil-approve-autoadddevices.md)
- [wdsutil 拒絕-autoadddevices 命令](wdsutil-reject-autoadddevices.md)
