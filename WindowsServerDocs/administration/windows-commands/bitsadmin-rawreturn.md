---
title: bitsadmin rawreturn
description: Bitsadmin rawreturn 命令的參考文章，此命令會傳回適用于剖析的資料。
ms.topic: reference
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d2be3712e4faf4803683ef32a10031894a913bd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026436"
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
