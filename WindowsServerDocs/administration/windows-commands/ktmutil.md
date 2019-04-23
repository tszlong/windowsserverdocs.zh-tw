---
title: ktmutil
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7af47ab8697345b81018c2539e0c451359bd2a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826459"
---
# <a name="ktmutil"></a>ktmutil



啟動核心交易管理員公用程式。 如果未指定參數，使用**ktmutil**會顯示可用的子命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ktmutil list tms 
ktmutil list transactions [{TmGuid}]
ktmutil resolve complete {TmGuid} {RmGuid} {EnGuid}
ktmutil resolve commit {TxGuid}
ktmutil resolve rollback {TxGuid}
ktmutil force commit {??Guid}
ktmutil force rollback {??Guid}
ktmutil forget
```

## <a name="parameters"></a>參數

## <a name="remarks"></a>備註

## <a name="BKMK_examples"></a>範例

若要強制認可 GUID 311a9209-03f4-11dc-918f-00188b8f707b Indoubt 交易，請輸入：
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)