---
title: 建立
description: Create 的參考主題，它會使用目前的內容和選項設定來啟動陰影複製建立程式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfddbebd5744d8cd222d67e46690ce8b5d2e0fde
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716840"
---
# <a name="create"></a>建立

使用目前的內容和選項設定，啟動陰影複製建立進程。 在陰影複製組中至少需要一個磁片區。

## <a name="syntax"></a>語法

```
create
```

## <a name="remarks"></a>備註

-   您必須使用 [**新增磁片**區] 命令新增至少一個磁片區，才能使用**create**命令。
-   您可以使用 [**開始備份**] 命令來指定完整備份，而不是複本備份。
-   執行**create**命令之後，您可以使用**exec**命令從陰影複製執行備份的複製腳本。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)