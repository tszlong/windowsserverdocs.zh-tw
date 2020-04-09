---
title: bdehdcfg newdriveletter
description: 適用于**bdehdcfg newdriveletter**的 Windows 命令主題，它會將新的磁碟機號指派給用來作為系統磁片磁碟機的磁片磁碟機部分。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a8757e7d0684912525817708fbe34953b049582
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851051"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg： newdriveletter

將新的磁碟機號指派給用來做為系統磁片磁碟機的磁片磁碟機部分。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------| ----------- |
|`<DriveLetter>`|定義將指派給指定目標磁片磁碟機的磁碟機號。|

## <a name="remarks"></a>備註

最佳做法是，建議您不要將磁碟機號指派給您的系統磁片磁碟機。

## <a name="examples"></a><a name="BKMK_Examples"></a>典型

下列範例顯示指派磁碟機號 P 的預設磁片磁碟機。

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)