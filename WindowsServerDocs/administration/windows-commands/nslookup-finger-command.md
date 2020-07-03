---
title: nslookup 手指
description: Nslookup finger 命令的參考文章，它會與目前裝置上的 finger 伺服器連接。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11ea2bde-8ccb-4b87-bbad-231dd9e5e858
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 413dc1a38b4fa7ee7bec28991547b5e0b5ef6fb9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934760"
---
# <a name="nslookup-finger"></a>nslookup/finger

連接目前裝置上的 finger 伺服器。

## <a name="syntax"></a>語法

```
finger [<username>] [{[>] <filename> | [>>] <filename>}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<username>` | 指定要查閱之使用者的名稱。 |
| `<filename>` | 指定用來儲存輸出的檔案名。 您可以使用大於（ `>` ）和 double 大於（ `>>` ）字元，以一般方式重新導向輸出。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
