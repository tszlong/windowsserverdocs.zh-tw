---
title: bitsadmin getreplyfilename
description: Bitsadmin getreplyfilename 命令的參考文章，它會取得包含工作之伺服器上傳-回復的檔案路徑。
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 213df5c5dcef57db8f1cdc2b26c90dbdc124007c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893881"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

取得包含工作之伺服器上傳-回復的檔案路徑。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的上傳-回復檔案名：

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
