---
title: 拒絕-AutoaddDevices
description: 拒絕 AutoaddDevices 的參考文章，它會拒絕等待系統管理核准的電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b678f7a9fc875dfeebf735475db3adfb7ad9ae7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932426"
---
# <a name="reject-autoadddevices"></a>拒絕-AutoaddDevices

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

拒絕正在等待系統管理核准的電腦。 啟用自動新增原則時，必須先進行系統管理核准，才會有未知的電腦（未預先設置的電腦）可以安裝映射。 您可以使用 [伺服器] 屬性頁的 [ **PXE 回應**] 索引標籤來啟用此原則。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/RequestId： <要求識別碼 &#124; 所有>|指定指派給擱置電腦的要求識別碼。 若要拒絕所有擱置中的電腦，請指定**all**。|
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
