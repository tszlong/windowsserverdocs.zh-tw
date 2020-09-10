---
title: 拒絕-AutoaddDevices
description: 拒絕 AutoaddDevices 的參考文章，會拒絕正在等待系統管理核准的電腦。
ms.topic: reference
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 554f3da87ddd3f99284c79614fdde516d171d16d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627909"
---
# <a name="reject-autoadddevices"></a>拒絕-AutoaddDevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

拒絕正在等待系統管理核准的電腦。 啟用自動新增原則時，在未知電腦之前需要系統管理核准 (未預先設置) 可以安裝映射。 您可以使用 [伺服器屬性] 頁面的 [ **PXE 回應** ] 索引標籤來啟用此原則。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/RequestId： &#124; 所有> 的 <要求識別碼|指定指派給擱置電腦的要求識別碼。 若要拒絕所有擱置的電腦，請指定 [ **全部**]。|
## <a name="examples"></a>範例
若要拒絕單一電腦，請輸入：
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
若要拒絕所有電腦，請輸入：
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-delete-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
