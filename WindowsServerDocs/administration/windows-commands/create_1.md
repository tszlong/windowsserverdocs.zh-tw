---
title: 建立
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70a8f53d9cb90fc36a76b11de2f93da71874617a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881269"
---
# <a name="create"></a>建立



啟動陰影複製建立程序，使用目前的內容和選項設定。 需要至少一個磁碟區陰影複製設定組中。

## <a name="syntax"></a>語法

```
create
```

## <a name="remarks"></a>備註

-   您必須新增至少一個磁碟區**新增磁碟區**命令之前，您可以使用**建立**命令。
-   您可以使用**開始備份**命令，以指定完整備份，而不是複製備份。
-   在執行後**建立**命令時，您可以使用**exec**命令來執行備份的重複資料刪除指令碼從陰影複製。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)