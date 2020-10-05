---
title: wdsutil 刪除-autoadddevices
description: Wdsutil delete-autoadddevices 的參考文章，會刪除自動新增資料庫中暫止、已拒絕或已核准的電腦。
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 98c5aa38fd5394d4060cf9eba874d1be5cef845b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729966"
---
# <a name="wdsutil-delete-autoadddevices"></a>wdsutil 刪除-autoadddevices

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
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 核准-autoadddevices 命令](wdsutil-approve-autoadddevices.md)
- [wdsutil get-autoadddevices 命令](wdsutil-get-autoadddevices.md)
- [wdsutil 拒絕-autoadddevices 命令](wdsutil-reject-autoadddevices.md)
