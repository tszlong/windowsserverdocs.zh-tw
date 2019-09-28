---
title: clean
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f871ad1d13e06bf0cbb886ba64a52e7a55a9a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379319"
---
# <a name="clean"></a>clean

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart clean 命令會從具有焦點的磁片中移除任何和所有磁碟分割或磁片區格式。
## <a name="syntax"></a>語法
```
clean [all]
```
## <a name="parameters"></a>參數

| 參數 |                                                        描述                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    全部    | 指定磁片上的每個和每個磁區都設定為零，這會完全刪除磁片上包含的所有資料。 |

## <a name="remarks"></a>備註
- 在主開機記錄（MBR）磁片上，只會覆寫 MBR 分割資訊和隱藏磁區資訊。
- 在 GUID 磁碟分割表格（gpt）磁片上，會覆寫 gpt 分割資訊（包括保護 MBR）。 沒有隱藏的磁區資訊。
- 必須選取磁片，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。
  ## <a name="BKMK_examples"></a>典型
  若要從選取的磁片中移除所有格式，請輸入：
  ```
  clean
  ```

[清除磁片](https://technet.microsoft.com/library/hh848661.aspx)
