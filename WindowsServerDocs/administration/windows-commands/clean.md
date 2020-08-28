---
title: clean
description: Diskpart clean 命令的參考文章，此命令會從磁片移除具有焦點的所有磁碟分割或磁片區格式化。
ms.topic: reference
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7fd0ccfef5a15e3289b8d9a3b2b1f0b62bfe76a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025972"
---
# <a name="clean"></a>clean

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從磁片移除具有焦點的所有磁碟分割或磁片區格式化。

>[!NOTE]
> 如需此命令的 PowerShell 版本，請參閱 [清除磁片命令](/powershell/module/storage/clear-disk)。

## <a name="syntax"></a>語法

```
clean [all]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| all | 指定磁片上的每個和每個磁區都設定為零，以完全刪除磁片上包含的所有資料。 |

#### <a name="remarks"></a>備註

- 在主開機記錄 (MBR) 磁片上，只會覆寫 MBR 分割資訊和隱藏的磁區資訊。

- 在 GUID 磁碟分割表格 (gpt) 磁片上，會覆寫 gpt 磁碟分割資訊（包括保護 MBR）。 沒有隱藏的磁區資訊。

- 必須選取磁片，此操作才能成功。 使用 [ **選取磁片** ] 命令來選取磁片，並將焦點移至該磁片。

## <a name="examples"></a>範例

若要移除所選磁片的所有格式設定，請輸入：

```
clean
```

## <a name="additional-references"></a>其他參考資料

- [清除磁片命令](/powershell/module/storage/clear-disk)

- [命令列語法關鍵](command-line-syntax-key.md)
