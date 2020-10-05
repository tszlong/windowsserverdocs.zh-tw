---
title: wdsutil 拒絕-autoadddevices
description: Wdsutil 拒絕 autoadddevices 的參考文章，會拒絕正在等待系統管理核准的電腦。
ms.topic: reference
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2cb970ce6b51933e72f208f6fbcc50bcb7b3164b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729868"
---
# <a name="wdsutil-reject-autoadddevices"></a>wdsutil 拒絕-autoadddevices

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
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 核准-autoadddevices 命令](wdsutil-approve-autoadddevices.md)
- [wdsutil delete-autoadddevices 命令](wdsutil-delete-autoadddevices.md)
- [wdsutil get-autoadddevices 命令](wdsutil-get-autoadddevices.md)
