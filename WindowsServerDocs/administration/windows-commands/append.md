---
title: append
description: '適用於 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe641e1336c163b5e98421a5fc32f8dbe64023b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435320"
---
# <a name="append"></a>append



可讓程式開啟資料檔中指定的目錄，如同它們是在目前的目錄。 如果未指定參數，使用**附加**顯示附加的目錄清單。

> [!NOTE]
> Windows 10 中不支援此命令。
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
| [\<Drive>:]<Path> |                                                                 指定磁碟機和要附加的目錄。                                                                  |
|       / x： 上       |                                                  搜尋檔案並啟動應用程式，適用於附加的目錄。                                                  |
|      / x： 關閉       |                                     僅適用於附加的目錄開啟檔案的要求。</br>**/ x： 關閉**是預設設定。                                     |
|     /path:on      |                               適用於已指定路徑的檔案要求附加的目錄。 **/path： 上**是預設設定。                               |
|     /path:off     |                                                                    關閉的效果 **/path： 上**。                                                                    |
|        /e         | 名為附加的環境變數中儲存一份附加的目錄清單。 **/e**可能使用只有您使用第一次**附加**啟動您的系統之後。 |
|         ;         |                                                                     清除附加的目錄清單。                                                                     |
|        /?         |                                                                    在命令提示字元顯示說明。                                                                     |

## <a name="BKMK_examples"></a>範例

若要清除附加的目錄清單，請輸入：
```
append ;
```
若要儲存一份附加的目錄，以名為附加的環境變數，請輸入：
```
append /e
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
