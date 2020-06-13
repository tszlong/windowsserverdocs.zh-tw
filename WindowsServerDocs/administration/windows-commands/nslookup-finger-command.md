---
title: nslookup 手指
description: Nslookup finger 命令的參考主題，它會與目前裝置上的 finger 伺服器連接。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11ea2bde-8ccb-4b87-bbad-231dd9e5e858
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6341e7297ea661fd9afd4f4b0bb5048d82099b94
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721491"
---
# <a name="nslookup-finger"></a>nslookup/finger

連接目前裝置上的 finger 伺服器。

## <a name="syntax"></a>語法

```
finger [<username>] [{[>] <filename> | [>>] <filename>}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<username>` | 指定要查閱之使用者的名稱。 |
| `<filename>` | 指定用來儲存輸出的檔案名。 您可以使用大於（ `>` ）和 double 大於（ `>>` ）字元，以一般方式重新導向輸出。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
