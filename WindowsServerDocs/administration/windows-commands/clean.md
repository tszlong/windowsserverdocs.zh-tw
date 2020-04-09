---
title: clean
description: 適用于 Diskpart clean 命令的 Windows 命令主題，會從具有焦點的磁片中移除任何和所有磁碟分割或磁片區格式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d573a0480c24a2a622618197dea7dccaeddac271
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847751"
---
# <a name="clean"></a>clean

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart clean 命令會從具有焦點的磁片中移除任何和所有磁碟分割或磁片區格式。

## <a name="syntax"></a>語法
```
clean [all]
```
### <a name="parameters"></a>參數

| 參數 |                                                        描述                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    全部    | 指定磁片上的每個和每個磁區都設定為零，這會完全刪除磁片上包含的所有資料。 |

## <a name="remarks"></a>備註
- 在主要開機記錄 (MBR) 磁碟上，僅覆寫 MBR 磁碟分割資訊及隱藏磁區資訊。
- 在 GUID 磁碟分割表格（gpt）磁片上，會覆寫 gpt 分割資訊（包括保護 MBR）。 沒有隱藏的磁區資訊。
- 必須選取磁片，此操作才能成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="examples"></a><a name=BKMK_examples></a>典型
  若要從選取的磁片中移除所有格式，請輸入：
  ```
  clean
  ```

## <a name="additional-references"></a>其他參考資料
[清除磁片](https://technet.microsoft.com/library/hh848661.aspx)
