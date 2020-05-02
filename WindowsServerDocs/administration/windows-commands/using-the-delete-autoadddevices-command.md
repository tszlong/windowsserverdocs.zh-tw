---
title: 刪除-AutoaddDevices
description: AutoaddDevices 的參考主題，可刪除自動新增資料庫中擱置、拒絕或核准的電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b5b24b68b2cfe3d387cb02b3715b70edba4300
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720987"
---
# <a name="delete-autoadddevices"></a>刪除-AutoaddDevices

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從自動新增資料庫中刪除擱置、拒絕或核准的電腦。 此資料庫會將這些電腦的相關資訊儲存在伺服器上。

## <a name="syntax"></a>語法
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Devicetype： {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|指定要從資料庫刪除的電腦類型。 這可以是下列三種類型的其中之一：<p>-   **PendingDevices**會傳回資料庫中狀態為 [擱置] 的所有電腦。<br />-   **RejectedDevices**會傳回資料庫中狀態為 [已拒絕] 的所有電腦。<br />-   **ApprovedDevices**會傳回狀態為 [已核准] 的所有電腦。|
## <a name="examples"></a>範例
若要刪除所有拒絕的電腦，請輸入：
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
若要刪除所有核准的電腦，請輸入：
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
的命令列語法索引鍵使用[AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)的[AutoaddDevices 命令](using-the-get-autoadddevices-command.md)

