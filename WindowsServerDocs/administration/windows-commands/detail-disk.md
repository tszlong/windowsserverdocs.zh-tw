---
title: 詳細資料磁碟
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819099"
---
# <a name="detail-disk"></a>詳細資料磁碟



顯示所選磁碟的內容以及該磁碟上的磁碟區。

## <a name="syntax"></a>語法

```
detail disk
```

## <a name="remarks"></a>備註

-   必須選取磁碟，這項作業才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。
-   如果所選的磁碟的虛擬硬碟 (VHD)，**詳細資料磁碟**報告為虛擬磁碟的匯流排類型。

## <a name="BKMK_examples"></a>範例

若要查看的屬性選取的磁碟和磁碟中的磁碟區的相關資訊，請輸入：
```
detail disk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

