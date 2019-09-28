---
title: append
description: '的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdc4243bee8055888b023a56921cef757dda6b7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382747"
---
# <a name="append"></a>append



允許程式在指定的目錄中開啟資料檔案，就像是在目前目錄中的檔案一樣。 如果使用時不含參數， **append**會顯示附加的目錄清單。

> [!NOTE]
> Windows 10 不支援此命令。
>

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>參數

|     參數     |                                                                                 描述                                                                                 |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive >：] <Path> |                                                                 指定要附加的磁片磁碟機和目錄。                                                                  |
|       /x： on       |                                                  將附加的目錄套用至檔案搜尋和啟動應用程式。                                                  |
|      /x： off       |                                     只會將附加的目錄套用至開啟檔案的要求。</br>**/x： off**是預設設定。                                     |
|     /path： on      |                               將附加的目錄套用到已指定路徑的檔案要求。 **/path： on**是預設設定。                               |
|     /path： off     |                                                                    關閉 **/path： on**的效果。                                                                    |
|        /e         | 將附加的目錄清單複本儲存在名為 APPEND 的環境變數中。 只有在您啟動系統之後第一次使用 [**附加**] 時，才能使用 **/e** 。 |
|         ;         |                                                                     清除附加的目錄清單。                                                                     |
|        /?         |                                                                    在命令提示字元顯示說明。                                                                     |

## <a name="BKMK_examples"></a>典型

若要清除附加的目錄清單，請輸入：
```
append ;
```
若要將附加目錄的複本儲存至名為 APPEND 的環境變數，請輸入：
```
append /e
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
