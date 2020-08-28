---
title: ktmutil
description: 用於啟動核心交易管理員公用程式之 ktmutil 命令的參考文章。
ms.topic: reference
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bc69964d0fbf7ad93a91b16f78ed86515e537b3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028256"
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