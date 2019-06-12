---
title: clean
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd5eb2ec1bde4523eb6f0f919e09b9711b2654fb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434324"
---
# <a name="clean"></a>clean

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Diskpart clean 命令會移除所有的資料分割或從具有焦點的磁碟格式化的磁碟區。
## <a name="syntax"></a>語法
```
clean [all]
```
## <a name="parameters"></a>參數

| 參數 |                                                        描述                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    全部    | 指定在磁碟上的每個磁區設為零，完全刪除磁碟上包含的所有資料。 |

## <a name="remarks"></a>備註
- 主開機記錄 (MBR) 磁碟上只有 MBR 磁碟分割資訊及隱藏磁區資訊會覆寫。
- 在 GUID 磁碟分割表格 (gpt) 磁碟上，會覆寫 gpt 磁碟分割資訊，包括保護的 MBR。 沒有隱藏的磁區資訊。
- 必須選取磁碟，這項作業才會成功。 使用**選取磁碟**命令來選取磁碟，並將焦點移到它。
  ## <a name="BKMK_examples"></a>範例
  若要移除所選磁碟的所有格式，請輸入：
  ```
  clean
  ```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
