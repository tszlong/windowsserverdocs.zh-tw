---
title: bdehdcfg 大小
description: '**Bdehdcfg size**的 Windows 命令主題，它會在建立新的系統磁片磁碟機時指定系統磁碟分割的大小。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c72bdfd0b8bf4dfd0aa36885ceb7fd249a18c332
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851021"
---
# <a name="bdehdcfg-size"></a>bdehdcfg：大小

指定建立新的系統磁片磁碟機時，系統磁碟分割的大小。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<SizeinMB>` | 指出要用於新資料分割的 mb 數。 |

## <a name="remarks"></a>備註

如果您未指定大小，此工具會使用預設值 300 MB。 系統磁片磁碟機的大小下限為 100 MB。 如果您要將系統修復或其他系統工具儲存在系統磁碟分割上，則應該相應增加大小。

> [!NOTE]
> [**大小**] 命令無法與 [**目標**`<DriveLetter>` **merge** ] 命令結合。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列範例說明如何使用**size**命令，將 500 MB 配置給預設系統磁片磁碟機。

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)