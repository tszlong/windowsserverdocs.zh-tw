---
title: 使用拒絕 AutoaddDevices 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852559"
---
# <a name="using-the-reject-autoadddevices-command"></a>使用拒絕 AutoaddDevices 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

拒絕擱置中的系統管理核准的電腦。 啟用自動新增原則時，系統管理員核准後未知的電腦 （其尚未預先設置） 可以安裝映像。 您可以使用此原則啟用**PXE 回應** 索引標籤的 伺服器屬性 頁面。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/RequestId:<Request ID &#124; ALL>|指定擱置電腦的要求 ID。 若要拒絕所有擱置電腦，指定**所有**。|
## <a name="BKMK_examples"></a>範例
若要拒絕單一電腦，請輸入：
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
若要拒絕所有電腦，請輸入：
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用核准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
 [使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
