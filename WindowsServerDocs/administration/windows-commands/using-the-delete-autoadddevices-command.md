---
title: 刪除-AutoaddDevices
description: AutoaddDevices 的參考文章，此文章會刪除自動新增資料庫中暫止、已拒絕或已核准的電腦。
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50160d9b015861f2e32f7ae693cbd99d6eccb020
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038186"
---
# <a name="delete-autoadddevices"></a>刪除-AutoaddDevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從自動新增資料庫刪除擱置、拒絕或核准的電腦。 此資料庫會將這些電腦的相關資訊儲存在伺服器上。

## <a name="syntax"></a>語法
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Devicetype： {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|指定要從資料庫刪除的電腦類型。 這可以是下列三種類型的其中一種：<p>-   **PendingDevices** 會傳回資料庫中狀態為 [擱置] 的所有電腦。<br />-   **RejectedDevices** 會傳回資料庫中狀態為 [已拒絕] 的所有電腦。<br />-   **ApprovedDevices** 會傳回狀態為 [已核准] 的所有電腦。|
## <a name="examples"></a>範例
若要刪除所有已拒絕的電腦，請輸入：
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
若要刪除所有已核准的電腦，請輸入：
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-get-autoadddevices-command.md) 
[使用 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
