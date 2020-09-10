---
title: bitsadmin rawreturn
description: Bitsadmin rawreturn 命令的參考文章，此命令會傳回適用于剖析的資料。
ms.topic: reference
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 286e84c16087cc000a6af29b3be53d529425dfde
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631213"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

傳回適用于剖析的資料。 一般來說，您會搭配使用此命令與 **/create** 和 **/get*** 參數，只接收值。 您必須在其他參數之前指定這個參數。

> [!NOTE]
> 此命令會從輸出中去除換行字元和格式。

## <a name="syntax"></a>語法

```
bitsadmin /rawreturn
```

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業狀態的原始資料：

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
