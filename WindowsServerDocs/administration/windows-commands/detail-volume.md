---
title: detail volume
description: 詳細資料磁片區的 Windows 命令主題，會顯示目前磁片區所在的磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0441beba769066c593e77b55b9266918e5f778
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846315"
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

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看目前磁片區所在的所有磁片，請輸入：
```
detail volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

