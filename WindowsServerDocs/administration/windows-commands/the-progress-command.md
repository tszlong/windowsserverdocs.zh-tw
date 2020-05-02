---
title: progress
description: 進度的參考主題，會在命令執行時顯示進度。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fcb06304df22208cef013d73a4ebf0af1991f88
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721435"
---
# <a name="progress"></a>progress

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