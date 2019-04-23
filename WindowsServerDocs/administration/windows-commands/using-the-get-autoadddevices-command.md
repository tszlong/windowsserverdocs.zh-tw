---
title: 使用 get AutoaddDevices 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 337c8e76923fe243982ba9c10d18f2e5a5e7d9ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885739"
---
# <a name="using-the-get-autoadddevices-command"></a>使用 get AutoaddDevices 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示在 Windows 部署服務伺服器上自動新增資料庫中的所有電腦。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|指定要傳回的電腦類型。<br /><br />-   **PendingDevices**傳回的狀態為暫止的資料庫中的所有電腦。<br />-   **RejectedDevices**程式具有的狀態為已拒絕資料庫中傳回的所有電腦。<br />-   **ApprovedDevices**有狀態為核准的資料庫中傳回的所有電腦。|
## <a name="BKMK_examples"></a>範例
若要查看所有已核准的電腦，請輸入：
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
若要查看所有已拒絕的電腦，請輸入：
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
[使用核准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
 [使用拒絕 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
