---
title: AutoaddDevices
description: AutoaddDevices 的參考文章，它會顯示在 Windows 部署服務伺服器上自動新增資料庫中的所有電腦。
ms.topic: reference
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f186d36fbdc4ccfac26eae9092c8bb89973d8835
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626346"
---
# <a name="get-autoadddevices"></a>AutoaddDevices

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
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-delete-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
