---
title: wdsutil 刪除-autoadddevices
description: Wdsutil autoadddevices 命令的參考文章，此命令會從自動新增資料庫中刪除暫止、已拒絕或已核准的電腦。
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 610b57c7db49c5d8cf354502634cf82212d52c19
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524813"
---
# <a name="wdsutil-delete-autoadddevices"></a>wdsutil 刪除-autoadddevices

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從自動新增資料庫刪除擱置、拒絕或核准的電腦。 此資料庫會將這些電腦的相關資訊儲存在伺服器上。

## <a name="syntax"></a>語法

```
wdsutil /delete-AutoaddDevices [/Server:<Servername>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| Devicetype`{PendingDevices|RejectedDevices|ApprovedDevices}` | 指定要從資料庫刪除的電腦類型。 此類型可以是 **PendingDevices**，它會傳回資料庫中狀態為 [擱置]、[ **RejectedDevices**] 的所有電腦，它會傳回資料庫中狀態為 [已拒絕] 或 [ **ApprovedDevices**] 的所有電腦，其會傳回狀態為 [已核准] 的所有電腦。 |

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

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
