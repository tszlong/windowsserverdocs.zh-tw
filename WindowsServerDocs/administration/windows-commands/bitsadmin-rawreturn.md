---
title: bitsadmin rawreturn
description: Bitsadmin rawreturn 命令的參考文章，它會傳回適用于剖析的資料。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b537b21678c100364406d4c59eaa02efd143e21
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926468"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

傳回適用于剖析的資料。 一般來說，您會搭配使用此命令與 **/create**和 **/get*** 參數，只接收值。 您必須在其他參數之前指定此參數。

> [!NOTE]
> 此命令會從輸出中去除分行符號和格式。

## <a name="syntax"></a>語法

```
bitsadmin /rawreturn
```

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業狀態的原始資料：

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
