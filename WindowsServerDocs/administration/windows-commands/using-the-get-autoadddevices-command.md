---
title: AutoaddDevices
description: AutoaddDevices 的參考文章，它會顯示在 Windows 部署服務伺服器上自動新增資料庫中的所有電腦。
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e9fcb25f465cdb26112a48e25c856a42dd21326
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892462"
---
# <a name="get-autoadddevices"></a>AutoaddDevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示在 Windows 部署服務伺服器上的自動新增資料庫中的所有電腦。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Devicetype： {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|指定要傳回的電腦類型。<p>-   **PendingDevices**會傳回資料庫中狀態為 [擱置] 的所有電腦。<br />-   **RejectedDevices**會傳回資料庫中狀態為 [已拒絕] 的所有電腦。<br />-   **ApprovedDevices**會傳回資料庫中狀態為 [已核准] 的所有電腦。|
## <a name="examples"></a>範例
若要查看所有已核准的電腦，請輸入：
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
若要查看所有拒絕的電腦，請輸入：
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-delete-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
