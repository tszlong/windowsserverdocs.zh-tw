---
title: 使用進度命令
description: 進度的參考文章，會在命令執行時顯示進度。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9284c7330adfbad0115b5b7f6bbab034b42fda4
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519627"
---
# <a name="using-the-progress-command"></a>使用進度命令

顯示執行命令時的進度。 您可以使用 **/progress**搭配您執行的任何其他 WDSUTIL 命令。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>範例

若要初始化伺服器並顯示進度，請輸入：
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```