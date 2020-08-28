---
title: change port
description: 變更埠命令的參考文章，此命令會列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8014ba67b2c4383aa56a6fce5eb486bbccfba7e7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031156"
---
# <a name="change-port"></a>change port

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|-----------------|----------------------------------------|
| <portX>=<portY> | 將 COM 對應 `<*portX*>` 至 `<*portY*>` |
| /d <portX> | 刪除 COM 的對應 `<*portX*>` |
| /query | 顯示目前的埠對應。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 大部分的 MS-DOS 應用程式僅支援透過 COM4 序列埠的 COM1。 **變更埠**命令會將序列埠對應至不同的埠號碼，讓不支援高編號 COM 埠的應用程式存取序列埠。 重新對應僅適用于目前的會話，如果您登出會話，然後重新登入，則不會保留。

- 使用不含任何參數的 **變更埠** 來顯示可用的 COM 埠及其目前的對應。

## <a name="examples"></a>範例

- 若要將 COM12 對應至 COM1 以供以 MS-DOS 為基礎的應用程式使用，請輸入：

  ```
  change port com12=com1
  ```

- 若要顯示目前的埠對應，請輸入：

  ```
  change port /query
  ```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [變更命令](change.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
