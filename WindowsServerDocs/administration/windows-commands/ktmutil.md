---
title: ktmutil
description: 適用于 ktmutil 命令的參考主題，它會啟動核心交易管理員公用程式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ca26d2289e32d8eb618ab7cc9393678a16e318b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817308"
---
# <a name="ktmutil"></a>ktmutil

啟動核心交易管理員公用程式。 如果使用時不含參數，則**ktmutil**會顯示可用的子命令。

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


若要強制具有 GUID 311a9209-03f4-11dc-918f-00188b8f707b 的 Indoubt 交易認可，請輸入：

```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)