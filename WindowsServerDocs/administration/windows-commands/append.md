---
title: 附加
description: Append 命令的參考文章，可讓程式在指定的目錄中開啟資料檔案，就像是在目前的目錄中一樣。
ms.topic: reference
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0002adabaa8c9cbd2235383d997c77670d33d522
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633471"
---
# <a name="append"></a>附加

允許程式在指定的目錄中開啟資料檔案，就像是在目前的目錄中一樣。 如果使用時不含參數， **append** 會顯示附加的目錄清單。

> [!NOTE]
> Windows 10 不支援這個命令。

## <a name="syntax"></a>語法

```
append [[<drive>:]<path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e]
append ;
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[\<drive>:]<path>` | 指定要附加的磁片磁碟機和目錄。 |
| /x：開啟 | 將附加的目錄套用至檔案搜尋和啟動應用程式。 |
| /x： off | 只會將附加的目錄套用至開啟檔案的要求。 **/X： off**選項是預設設定。 |
| /path：開啟 | 將附加的目錄套用至已指定路徑的檔案要求。 **/path： on** 是預設設定。 |
| /path： off | 關閉 **/path： on**的效果。 |
| /e | 將附加目錄清單的複本儲存在名為 APPEND 的環境變數中。 只有當您在啟動系統之後第一次使用 [**附加**] 時，才能使用 **/e** 。 |
| ; | 清除附加的目錄清單。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要清除附加的目錄清單，請輸入：

```
append ;
```

若要將附加目錄的複本儲存至名為 *append*的環境變數，請輸入：

```
append /e
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
