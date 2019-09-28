---
title: 使用 AutoaddDevices 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e914506f731685b17117b359359f60ea91992dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363542"
---
# <a name="using-the-delete-autoadddevices-command"></a>使用 AutoaddDevices 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從自動新增資料庫中刪除擱置、拒絕或核准的電腦。 此資料庫會將這些電腦的相關資訊儲存在伺服器上。
## <a name="syntax"></a>語法
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Devicetype： {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|指定要從資料庫刪除的電腦類型。 這可以是下列三種類型的其中之一：<br /><br />-   **PendingDevices**會傳回資料庫中狀態為 [擱置] 的所有電腦。<br />-   **RejectedDevices**會傳回資料庫中狀態為 [已拒絕] 的所有電腦。<br />-   **ApprovedDevices**會傳回狀態為 [已核准] 的所有電腦。|
## <a name="BKMK_examples"></a>典型
若要刪除所有拒絕的電腦，請輸入：
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
若要刪除所有核准的電腦，請輸入：
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
 使用[AutoaddDevices 命令](using-the-get-autoadddevices-command.md)，
 使用[AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
