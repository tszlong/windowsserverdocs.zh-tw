---
title: detail disk
description: 詳細資料磁片命令的參考文章，它會顯示所選磁片的屬性和該磁片上的磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bb5ec5a51f16aecf1b8ee35c78736f1791d3106
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929465"
---
# <a name="detail-disk"></a>detail disk

顯示所選磁碟的內容以及該磁碟上磁碟區。 開始之前，您必須先選取磁片，此作業才會成功。 使用 [[選取磁片](select-disk.md)] 命令來選取磁片，並將焦點移至它。 如果您選取虛擬硬碟（VHD），此命令會將磁片的匯流排類型顯示為 [*虛擬*]。

## <a name="syntax"></a>語法

```
detail disk
```

## <a name="examples"></a>範例

若要查看所選磁片的內容，以及磁片中磁片區的相關資訊，請輸入：

```
detail disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [詳細資料命令](detail.md)
