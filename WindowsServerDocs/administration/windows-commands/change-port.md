---
title: change port
description: '[變更埠] 命令的參考文章，其會列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0735c4c21ae8e321da1cfe31c2874f3dcfc540c7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922508"
---
# <a name="change-port"></a>change port

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱[Windows Server 中遠端桌面服務的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|-----------------|----------------------------------------|
| <portX>=<portY> | 將 COM 對應 `<*portX*>` 至`<*portY*>` |
| /d<portX> | 刪除 COM 的對應`<*portX*>` |
| /query | 顯示目前的埠對應。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 大部分的 MS-DOS 應用程式僅支援 COM1 到 COM4 的序列埠。 [**變更埠**] 命令會將序列埠對應至不同的埠號碼，允許不支援高編號 COM 埠的應用程式存取序列埠。 重新對應只適用于目前的會話，如果您登出會話然後再次登入，則不會保留。

- 使用不含任何參數的**變更埠**，以顯示可用的 COM 埠和其目前的對應。

## <a name="examples"></a>範例

- 若要將 COM12 對應到 COM1 以供以 MS-DOS 為基礎的應用程式使用，請輸入：

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
