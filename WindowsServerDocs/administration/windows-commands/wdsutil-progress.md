---
title: wdsutil 進度
description: Wdsutil 進度的參考文章，會在命令執行時顯示進度。
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a7ddc18db35b110c8b5c513f798e3408aafd93f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729869"
---
# <a name="wdsutil-progress"></a>wdsutil/progress

顯示命令執行時的進度。 您可以使用 **/progress** 搭配您所執行的任何其他 wdsutil 命令。 如果您想要為此命令開啟詳細資訊記錄，您必須直接在**wdsutil**之後指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
wdsutil /progress <commands>
```

## <a name="examples"></a>範例

若要初始化伺服器並顯示進度，請輸入：

```
wdsutil /verbose /progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)