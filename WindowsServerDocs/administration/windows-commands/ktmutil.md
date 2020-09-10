---
title: ktmutil
description: 用於啟動核心交易管理員公用程式之 ktmutil 命令的參考文章。
ms.topic: reference
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 07067415406a0f8ba88d24362ec1b6252f8b88cc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627595"
---
# <a name="ktmutil"></a>ktmutil

啟動核心交易管理員公用程式。 如果使用時不含參數，則 **ktmutil** 會顯示可用的子命令。

## <a name="syntax"></a>語法

```
ktmutil list tms
ktmutil list transactions [{TmGUID}]
ktmutil resolve complete {TmGUID} {RmGUID} {EnGUID}
ktmutil resolve commit {TxGUID}
ktmutil resolve rollback {TxGUID}
ktmutil force commit {GUID}
ktmutil force rollback {GUID}
ktmutil forget
```

## <a name="examples"></a>範例


若要強制使用 GUID 311a9209-03f4-11dc-918f-00188b8f707b 認可 Indoubt 交易，請輸入：

```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)