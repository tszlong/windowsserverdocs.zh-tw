---
title: detail volume
description: 詳細資料磁片區的參考主題，顯示目前磁片區所在的磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2958c82b1dfc3b99d0e15690ef9857e7d83b244f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719612"
---
# <a name="detail-volume"></a>detail volume

顯示目前磁碟區所位於的磁碟。

## <a name="syntax"></a>語法

```
detail volume
```

## <a name="remarks"></a>備註

-   必須選取一個磁碟區，這項操作才能繼續。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。
-   磁片區詳細資料不適用於唯讀磁片區，例如 DVD-ROM 或 CD-ROM 光碟機。

## <a name="examples"></a>範例

若要查看目前磁片區所在的所有磁片，請輸入：
```
detail volume
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

