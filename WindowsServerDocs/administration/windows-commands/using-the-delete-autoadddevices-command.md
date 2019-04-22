---
title: 使用 delete AutoaddDevices 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813499"
---
# <a name="using-the-delete-autoadddevices-command"></a>使用 delete AutoaddDevices 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

刪除遭到拒絕，或從自動新增資料庫核准擱置中的電腦。 這個資料庫儲存在伺服器上的這些電腦的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|指定要從資料庫刪除電腦的類型。 這可以是下列三種類型的任何一項：<br /><br />-   **PendingDevices**傳回的狀態為暫止的資料庫中的所有電腦。<br />-   **RejectedDevices**程式具有的狀態為已拒絕資料庫中傳回的所有電腦。<br />-   **ApprovedDevices**傳回之狀態的所有電腦核准。|
## <a name="BKMK_examples"></a>範例
若要刪除所有已拒絕的電腦，請輸入：
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
若要刪除所有已核准的電腦，請輸入：
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用核准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
[使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
 [使用拒絕 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
