---
title: detail disk
description: 詳細資料磁片的參考主題，它會顯示所選磁片的內容，以及該磁片上的磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a746506d6c9609e3214dbd48e5fa91f52d16ab4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710511"
---
# <a name="detail-disk"></a>detail disk

顯示所選磁碟的內容以及該磁碟上磁碟區。

## <a name="syntax"></a>語法

```
detail disk
```

## <a name="remarks"></a>備註

-   必須選取磁片，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。
-   如果選取的磁片是虛擬硬碟（VHD），**詳細資料磁片**會將磁片的匯流排類型報告為 [虛擬]。

## <a name="examples"></a>範例

若要查看所選磁片的內容，以及磁片中磁片區的相關資訊，請輸入：
```
detail disk
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

